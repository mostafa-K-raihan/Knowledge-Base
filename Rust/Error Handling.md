### Panic
Rust does [`panic`](http://doc.rust-lang.org/std/macro.panic!.html) whenever it come across something unexpected. (`panic` causes the current task to unwind, and in most cases, the entire program aborts.) Here’s an example:


```rust
// Guess a number between 1 and 10.
// If it matches the number I had in mind, return true. Else, return false.
fn guess(n: i32) -> bool {
    if n < 1 || n > 10 {
        panic!("Invalid number: {}", n);
    }
    n == 5
}

fn main() {
    guess(11);
}
```

This will print in the console
```sh
thread '<main>' panicked at 'Invalid number: 11', src/bin/panic-simple.rs:5
```

Here’s another example that is slightly less contrived. A program that accepts an integer as an argument, doubles it and prints it.

unwrap-double

```rust
use std::env;

fn main() {
    let mut argv = env::args();
    let arg: String = argv.nth(1).unwrap(); // error 1
    let n: i32 = arg.parse().unwrap(); // error 2
    println!("{}", 2 * n);
}

// $ cargo run --bin unwrap-double 5
// 10
```

If you give this program zero arguments (error 1) or if the first argument isn’t an integer (error 2), the program will panic just like in the first example.

### Unwrapping explained

In the previous example the program would simply panic if it reached one of the two error conditions, yet, the program does not include an explicit call to `panic` like the first example. This is because the panic is embedded in the calls to `unwrap`.

To *unwrap* something in Rust is to say, *Give me the result of the computation, and if there was an error, just panic and stop the program*  

### The `Option` type

The `Option` type is [defined in the standard library](http://doc.rust-lang.org/std/option/enum.Option.html):

option-def

```rust
enum Option<T> {
    None,
    Some(T),
}
```

The `Option` type is a way to use Rust’s type system to express the _possibility of absence_. Encoding the possibility of absence into the type system is an important concept because it will cause the compiler to force the programmer to handle that absence. Let’s take a look at an example that tries to find a character in a string:

option-ex-string-find

```rust
// Searches `haystack` for the Unicode character `needle`. If one is found, the
// byte offset of the character is returned. Otherwise, `None` is returned.
fn find(haystack: &str, needle: char) -> Option<usize> {
    for (offset, c) in haystack.char_indices() {
        if c == needle {
            return Some(offset);
        }
    }
    None
}
```

(Pro-tip: don’t use this code. Instead, use the [`find`](http://doc.rust-lang.org/std/primitive.str.html#method.find) method from the standard library.)

Notice that when this function finds a matching character, it doesn’t just return the `offset`. Instead, it returns `Some(offset)`. `Some` is a variant or a _value constructor_ for the `Option` type. You can think of it as a function with the type `fn<T>(value: T) -> Option<T>`. Correspondingly, `None` is also a value constructor, except it has no arguments. You can think of `None` as a function with the type `fn<T>() -> Option<T>`.

This might seem like much ado about nothing, but this is only half of the story. The other half is _using_ the `find` function we’ve written. Let’s try to use it to find the extension in a file name.

option-ex-string-find
```rust
fn main_find() {
    let file_name = "foobar.rs";
    match find(file_name, '.') {
        None => println!("No file extension found."),
        Some(i) => println!("File extension: {}", &file_name[i+1..]),
    }
}
```

This code uses [pattern matching](http://doc.rust-lang.org/1.0.0-beta.5/book/patterns.html) to do _case analysis_ on the `Option<usize>` returned by the `find` function. In fact, case analysis is the only way to get at the value stored inside an `Option<T>`. This means that you, as the programmer, must handle the case when an `Option<T>` is `None` instead of `Some(t)`.

But wait, what about `unwrap` used in unwrap-double There was no case analysis there! Instead, the case analysis was put inside the `unwrap` method for you. You could define it yourself if you want:

option-def-unwrap

```rust
enum Option<T> {
    None,
    Some(T),
}

impl<T> Option<T> {
    fn unwrap(self) -> T {
        match self {
            Option::Some(val) => val,
            Option::None =>
              panic!("called `Option::unwrap()` on a `None` value"),
        }
    }
}
```

The `unwrap` method _abstracts away the case analysis_. This is precisely the thing that makes `unwrap` ergonomic to use. Unfortunately, that `panic!` means that `unwrap` is not composable: it is the bull in the china shop.

#### Composing `Option<T>` values

In [`option-ex-string-find`](https://blog.burntsushi.net/rust-error-handling/#code-option-ex-string-find-2) we saw how to use `find` to discover the extension in a file name. Of course, not all file names have a `.` in them, so it’s possible that the file name has no extension. This _possibility of absence_ is encoded into the types using `Option<T>`. In other words, the compiler will force us to address the possibility that an extension does not exist. In our case, we just print out a message saying as such.

Getting the extension of a file name is a pretty common operation, so it makes sense to put it into a function:

option-ex-string-find

```rust
// Returns the extension of the given file name, where the extension is defined
// as all characters succeeding the first `.`.
// If `file_name` has no `.`, then `None` is returned.
fn extension_explicit(file_name: &str) -> Option<&str> {
    match find(file_name, '.') {
        None => None,
        Some(i) => Some(&file_name[i+1..]),
    }
}
```

(Pro-tip: don’t use this code. Use the [`extension`](http://doc.rust-lang.org/std/path/struct.Path.html#method.extension) method in the standard library instead.)

The code stays simple, but the important thing to notice is that the type of `find` forces us to consider the possibility of absence. This is a good thing because it means the compiler won’t let us accidentally forget about the case where a file name doesn’t have an extension. On the other hand, doing explicit case analysis like we’ve done in `extension_explicit` every time can get a bit tiresome.

In fact, the case analysis in `extension_explicit` follows a very common pattern: _map_ a function on to the value inside of an `Option<T>`, unless the option is `None`, in which case, just return `None`.

Rust has parametric polymorphism, so it is very easy to define a combinator that abstracts this pattern:

option-map

```rust
fn map<F, T, A>(option: Option<T>, f: F) -> Option<A> where F: FnOnce(T) -> A {
    match option {
        None => None,
        Some(value) => Some(f(value)),
    }
}
```