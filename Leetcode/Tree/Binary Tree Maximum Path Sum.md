A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.

The **path sum** of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return _the maximum **path sum** of any **non-empty** path_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)

**Input:** root = [1,2,3]
**Output:** 6
**Explanation:** The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg)

**Input:** root = [-10,9,20,null,null,15,7]
**Output:** 42
**Explanation:** The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.

**Constraints:**

-   The number of nodes in the tree is in the range [1, 3 * 10<sup>4</sup>].
-   `-1000 <= Node.val <= 1000`

```python
# Definition for a binary tree node.

# class TreeNode:

#     def __init__(self, val=0, left=None, right=None):

#         self.val = val

#         self.left = left

#         self.right = right

class Solution:

    def maxPathSum(self, root: Optional[TreeNode]) -> int:

        self.max_sum = float('-inf')

        def dfs(root):

            if not root:

                return 0

            # find left and right subtree sums

            # also limit the sum by going out towards negative

            l, r = max(0, dfs(root.left)), max(0, dfs(root.right))

  

            # update the max sum by taking left, right and root sum

            self.max_sum = max(self.max_sum, root.val + l + r)

  

            # take only larger sum, ie choose only one branch with root

            return max(l, r) + root.val

  

        dfs(root)

        return self.max_sum
```