Given an integer `n`, return _the least number of perfect square numbers that sum to_ `n`.

A **perfect square** is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, `1`, `4`, `9`, and `16` are perfect squares while `3` and `11` are not.

**Example 1:**

**Input:** n = 12
**Output:** 3
**Explanation:** 12 = 4 + 4 + 4.

**Example 2:**

**Input:** n = 13
**Output:** 2
**Explanation:** 13 = 4 + 9.

**Constraints:**

-   1 <= n <= 10<sup>4</sup>

```python
class Solution:
    def numSquares(self, n: int) -> int:
        # you have to understand that it is a dp
        # not greedy
        # counter example is 18, where choosing 9 first wil lead fewer number, than 16
        dp = [inf for _ in range(n+1)]
        dp[0] = 0
        
        # first gather all perfect squares upto n
        ps, x = [], int(sqrt(n))
        for i in range(x+1):
            ps.append(i*i)
            
        for i in range(1, n+1):
            count = inf
            for j in ps:
                # if the ps can be chosen, then choose it
                # similar to coin change
                # where choosing 1 coin will reduce the problem size (n-coin value)
                # keep track of the min value
                if j <= i:
                    count = min(count, 1 + dp[i - j])
                else:
                    break
            dp[i] = count
        
        return dp[n]
        
```