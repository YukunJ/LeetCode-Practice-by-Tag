**445. Add Two Numbers II**

```Tag : linked list/math```

**Description:**

You are given two **non-empty** linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example1:**

![avatar](Fig/445-E1.jpg)

		Input: l1 = [7,2,4,3], l2 = [5,6,4]
		Output: [7,8,0,7]

**Example2:**

		Input: l1 = [2,4,3], l2 = [5,6,4]
		Output: [8,0,7]

**Example3:**

		Input: l1 = [0], l2 = [0]
		Output: [0]

-----------

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        """
        To achieve the goal of "not reversing the list"
        we need to first deal with the "shared length" part of the two linked list
        and we finally deal with the accumulated carrier
        
        denote n := length of l1 list, m := length of l2 list
        Time Complexity : O(n+m)
        Space Complexity : O(max(n, m))
        """
        # find the length of both 
        len1, len2 = 0, 0
        curr1, curr2 = l1, l2
        while curr1:
            len1 += 1
            curr1 = curr1.next
        while curr2:
            len2 += 1
            curr2 = curr2.next
        
        # sum up the corresponding position without dealing with carrier
        curr1, curr2 = l1, l2
        head = None
        while len1 > 0 and len2 > 0:
            val = 0
            if len1 >= len2:
                val += curr1.val
                curr1 = curr1.next
                len1 -= 1
            if len1 < len2:
                val += curr2.val
                curr2 = curr2.next
                len2 -= 1
            
            # update: add to the front of linked list
            curr = ListNode(val)
            curr.next = head
            head = curr
        
        # deal with the carrier part
        tmp_curr, head = head, None
        carry = 0
        while tmp_curr:
            carry, val = divmod(tmp_curr.val+carry, 10)
            curr = ListNode(val)
            curr.next = head
            head = curr
            
            tmp_curr = tmp_curr.next
        
        if carry > 0:
            curr = ListNode(carry)
            curr.next = head
            head = curr
        
        return head
```
