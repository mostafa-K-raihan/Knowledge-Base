There is a tree (i.e. a connected, undirected graph with no cycles) consisting of `n` nodes numbered from `0` to `n - 1` and exactly `n - 1` edges.

You are given a **0-indexed** integer array `vals` of length `n` where `vals[i]` denotes the value of the `ith` node. You are also given a 2D integer array `edges` where `edges[i] = [ai, bi]` denotes that there exists an **undirected** edge connecting nodes `ai` and `bi`.

A **good path** is a simple path that satisfies the following conditions:

1.  The starting node and the ending node have the **same** value.
2.  All nodes between the starting node and the ending node have values **less than or equal to** the starting node (i.e. the starting node's value should be the maximum value along the path).

Return _the number of distinct good paths_.

Note that a path and its reverse are counted as the **same** path. For example, `0 -> 1` is considered to be the same as `1 -> 0`. A single node is also considered as a valid path.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/08/04/f9caaac15b383af9115c5586779dec5.png)

**Input:** vals = [1,3,2,1,3], edges = [[0,1],[0,2],[2,3],[2,4]]
**Output:** 6
**Explanation:** There are 5 good paths consisting of a single node.
There is 1 additional good path: 1 -> 0 -> 2 -> 4.
(The reverse path 4 -> 2 -> 0 -> 1 is treated as the same as 1 -> 0 -> 2 -> 4.)
Note that 0 -> 2 -> 3 is not a good path because vals[2] > vals[0].

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/08/04/149d3065ec165a71a1b9aec890776ff.png)

**Input:** vals = [1,1,2,2,3], edges = [[0,1],[1,2],[2,3],[2,4]]
**Output:** 7
**Explanation:** There are 5 good paths consisting of a single node.
There are 2 additional good paths: 0 -> 1 and 2 -> 3.

**Example 3:**

![](https://assets.leetcode.com/uploads/2022/08/04/31705e22af3d9c0a557459bc7d1b62d.png)

**Input:** vals = [1], edges = []
**Output:** 1
**Explanation:** The tree consists of only one node, so there is one good path.

**Constraints:**

-   `n == vals.length`
-   `1 <= n <= 3 * 104`
-   `0 <= vals[i] <= 105`
-   `edges.length == n - 1`
-   `edges[i].length == 2`
-   `0 <= ai, bi < n`
-   `ai != bi`
-   `edges` represents a valid tree.

```python
class Solution:
    def numberOfGoodPaths(self, vals: List[int], edges: List[List[int]]) -> int:
        '''
        This problem is based on Disjoint Set Union.
        We first consider all n nodes as n separate trees.
        Any edge joins exactly 2 of these subtrees.
        First of all, we need to sort edges vector based on the maximum value of vertices that it connects.
        Once sorted, we start looking at the edges one by one.
        At every iteration, we will connect 2 subtrees.
        Suppose the max elements in these 2 subtrees is a & b respectively and
        there frequency is a_freq & b_freq respectively.
  
        If a==b, we have found a_freq*b_freq new paths.
        These paths have starting nodes in first subtree and ending nodes in the other subtree.
        Now we merge these two subtrees.
        We set parent[b]=a,
        so a_freq=a_freq + b_freq.
  
        Suppose a!=b. In this case we have found no new paths.
        Assign parent of subtree with smaller max element to parent of subtree with larger max element.

        Continue this until we have covered all the edges.
        '''

  

        n = len(vals)
        parent, freq, max_val = [-1]*n, {}, {}

        for i, v in enumerate (vals):
            parent[i] = i
            freq[i] = 1
            max_val[i] = v

        def find(x):
            if parent[x] == x:
                return x
            parent[x] = find(parent[x])
            return parent[x]

        def union(x, y):
            nonlocal ans
            rx, ry = find(x), find(y)
            mx, my = max_val[rx], max_val[ry]

            if mx != my:
                if mx > my:
                    parent[ry] = rx
                else:
                    parent[rx] = ry
                return
            ans += (freq[rx] * freq[ry])
            freq[rx] += freq[ry]
            parent[ry] = rx
  

        ans = n
        for x, y in sorted(edges, key=lambda edge: max(vals[edge[0]], vals[edge[1]])):
            union(x, y)

        return ans
```