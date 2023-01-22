### Length
```rust
let s = "Rust is cool";
print!("{}", s.len())
```
### Iterate over a string
```rust
let s = "Rust is cool";
for i in s.chars() {
	print!("{}", i);
}
```

### Iterate over a string with indices
```rust
let s = "Rust is cool";
for (i, c) in s.chars().enumerate() {
	// character c and index i
}
```
