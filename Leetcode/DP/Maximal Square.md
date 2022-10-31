### Problem Statement
Given an `m x n` binary `matrix` filled with `0`'s and `1`'s, _find the largest square containing only_ `1`'s _and return its area_.

![[Pasted image 20221025143736.png]]

```
Input: matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
Output: 4
```
### Code
```python
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        
        # the idea is to store largest squares' len starting on each cell
        # like what is the larget square can be constructed from a cell, given that
        # the cell is the left top corner?
        
        length, rows, cols = 0, len(matrix), len(matrix[0])
        
        dp = [[0 for j in range(cols)] for i in range(rows)]
        
        # base cases
        # for the last col, we can't form any square, cuz it will go out
        # we can only construct a square of len 1 if the matrix has a 1 in
        # that cell
        for i in range(rows):
            dp[i][cols-1] = 1 if matrix[i][cols-1] == "1" else 0
            length = max(length, dp[i][cols-1])
        
        # base cases
        # for the last row, we can't form any square, cuz it will go out
        # we can only construct a square of len 1 if the matrix has a 1 in
        # that cell
        
        for i in range(cols):
            dp[rows-1][i] = 1 if matrix[rows-1][i] == "1" else 0
            length = max(length, dp[rows-1][i])
        # recurrence relation
        # for each other cell, we can construct a square of len > 1
        # if its left, down, and diagonally right cells have 1
        # i.e (i, j+1), (i+1, j), (i+1, j+1)
        # if all of them are 1, and the current cell is 1, then we can 
        # create a square of 2
        # but if any one of them is not, then we can not construct a square
        # of 2. Idea behind taking a min, is, at least this many square can be
        # constructed.
        
        for i in range(rows-2, -1, -1):
            for j in range(cols-2, -1, -1):
                if (matrix[i][j] == "1"):
                    dp[i][j] = 1 + min (dp[i][j+1], dp[i+1][j], dp[i+1][j+1])
                    length = max(dp[i][j], length)
        
        return length ** 2
```