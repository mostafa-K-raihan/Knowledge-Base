Two strings are considered **close** if you can attain one from the other using the following operations:

-   Operation 1: Swap any two **existing** characters.
    -   For example, `abcde -> aecdb`
-   Operation 2: Transform **every** occurrence of one **existing** character into another **existing** character, and do the same with the other character.
    -   For example, `aacabb -> bbcbaa` (all `a`'s turn into `b`'s, and all `b`'s turn into `a`'s)

You can use the operations on either string as many times as necessary.

Given two strings, `word1` and `word2`, return `true` _if_ `word1` _and_ `word2` _are **close**, and_ `false` _otherwise._

**Example 1:**

**Input:** word1 = "abc", word2 = "bca"
**Output:** true
**Explanation:** You can attain word2 from word1 in 2 operations.
Apply Operation 1: "abc" -> "acb"
Apply Operation 1: "acb" -> "bca"

**Example 2:**

**Input:** word1 = "a", word2 = "aa"
**Output:** false
**Explanation:** It is impossible to attain word2 from word1, or vice versa, in any number of operations.

**Example 3:**

**Input:** word1 = "cabbba", word2 = "abbccc"
**Output:** true
**Explanation:** You can attain word2 from word1 in 3 operations.
Apply Operation 1: "cabbba" -> "caabbb"
`Apply Operation 2: "`caabbb" -> "baaccc"
Apply Operation 2: "baaccc" -> "abbccc"

**Constraints:**

-   `1 <= word1.length, word2.length <= 105`
-   `word1` and `word2` contain only lowercase English letters.

```rust
impl Solution {
    pub fn close_strings(word1: String, word2: String) -> bool {
    // if the length does not match no need to go further
        if (word1.len() != word2.len()) {
            return false;
        }

		// create 2 hash maps, and init with 0, with 26 length
        let (mut freq1, mut freq2) = (vec![0; 26], vec![0; 26]);

		// convert words to bytes array
        let (word1, word2) = (word1.as_bytes(), word2.as_bytes());

        for i in 0..word1.len() {
	       // compute the index 
            let (a, b) = ((word1[i] - b'a') as usize, (word2[i] - b'a') as usize);

            freq1[a] +=1;
            freq2[b] +=1;
        }


		// if one map contains, but other does not contain a specific char, we cant swap or transform
        for i in 0..26 {
            if ((freq1[i] == 0 && freq2[i] != 0) || (freq1[i] != 0 && freq2[i] == 0)) {
                return false;
            }
        }

  

        freq1.sort();
        freq2.sort();

		// once the character counts are sorted
		// we can map one freq. char to other freq. char
        for i in 0..26 {
            if (freq1[i] != freq2[i]) {
                return false;
            }
        }
        return true;

    }

}
```