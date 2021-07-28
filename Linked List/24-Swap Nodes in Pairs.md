**24. Swap Nodes in Pairs**

```Tag : linked list/recursion```

**Description:**

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)


**Example1**:

![avatar](Fig/24-E1.jpeg)

		Input: head = [1,2,3,4]
		Output: [2,1,4,3]

 
**Example2**:
 
		Input: head = []
		Output: []

**Example3**:
 
		Input: head = [1]
		Output: [1]
      
-----------


```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        """
        We want to find a in-place solution without using recursion stack height
	One interesting approach is :
        consider a linked list 1 -> 2 -> 3 -> 4 -> 5-> 6 -> ...
        the swapPairs answer is 2 -> 1 -> 4 -> 3 -> 6 -> 5
        we want to construct 2 jump linked list:
        1) 1 -> 3 -> 5 -> ... odd list
        2) 2 -> 4 -> 6 -> ... even list
        and join (1) and (2) one node by another, (2) moves first
	But this approach is a bit "think too much, not necessary". We follow an easier approach as follows.

        denote n := length of linked list
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        if not head or not head.next: # boundary case, empty or single node
            return head

        header = ListNode(float('inf'), next=head)
        prev = header
        while prev.next and prev.next.next:
            # when there is a remaining pair to be swaped
            # change prev -> node1 -> node2 -> node2.next
            # to prev -> node2 -> node1 -> node2.next
            node1 = prev.next
            node2 = prev.next.next
            node1.next = node2.next
            node2.next = node1
            prev.next = node2
            prev = node1
        return header.next  
```
