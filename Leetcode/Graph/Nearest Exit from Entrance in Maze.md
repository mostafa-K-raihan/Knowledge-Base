You are given an `m x n` matrix `maze` (**0-indexed**) with empty cells (represented as `'.'`) and walls (represented as `'+'`). You are also given the `entrance` of the maze, where `entrance = [entrancerow, entrancecol]` denotes the row and column of the cell you are initially standing at.

In one step, you can move one cell **up**, **down**, **left**, or **right**. You cannot step into a cell with a wall, and you cannot step outside the maze. Your goal is to find the **nearest exit** from the `entrance`. An **exit** is defined as an **empty cell** that is at the **border** of the `maze`. The `entrance` **does not count** as an exit.

Return _the **number of steps** in the shortest path from the_ `entrance` _to the nearest exit, or_ `-1` _if no such path exists_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/04/nearest1-grid.jpg)

**Input:** maze = [["+","+",".","+"],[".",".",".","+"],["+","+","+","."]], entrance = [1,2]
**Output:** 1
**Explanation:** There are 3 exits in this maze at [1,0], [0,2], and [2,3].
Initially, you are at the entrance cell [1,2].
- You can reach [1,0] by moving 2 steps left.
- You can reach [0,2] by moving 1 step up.
It is impossible to reach [2,3] from the entrance.
Thus, the nearest exit is [0,2], which is 1 step away.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/06/04/nearesr2-grid.jpg)

**Input:** maze = [["+","+","+"],[".",".","."],["+","+","+"]], entrance = [1,0]
**Output:** 2
**Explanation:** There is 1 exit in this maze at [1,2].
[1,0] does not count as an exit since it is the entrance cell.
Initially, you are at the entrance cell [1,0].
- You can reach [1,2] by moving 2 steps right.
Thus, the nearest exit is [1,2], which is 2 steps away.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/06/04/nearest3-grid.jpg)

**Input:** maze = [[".","+"]], entrance = [0,0]
**Output:** -1
**Explanation:** There are no exits in this maze.

**Constraints:**

-   `maze.length == m`
-   `maze[i].length == n`
-   `1 <= m, n <= 100`
-   `maze[i][j]` is either `'.'` or `'+'`.
-   `entrance.length == 2`
-   0 <= entrance<sub>row</sub> < m
-   0 <= entrance<sub>col</sub> < n
-   `entrance` will always be an empty cell.

```python
class Solution:
    def nearestExit(self, maze: List[List[str]], entrance: List[int]) -> int:
        rows, cols = len(maze), len(maze[0])
        
        q = deque([[entrance[0], entrance[1], 0]])
        
        visited = set()
        visited.add((entrance[0], entrance[1]))
        
        directions = [[1, 0], [-1, 0], [0, 1], [0, -1]]
        
        while q:
            x, y, d = q.popleft()
            
            for (dr, dc) in directions:
                new_x, new_y = dr + x, dc + y
                
                if (new_x in range(rows) and
                    new_y in range(cols) and
                    maze[new_x][new_y] == '.' and
                   (new_x, new_y) not in visited
                ):
                    # possibility of early exit,
                    # check for boundary condition
                    if new_x == 0 or new_y == 0 or new_x == rows -1 or new_y == cols-1:
                        return d+1
                
                    q.append([new_x, new_y, d+1])
                    visited.add((new_x, new_y))
        return -1
        
                    
        
```