Given an `n x n` array of integers `matrix`, return _the **minimum sum** of any **falling path** through_ `matrix`.

A **falling path** starts at any element in the first row and chooses the element in the next row that is either directly below or diagonally left/right. Specifically, the next element from position `(row, col)` will be `(row + 1, col - 1)`, `(row + 1, col)`, or `(row + 1, col + 1)`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/11/03/failing1-grid.jpg)

**Input:** matrix = [[2,1,3],[6,5,4],[7,8,9]]
**Output:** 13
**Explanation:** There are two falling paths with a minimum sum as shown.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/11/03/failing2-grid.jpg)

**Input:** matrix = [[-19,57],[-40,-5]]
**Output:** -59
**Explanation:** The falling path with a minimum sum is shown.

**Constraints:**

-   `n == matrix.length == matrix[i].length`
-   `1 <= n <= 100`
-   `-100 <= matrix[i][j] <= 100`

```python
class Solution:

    def minFallingPathSum(self, matrix: List[List[int]]) -> int:

        ROWS, COLS = len(matrix), len(matrix[0])

  

        for i in range(ROWS-2, -1, -1):

            for j in range(COLS-1, -1, -1):

                a = matrix[i+1][j+1] if j+1 < COLS else float('inf')

                b = matrix[i+1][j]

                c = matrix[i+1][j-1] if j-1 >= 0 else float('inf')

  

                matrix[i][j] += min(a, b, c)

        return min(matrix[0])
```