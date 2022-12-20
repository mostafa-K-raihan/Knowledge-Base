There is a **bi-directional** graph with `n` vertices, where each vertex is labeled from `0` to `n - 1` (**inclusive**). The edges in the graph are represented as a 2D integer array `edges`, where each `edges[i] = [ui, vi]` denotes a bi-directional edge between vertex `ui` and vertex `vi`. Every vertex pair is connected by **at most one** edge, and no vertex has an edge to itself.

You want to determine if there is a **valid path** that exists from vertex `source` to vertex `destination`.

Given `edges` and the integers `n`, `source`, and `destination`, return `true` _if there is a **valid path** from_ `source` _to_ `destination`_, or_ `false` _otherwise__._

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex1.png)

**Input:** n = 3, edges = [[0,1],[1,2],[2,0]], source = 0, destination = 2
**Output:** true
**Explanation:** There are two paths from vertex 0 to vertex 2:
- 0 → 1 → 2
- 0 → 2

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex2.png)

**Input:** n = 6, edges = [[0,1],[0,2],[3,5],[5,4],[4,3]], source = 0, destination = 5
**Output:** false
**Explanation:** There is no path from vertex 0 to vertex 5.

**Constraints:**

-   1 <= n <= 2 * 10<sup>5</sup>
-   0 <= edges.length <= 2 * 10<sup>5</sup>
-   `edges[i].length == 2`
-   `0 <= ui, vi <= n - 1`
-   `ui != vi`
-   `0 <= source, destination <= n - 1`
-   There are no duplicate edges.
-   There are no self edges.


```python
class Solution:

    def validPath(self, n: int, edges: List[List[int]], source: int, destination: int) -> bool:
	    # standard bfs
        q = deque([source])
        adj = [[] for x in range(n)]

        for edge in edges:
            s, d = edge[0], edge[1]
            adj[s].append(d)
            adj[d].append(s)

        visited = set()

        while q:
            front = q.popleft()
	        # edge case if source is the destination to begin with
            if front == destination:
                return True

            for nei in adj[front]:
                if nei not in visited:
                    visited.add(nei)
                    if nei == destination:
                        return True

                    q.append(nei)

        return False
```
