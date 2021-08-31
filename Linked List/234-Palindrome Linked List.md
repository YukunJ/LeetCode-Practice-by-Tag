**234. Palindrome Linked List**

```Tag : linked list/recursion/two pointers```

**Description:**

Given the ```head``` of a singly linked list, return true if it is a palindrome.

**Example1:**

![avatar](Fig/234-E1.jpg)
	
		Input: head = [1,2,2,1]
		Output: true

**Example2:**

![avatar](Fig/234-E2.jpg)

		Input: head = [1,2]
		Output: false

**Follow up**: Could you do it in ```O(n)``` time and ```O(1)``` space?

-----------

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        """
        To do this problem in O(n) time complexity and O(1) space
        we first need to know the length of the linked list
        then we try to reverse the second half of the linked list
        and then walk step by step to compare the first half and second half
        
        denote n := length of this linked list
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        def reverse(head: ListNode) -> ListNode:
            """Helper function: reverse a linked list iteratively"""
            if not head or not head.next:
                return head
            prev, curr = None, head
            while curr:
                next_node = curr.next
                curr.next = prev
                prev, curr = curr, next_node
            return prev
        
        if not head or not head.next: 
            # the boundary case: empty or single
            return True
        
        # get the length of linked list
        length = 0
        curr = head
        while curr:
            length += 1
            curr = curr.next
            
        # if the length is even, we want to skip exactly half
        # if the length is odd, we want to skip integer division by 2 plus 1
        to_skip = (length +1 ) // 2 
        prev, curr_second = None, head
        for i in range(to_skip):
            prev = curr_second
            curr_second = curr_second.next
        
        # we will record the end of first half, because we want to restore the linked list intact later
        first_half_end = prev
        first_half_end.next = None # break the linking temporarily
        reversed_head = reverse(curr_second)
        reversed_curr = reversed_head
        answer = True
        while head and reversed_curr:
            if head.val != reversed_curr.val:
                answer = False
                break
            head, reversed_curr = head.next, reversed_curr.next
        
        # restore the linked list, re-reverse the second half and join with the first half
        original_second_head = reverse(reversed_head)
        first_half_end.next = original_second_head
        
        return answer
```
