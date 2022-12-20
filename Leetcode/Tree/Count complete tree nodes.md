## Problem Statement
Given the `root` of a **complete** binary tree, return the number of the nodes in the tree.

According to **[Wikipedia](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between `1` and `2h` nodes inclusive at the last level `h`.

Design an algorithm that runs in less than `O(n)` time complexity.

![[Pasted image 20221115112628.png]]

**Example 1**
**Input:** root = [1,2,3,4,5,6]
**Output:** 6

**Example 2**
**Input:** root = []
**Output:** 0

**Example 3**
**Input:** root = [1]
**Output:** 1

**Constraints**
-   The number of nodes in the tree is in the range [0, 5 * 10<sup>4</sup>].
-   0 <= Node.val <= 5 * 10<sup>4</sup>
-   The tree is guaranteed to be **complete**.

### Tricks:
If the leftmost and rightmost nodes are valid, the tree is full binary tree, and the total node of that tree is 2 ^ level - 1.

### Code
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        
        if root:
            return 1 + self.countNodes(root.left) + self.countNodes(root.right)
        return 0
        # BFS iterative
#         if not root:
#             return 0
        
#         q, m = deque([root]), 0
        
#         while q:
#             node = q.popleft()
#             if node:
#                 m += 1
#                 q.append(node.left)
#                 q.append(node.right)

#         return m

        # Using the fact that both the children of complete bin tree is a complete bin tree
        
        
        if not root:
            return 0
        
        left, right = root.left, root.right
        lcount, rcount = 1, 1
        
        while left:
            # go to the left most node
            # and compute level
            left = left.left
            lcount += 1
        while right:
            # go to the right most node
            # and compute level
            right = right.right
            rcount += 1
            
        # if both levels are same
        # which means the tree is completely full
        # so we can return early using a formula
        if lcount == rcount:
            return 2**lcount - 1
        
        else:
            # the tree is not completely full from the top
            # maybe it is full in one level deep?
            # so we go deeper one level
            return 1 + self.countNodes(root.left) + self.countNodes(root.right)
```