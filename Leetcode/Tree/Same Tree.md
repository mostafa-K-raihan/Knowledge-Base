Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/20/ex1.jpg)

**Input:** p = [1,2,3], q = [1,2,3]
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/20/ex2.jpg)

**Input:** p = [1,2], q = [1,null,2]
**Output:** false

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/12/20/ex3.jpg)

**Input:** p = [1,2,1], q = [1,1,2]
**Output:** false

**Constraints:**

-   The number of nodes in both trees is in the range `[0, 100]`.
-   `-104 <= Node.val <= 104`

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
		# if both are null, they are same
        if not p and not q:
            return True

		# if one is null, and the other is not, they are not same
        if (p and not q) or (not p and q):
            return False

		# if the val does not match in the same node they are not same
        if p.val != q.val:
            return False

		# check recursively on left subtree and right
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```