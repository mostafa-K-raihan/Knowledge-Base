A binary string is monotone increasing if it consists of some number of `0`'s (possibly none), followed by some number of `1`'s (also possibly none).

You are given a binary string `s`. You can flip `s[i]` changing it from `0` to `1` or from `1` to `0`.

Return _the minimum number of flips to make_ `s` _monotone increasing_.

**Example 1:**

**Input:** s = "00110"
**Output:** 1
**Explanation:** We flip the last digit to get 00111.

**Example 2:**

**Input:** s = "010110"
**Output:** 2
**Explanation:** We flip to get 011111, or alternatively 000111.

**Example 3:**

**Input:** s = "00011000"
**Output:** 2
**Explanation:** We flip to get 00000000.

**Constraints:**

-   `1 <= s.length <= 105`
-   `s[i]` is either `'0'` or `'1'`.

```python
class Solution:
    def minFlipsMonoIncr(self, s: str) -> int:
	   '''
	   We need to find a dividing line (|), before that line
	   all the 1's need to be 0, and after that, all the 0's need to be 1
	   and the total flip needed for that | is sum of left and right

		so we start off from the very beginning, | 001001 assuming all the zeros after 
		the | needs to be one, so counted total 0s as min_flip
		and move to the right, if we encounter a 0 we reduce the flip or unflip it, since before | ther should be a zero 0 | 01001
		and if we encounter a 1, we increase a flip, because before | there should be a 1
	   001 | 001
	   ..^ this need to be fliped, so increase the count

		keep track of the min_flip
	   '''
        min_flip = flip_so_far = s.count('0')

        for c in s:
            if c == '1':
                flip_so_far += 1

            else:
                flip_so_far -= 1
                min_flip = min(min_flip, flip_so_far)

        return min_flip
```