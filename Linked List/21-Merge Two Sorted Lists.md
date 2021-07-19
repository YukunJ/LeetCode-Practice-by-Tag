**21. Merge Two Sorted Lists**

```Tag: Linked List/Recursion```

**Description:**

Merge two sorted linked lists and return it as a sorted list. The list should be made by splicing together the nodes of the first two lists.

**Example1:**

![avatar](Fig/21-E1.jpeg)

		Input: l1 = [1,2,4], l2 = [1,3,4]
		Output: [1,1,2,3,4,4]

**Example2:**

		Input: l1 = [], l2 = []
		Output: []

**Example3:**

		Input: l1 = [], l2 = [0]
		Output: [0]

-----------

**Solution1: Recursion**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        """
        We could use Recursion to solve this problem
        given two nonempty linked list l1 and l2
        if l1[0].val <= l2[0].val, then the head of merged list is l1[0], we continue to merge l1[1:] and l2
        if l1[0].val < l2[0].val, then the head of merged list is l2[0], we continue to merge l1 and l2[1:]
        boundary case is one of them becomes empty and we just return the other one

        denote n, m := len(l1 list), len(l2 list)
        Time Complexity : O(n+m)
        Space Complexity : O(n+m)
        """
        if not l1: # boundary case
            return l2
        if not l2: # boundary case
            return l1
        if l1.val <= l2.val:
            # l1[0] + merge(l1[1:], l2)
            l1.next = self.mergeTwoLists(l1.next, l2)
            return l1
        else:
            # l2[0] + merge(l1, l2[1:])
            l2.next = self.mergeTwoLists(l1, l2.next)
            return l2    
```

-----------

**Solution2: Iterative**

```python
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        """
        We could iteratively solve this problem without using recursion thus stack space 
        we make a fake header and everytime make it point to the smaller one of two linked list's head
        then move the corresponding list head to next one

        denote n, m := len(l1 list), len(l2 list)
        Time Complexity : O(n+m)
        Space Complexity : O(1)
        """
        header = ListNode(float('inf')) # the fake header
        curr = header
        while l1 and l2:
            if l1.val <= l2.val:
                curr.next = l1
                curr = curr.next
                l1 = l1.next
            else:
                curr.next = l2
                curr = curr.next
                l2 = l2.next 

        # end of while loop, one of the linked list is empty already
        curr.next = l1 if l2 is None else l2  

        return header.next 
```