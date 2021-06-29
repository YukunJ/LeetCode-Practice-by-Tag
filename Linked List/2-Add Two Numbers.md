**2. Add Two Numbers**

```Tag: linked list```

**Description:**

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.



**Example1**:

![avatar](Fig/2-E1.jpg)

		Input: l1 = [2,4,3], l2 = [5,6,4]
		Output: [7,0,8]
		Explanation: 342 + 465 = 807.

**Example2**:

		Input: l1 = [0], l2 = [0]
		Output: [0]

**Example3**:

		Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
		Output: [8,9,9,9,0,0,0,1]

-----------

**Solution1: Recursion**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        """
        This is recursion problem in the setting of linked list
        except from add two numbers into a new ListNode,
        we need to consider about the addition carrier and when to stop
        we stop recursion only when both l1 and l2 are None and carrier is 0
        
        denote n := len(l1), m := len(l2)
        Time Complexity : O(max(n,m))
        Space Complexity : O(max(n,m)) recursion stack height
        """
        def helper(l1: ListNode, l2: ListNode, carrier: int) -> ListNode:
            if (not l1) and (not l2) and (carrier == 0):
                # end of addition recursion, return
                return None
            else:
                val = carrier
                next_l1, next_l2, next_carrier = l1, l2, 0
                if l1:
                    val += l1.val
                    next_l1 = l1.next
                if l2:
                    val += l2.val
                    next_l2 = l2.next
                val, next_carrier = val % 10, val // 10
                curr = ListNode(val=val)
                curr.next = helper(next_l1, next_l2, next_carrier) # recursion
                return curr
        
        return helper(l1, l2, 0)
```

-----------

**Soluton2: Iterative**

```python
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        """
        we can also iteratively solve the problem without recursion,
        this will reduce our time complexity from O(max(n,m)) to O(1)
        
        denote n := len(l1), m := len(l2)
        Time Complexity : O(max(n,m))
        Space Complexity : O(1)
        """
        carrier = 0
        header = ListNode() # a fake node to store the beginning of our result
        curr = header # the stepper
        while l1 or l2:
            v1 = l1.val if l1 else 0
            v2 = l2.val if l2 else 0
            v = v1 + v2 + carrier
            v, carrier = v % 10, v // 10
            next_node = ListNode(val=v)
            curr.next = next_node
            curr = curr.next
            if l1:
                l1 = l1.next
            if l2:
                l2 = l2.next
        if carrier > 0:
            next_node = ListNode(val=carrier)
            curr.next = next_node
        
        return header.next
```
