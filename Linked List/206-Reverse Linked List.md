**206. Reverse Linked List**

```Tag : linked list/recursion```

**Description:**

Given the ```head``` of a singly linked list, reverse the list, and return the *reversed list*.

**Example1:**

![avatar](Fig/206-E1.jpg)
	
		Input: head = [1,2,3,4,5]
		Output: [5,4,3,2,1]

**Example2:**

![avatar](Fig/206-E2.jpg)

		Input: head = [1,2]
		Output: [2,1]

**Example3:**

		Input: head = []
		Output: []

**Follow up**: A linked list can be reversed either iteratively or recursively. Could you implement both?

-----------

**Solution1: Recursion**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        """
        Recursion version
        denote n := length of this linked list
        Time Complexity : O(n)
        Space Complexity : O(n) for stack height
        """
        if not head or not head.next: 
            # boundary case empty or single
            return head
        # the current node is the last node after reversion
        # the next node is the tail of reversed sub linked list
        reversehead = self.reverseList(head.next)
        head.next.next = head
        head.next = None # break the loop
        return reversehead
```

-----------

**Solution2: Iterative**

```python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        """
        Iterative version
        denote n := length of this linked list
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        if not head or not head.next: 
            # boundary case empty or single
            return head
        prev, curr = None, head
        # 1 -> 2 -> 3 -> 4 -> None
        # None <- 1 <- 2 <- 3 <- 4
        while curr:
            next_node = curr.next
            curr.next = prev
            prev, curr = curr, next_node
        return prev
```