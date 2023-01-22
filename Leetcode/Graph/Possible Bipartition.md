We want to split a group of `n` people (labeled from `1` to `n`) into two groups of **any size**. Each person may dislike some other people, and they should not go into the same group.

Given the integer `n` and the array `dislikes` where `dislikes[i] = [ai, bi]` indicates that the person labeled `ai` does not like the person labeled `bi`, return `true` _if it is possible to split everyone into two groups in this way_.

**Example 1:**

**Input:** n = 4, dislikes = [[1,2],[1,3],[2,4]]
**Output:** true
**Explanation:** group1 [1,4] and group2 [2,3].

**Example 2:**

**Input:** n = 3, dislikes = [[1,2],[1,3],[2,3]]
**Output:** false

**Example 3:**

**Input:** n = 5, dislikes = [[1,2],[2,3],[3,4],[4,5],[1,5]]
**Output:** false

**Constraints:**

-   `1 <= n <= 2000`
-   0 <= dislikes.length <= 10<sup>4</sup>
-   `dislikes[i].length == 2`
-   `1 <= dislikes[i][j] <= n`
-   `ai < bi`
-   All the pairs of `dislikes` are **unique**.

```python
class Solution:

    def possibleBipartition(self, n: int, dislikes: List[List[int]]) -> bool:
        graph = defaultdict(list)
        # create adjacency list
        for x, y in dislikes:
            graph[x].append(y)
            graph[y].append(x)  

        q = deque([])
        # init color list for each node from 1 to n, started from 0, but we can discard 0
        # -1 for unvisited
        # 0, 1 for the two groups
        colors = [-1 for i in range(n+1)]

        # graph maybe disconnected
        # so we need to check all vertices from 1 to n
        for i in range(1, n+1):
            # if it is not visited, push it to q, and color it to 0
            if colors[i] == -1:
                q.append(i)
                colors[i] = 0

            while q:
                front = q.popleft()
                current_color = colors[front]
                
                # for all the neighbors
                for nei in graph[front]:
                    # if it is not visited yet, assign it a different color
                    # if it was a zero before 1-0 = 1, if it was 1 before 1-1 = 0
                    # and push it to q, to process further (its neighbors)
                    if colors[nei] == -1:
                        colors[nei] = 1 - current_color
                        q.append(nei)
                    # if it is visited, check if it is the same color as the source node
                    if colors[nei] == current_color:
                        return False
        # we process all the nodes, did not hit any bump
        return True
```

