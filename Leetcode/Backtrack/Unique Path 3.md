You are given an `m x n` integer array `grid` where `grid[i][j]` could be:

-   `1` representing the starting square. There is exactly one starting square.
-   `2` representing the ending square. There is exactly one ending square.
-   `0` representing empty squares we can walk over.
-   `-1` representing obstacles that we cannot walk over.

Return _the number of 4-directional walks from the starting square to the ending square, that walk over every non-obstacle square exactly once_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/08/02/lc-unique1.jpg)

**Input:** grid = [[1,0,0,0],[0,0,0,0],[0,0,2,-1]]
**Output:** 2
**Explanation:** We have the following two paths: 
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2)
2. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2)

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/08/02/lc-unique2.jpg)

**Input:** grid = [[1,0,0,0],[0,0,0,0],[0,0,0,2]]
**Output:** 4
**Explanation:** We have the following four paths: 
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2),(2,3)
2. (0,0),(0,1),(1,1),(1,0),(2,0),(2,1),(2,2),(1,2),(0,2),(0,3),(1,3),(2,3)
3. (0,0),(1,0),(2,0),(2,1),(2,2),(1,2),(1,1),(0,1),(0,2),(0,3),(1,3),(2,3)
4. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2),(2,3)

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/08/02/lc-unique3-.jpg)

**Input:** grid = [[0,1],[2,0]]
**Output:** 0
**Explanation:** There is no path that walks over every empty square exactly once.
Note that the starting and ending square can be anywhere in the grid.

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `1 <= m, n <= 20`
-   `1 <= m * n <= 20`
-   `-1 <= grid[i][j] <= 2`
-   There is exactly one starting cell and one ending cell.

```python
class Solution:

    def uniquePathsIII(self, grid: List[List[int]]) -> int:
        ROWS, COLS = len(grid), len(grid[0])
        ans = 0

        def dfs(i, j, zeros):
            nonlocal ans
            if (i not in range(ROWS) or
                j not in range(COLS) or
                grid[i][j] == -1
            ):
                return
  

            if (grid[i][j] == 2):
                if (zeros == 0):
                    ans += 1
                return

            grid[i][j] = -1
            dfs(i+1, j, zeros-1)
            dfs(i-1, j, zeros-1)
            dfs(i, j+1, zeros-1)
            dfs(i, j-1, zeros-1)
            #revert back to zero

            grid[i][j] = 0

  
        zeros, x, y = 0, 0, 0
        for i in range(ROWS):
            for j in range(COLS):
                if grid[i][j] == 1:
                    x = i
                    y = j
                elif grid[i][j] == 0:
                    zeros += 1
        # since we will be setting this cell to zero, we start with + 1
        dfs(x, y, zeros+1)
        return ans
```


```python
class Solution:
    def uniquePathsIII(self, grid: List[List[int]]) -> int:
        R, C = len(grid), len(grid[0])
        self.ans = 0

        zeros = sum(row.count(0) for row in grid)
        start = [(x, y) for x in range(R) for y in range(C) if grid[x][y] == 1][0]


        def dfs(x, y, zeros):
            grid[x][y] = 3
            for dr, dc in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
                X, Y = x + dr, y + dc
                if X in range(R) and Y in range(C):

                    if grid[X][Y] == 2 and zeros == 0:
                        self.ans += 1
                    if grid[X][Y] == 0:
                        dfs(X, Y, zeros - 1)
                
            grid[x][y] = 0
            return

        dfs(*start, zeros)
        return self.ans
```