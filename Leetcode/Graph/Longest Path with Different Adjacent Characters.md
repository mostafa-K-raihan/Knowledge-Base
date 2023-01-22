ou are given a **tree** (i.e. a connected, undirected graph that has no cycles) **rooted** at node `0` consisting of `n` nodes numbered from `0` to `n - 1`. The tree is represented by a **0-indexed** array `parent` of size `n`, where `parent[i]` is the parent of node `i`. Since node `0` is the root, `parent[0] == -1`.

You are also given a string `s` of length `n`, where `s[i]` is the character assigned to node `i`.

Return _the length of the **longest path** in the tree such that no pair of **adjacent** nodes on the path have the same character assigned to them._

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/03/25/testingdrawio.png)

**Input:** parent = [-1,0,0,1,1,2], s = "abacbe"
**Output:** 3
**Explanation:** The longest path where each two adjacent nodes have different characters in the tree is the path: 0 -> 1 -> 3. The length of this path is 3, so 3 is returned.
It can be proven that there is no longer path that satisfies the conditions. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/25/graph2drawio.png)

**Input:** parent = [-1,0,0,0], s = "aabc"
**Output:** 3
**Explanation:** The longest path where each two adjacent nodes have different characters is the path: 2 -> 0 -> 3. The length of this path is 3, so 3 is returned.

**Constraints:**

-   `n == parent.length == s.length`
-   `1 <= n <= 105`
-   `0 <= parent[i] <= n - 1` for all `i >= 1`
-   `parent[0] == -1`
-   `parent` represents a valid tree.
-   `s` consists of only lowercase English letters.

```python
class Solution:
    def longestPath(self, parent: List[int], s: str) -> int:
        g = defaultdict(list)
        for i in range(len(parent)):
            # since -1 is the parent of root
            if (parent[i] ^ -1):
                g[parent[i]].append(i)

        ans = 0
        # when we are considering a node, we need to find two largest lengths
        # from its children, then the maxLength for that node would be
        # largest length + 1 (for the node) + second largest node
        # imagine the node is in the middle of the chain
        # the left part is for the nodes that creates the largest chain (from the nodes children)
        # the right part is for the nodes that creates the second largest chain (from the nodes children)
        def dfs(node):
            nonlocal ans
            # keep track of largest two path lengths
            max1, max2 = 0, 0
            for i in g[node]:
                # for each child of a node, calc its largest path
                val = dfs(i)
                # if the char differs between the nodes
                if s[node] != s[i]:
                    # update the 2nd largest
                    if val > max2:
                        max2 = val
                    # if the 2nd largest becomes greater than the 1st one, swap those
                    if max2 > max1:
                        max1, max2 = max2, max1
            ans = max(ans, max1 + max2 + 1)
            # largest length + node itself
            return max1 + 1

        dfs(0)
        return ans
```