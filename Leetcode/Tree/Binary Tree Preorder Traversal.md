Given the `root` of a binary tree, return _the preorder traversal of its nodes' values_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg)

**Input:** root = [1,null,2,3]
**Output:** [1,2,3]

**Example 2:**

**Input:** root = []
**Output:** []

**Example 3:**

**Input:** root = [1]
**Output:** [1]

**Constraints:**

-   The number of nodes in the tree is in the range `[0, 100]`.
-   `-100 <= Node.val <= 100`

**Follow up:** Recursive solution is trivial, could you do it iteratively?

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res, s = [], []
        s.append(root)

        while s:
            curr = s.pop()
            if not curr:
                break
            res.append(curr.val)
            if curr.right:
                s.append(curr.right)
            if curr.left:
                s.append(curr.left)
        return res
```