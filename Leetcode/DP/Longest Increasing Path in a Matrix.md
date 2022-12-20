Given an `m x n` integers `matrix`, return _the length of the longest increasing path in_ `matrix`.

From each cell, you can either move in four directions: left, right, up, or down. You **may not** move **diagonally** or move **outside the boundary** (i.e., wrap-around is not allowed).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/05/grid1.jpg)

**Input:** matrix = [[9,9,4],[6,6,8],[2,1,1]]
**Output:** 4
**Explanation:** The longest increasing path is `[1, 2, 6, 9]`.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/27/tmp-grid.jpg)

**Input:** matrix = [[3,4,5],[3,2,6],[2,2,1]]
**Output:** 4
**Explanation:** The longest increasing path is `[3, 4, 5, 6]`. Moving diagonally is not allowed.

**Example 3:**

**Input:** matrix = [[1]]
**Output:** 1

**Constraints:**

-   `m == matrix.length`
-   `n == matrix[i].length`
-   `1 <= m, n <= 200`
-   0 <= matrix[i][j] <= 2<sup>31</sup> - 1

```python
class Solution:
    def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
        ROWS, COLS = len(matrix), len(matrix[0])
        
        dp = {}
        
        def dfs(r, c, prev):
            if (r not in range(ROWS) or
                c not in range(COLS) or
                matrix[r][c] <= prev # here is the increasing check
            ):
                return 0
            
            if (r, c) in dp:
                return dp[(r, c)]
            
            newPrev = matrix[r][c]
            res = 1
            
            for dr, dc in [[1, 0], [-1, 0], [0, 1], [0, -1]]:
                res = max(res, 1 + dfs(r + dr, c + dc, newPrev))
                
                
            dp[(r, c)] = res
            
            return res
        
        
        for r in range(ROWS):
            for c in range(COLS):
                dfs(r, c, -1) # so that we can determine if the next cell we go is increasing or not
                
        
        return max(dp.values())
```