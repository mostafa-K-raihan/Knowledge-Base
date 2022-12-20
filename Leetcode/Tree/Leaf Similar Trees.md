Consider all the leaves of a binary tree, from left to right order, the values of those leaves form a **leaf value sequence**_._

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/16/tree.png)

For example, in the given tree above, the leaf value sequence is `(6, 7, 4, 9, 8)`.

Two binary trees are considered _leaf-similar_ if their leaf value sequence is the same.

Return `true` if and only if the two given trees with head nodes `root1` and `root2` are leaf-similar.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/03/leaf-similar-1.jpg)

**Input:** root1 = [3,5,1,6,2,9,8,null,null,7,4], root2 = [3,5,1,6,7,4,2,null,null,null,null,null,null,9,8]
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/03/leaf-similar-2.jpg)

**Input:** root1 = [1,2,3], root2 = [1,3,2]
**Output:** false

**Constraints:**

-   The number of nodes in each tree will be in the range `[1, 200]`.
-   Both of the given trees will have values in the range `[0, 200]`.

```python
# Definition for a binary tree node.

# class TreeNode:

#     def __init__(self, val=0, left=None, right=None):

#         self.val = val

#         self.left = left

#         self.right = right

class Solution:

    def leafSimilar(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> bool:

        def getLeaf(root, arr):

            if not root:

                return

            if not root.left and not root.right:

                arr.append(root.val)

            getLeaf(root.left, arr)

            getLeaf(root.right, arr)

  

        left, right = [], []

        getLeaf(root1, left)

        getLeaf(root2, right)

  

        return left == right
```