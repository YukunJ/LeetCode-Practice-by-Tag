**19. Remove Nth Node From End of List**

```Tag : linked list/two pointers```

**Description:**

Given the ```head``` of a linked list, remove the ```n-th``` node from the end of the list and return its head.


**Example1:**

![avatar](Fig/19-E1.jpg)

		Input: head = [1,2,3,4,5], n = 2
		Output: [1,2,3,5]

**Example2:**

		Input: head = [1], n = 1
		Output: []

**Example3:**

		Input: head = [1,2], n = 1
		Output: [1]

-----------

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        """
        The difficulty in this problem is that,
        as we traversal along the linked list, we "forget" the node in the past
        to be able to correctly "remove" a previous node,
        we can remember the 'current - n' node from the past and its prev and next
        
        denote n := length of linked list
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        header = ListNode(float('inf'), next=head) # fake header
        curr = head
        for i in range(n):
            # let the current node move n steps in advance
            curr = curr.next
        prev = header # the previous node of the node-to-remove
        while curr:
            prev = prev.next
            curr = curr.next
        prev.next = prev.next.next
        return header.next
```
