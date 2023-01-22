Given an undirected tree consisting of `n` vertices numbered from `0` to `n-1`, which has some apples in their vertices. You spend 1 second to walk over one edge of the tree. _Return the minimum time in seconds you have to spend to collect all apples in the tree, starting at **vertex 0** and coming back to this vertex._

The edges of the undirected tree are given in the array `edges`, where `edges[i] = [ai, bi]` means that exists an edge connecting the vertices `ai` and `bi`. Additionally, there is a boolean array `hasApple`, where `hasApple[i] = true` means that vertex `i` has an apple; otherwise, it does not have any apple.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/04/23/min_time_collect_apple_1.png)

**Input:** n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,true,false,true,true,false]
**Output:** 8 
**Explanation:** The figure above represents the given tree where red vertices have an apple. One optimal path to collect all apples is shown by the green arrows.  

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/04/23/min_time_collect_apple_2.png)

**Input:** n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,true,false,false,true,false]
**Output:** 6
**Explanation:** The figure above represents the given tree where red vertices have an apple. One optimal path to collect all apples is shown by the green arrows.  

**Example 3:**

**Input:** n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,false,false,false,false,false]
**Output:** 0

**Constraints:**

-   `1 <= n <= 105`
-   `edges.length == n - 1`
-   `edges[i].length == 2`
-   `0 <= ai < bi <= n - 1`
-   `fromi < toi`
-   `hasApple.length == n`

```python
class Solution:
    def minTime(self, n: int, edges: List[List[int]], hasApple: List[bool]) -> int:
        g = defaultdict(list)
        visited = [False for i in range(n)]
        for x, y in edges:
            g[x].append(y)
            g[y].append(x)

        count = 0
        # define a dfs function
        # which will return True if we find an apple in one node
        # otherwise false
        # but even if we don't find an apple or we do find apple
        # regardless, we need to check its children for possible apples there
        # if we find an apple, we need to add 2, for going and coming back via the edge
        def dfs(node):
            nonlocal count
            ret = False
            if hasApple[node]:
                ret = True

            visited[node] = True
            for nei in g[node]:
                if not visited[nei]:
                    if dfs(nei):
                        count += 2
                        ret = True
            return ret

        dfs(0)
        return count
```