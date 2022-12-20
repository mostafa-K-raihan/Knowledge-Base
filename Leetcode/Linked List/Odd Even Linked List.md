Given the `head` of a singly linked list, group all the nodes with odd indices together followed by the nodes with even indices, and return _the reordered list_.

The **first** node is considered **odd**, and the **second** node is **even**, and so on.

Note that the relative order inside both the even and odd groups should remain as it was in the input.

You must solve the problem in `O(1)` extra space complexity and `O(n)` time complexity.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/10/oddeven-linked-list.jpg)

**Input:** head = [1,2,3,4,5]
**Output:** [1,3,5,2,4]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/10/oddeven2-linked-list.jpg)

**Input:** head = [2,1,3,5,6,4,7]
**Output:** [2,3,6,7,1,5,4]

**Constraints:**

-   The number of nodes in the linked list is in the range [0, 10<sup>4</sup>].
-   -10<sup>6</sup> <= Node.val <= 10<sup>6</sup>

```python
# Definition for singly-linked list.

# class ListNode:

#     def __init__(self, val=0, next=None):

#         self.val = val

#         self.next = next

class Solution:

    def oddEvenList(self, head: Optional[ListNode]) -> Optional[ListNode]:

        # init two head, one points to odd position, the other points to even position

        odd, even = head, head.next if head else None

        # keep track of the even head

        evenHead = even

  

        # keep assigning even next to odd next

        # move the odd pointer along with it

  

        # then assign odd next to even next

        # and move the pointer along

  

        while even and even.next:

            odd.next = even.next

            odd = odd.next

            even.next = odd.next if odd else None

            even = even.next

  

        # for empty head []

        # no join necessary

        if odd:

            odd.next = evenHead

        return head
```