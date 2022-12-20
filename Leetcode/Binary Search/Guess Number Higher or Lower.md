## Problem statement
We are playing the Guess Game. The game is as follows:

I pick a number from `1` to `n`. You have to guess which number I picked.

Every time you guess wrong, I will tell you whether the number I picked is higher or lower than your guess.

You call a pre-defined API `int guess(int num)`, which returns three possible results:

-   `-1`: Your guess is higher than the number I picked (i.e. `num > pick`).
-   `1`: Your guess is lower than the number I picked (i.e. `num < pick`).
-   `0`: your guess is equal to the number I picked (i.e. `num == pick`).

Return _the number that I picked_.

**Example 1**
**Input:** n = 10, pick = 6
**Output:** 6

**Example 2:**

**Input:** n = 1, pick = 1
**Output:** 1

**Example 3:**

**Input:** n = 2, pick = 1
**Output:** 1

**Constraints:**

-   1 <= n <= 2<sup>31</sup> - 1
-   `1 <= pick <= n`

## Code
```python
# The guess API is already defined for you.
# @param num, your guess
# @return -1 if num is higher than the picked number
#          1 if num is lower than the picked number
#          otherwise return 0
# def guess(num: int) -> int:

class Solution:
    def guessNumber(self, n: int) -> int:
        start, end = 1, n
        
        while start <= end:
            mid = start + (end - start) // 2
            
            x = guess(mid)
            
            if x < 0:
                end = mid - 1
            elif x > 0:
                start = mid + 1
            else:
                return mid
```

```rust
/** 
 * Forward declaration of guess API.
 * @param  num   your guess
 * @return 	     -1 if num is higher than the picked number
 *			      1 if num is lower than the picked number
 *               otherwise return 0
 * unsafe fn guess(num: i32) -> i32 {}
 */

impl Solution {
    unsafe fn guessNumber(n: i32) -> i32 {
        let (mut start, mut end, mut mid): (i32, i32, i32) = (1, n, 0);
        while start <= end {
            mid = start + ((end - start) / 2);
            let x = guess(mid);
            
            if x < 0 {
                end = mid - 1;
            } else if x > 0 {
                start = mid + 1;
            } else {
                break;
            }
        }
        mid
    }
}
```