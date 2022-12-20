Given two strings `text1` and `text2`, return _the length of their longest **common subsequence**._ If there is no **common subsequence**, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

-   For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

**Example 1:**

**Input:** text1 = "abcde", text2 = "ace" 
**Output:** 3  
**Explanation:** The longest common subsequence is "ace" and its length is 3.

**Example 2:**

**Input:** text1 = "abc", text2 = "abc"
**Output:** 3
**Explanation:** The longest common subsequence is "abc" and its length is 3.

**Example 3:**

**Input:** text1 = "abc", text2 = "def"
**Output:** 0
**Explanation:** There is no such common subsequence, so the result is 0.

**Constraints:**

-   `1 <= text1.length, text2.length <= 1000`
-   `text1` and `text2` consist of only lowercase English characters.


```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:


        first, second = len(text1), len(text2)
        # recursive       
        # dp = {}

        # def helper(i, j):
        #     if i >= first or j >= second:
        #         return 0

        #     if (i, j) in dp:
        #         return dp[(i, j)]

        #     if text1[i] == text2[j]:
        #         return 1 + helper(i+1, j+1)
            
        #     dp[(i, j)] = max(helper(i+1, j), helper(i, j+1))
        #     return dp[(i, j)]
        
        # return helper(0, 0)

        # iterative

        dp = [[0 for x in range(second + 1)] for y in range(first + 1)]

        for i in range(1, first+1):
            for j in range(1, second+1):
                dp[i][j] = (1 + dp[i-1][j-1]) if text1[i-1] == text2[j-1] else max(dp[i-1][j], dp[i][j-1])

        # print(dp)
        
        return dp[first][second]
```