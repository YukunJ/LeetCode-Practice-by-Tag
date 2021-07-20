**148. Sort List**

```Tag : linked list/two pointers/sorting```

**Description:**

Given the ```head``` of a linked list, return the list after sorting it in **ascending order**.

Follow up: Can you sort the linked list in ```O(n*logn)``` time and ```O(1)``` memory (i.e. constant space)?


**Example1**:

![avatar](Fig/148-E1.jpeg)

		Input: head = [4,2,1,3]
		Output: [1,2,3,4]

**Example2**:

![avatar](Fig/148-E2.jpeg)

		Input: head = [-1,5,3,4,0]
		Output: [-1,0,3,4,5]

**Example3**:

		Input: head = []
		Output: []

        
-----------

**Solution1: Recursion Merge Sort (Space O(logn))**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        """
        we could use recursion to do a merge sort on the linked list
        we need to use fast/slow pointers to find the mid point and cut there
        because linked list could just re-link the "next" field without re-allocating space
        we could save the space complexity from standard O(n) re-allocation of array to O(logn) of recursion stack height
        
        denote n := len(linked list)
        Time Complexity : O(n*logn) standard merge sort
        Space Complexity : O(logn) recursion stack height
        """
        def merge(l1: ListNode, l2: ListNode):
            """Helper Function to merge two sorted linked list in-place"""
            if not l1: # boundary
                return l2
            elif not l2: # boundary
                return l1
            else:
                header = ListNode(float('inf'))
                curr = header
                while l1 and l2:
                    if l1.val <= l2.val:
                        curr.next = l1
                        l1 = l1.next
                    else:
                        curr.next = l2
                        l2 = l2.next
                    curr = curr.next 
                
                # deal with remaining part
                if l1:
                    curr.next = l1
                elif l2:
                    curr.next = l2
                # return first node after sort
                return header.next 
        
        # boundary case: empty or single node
        if not head or not head.next:
            return head
        # use fast and slow pointers to find mid point
        header = ListNode(float('inf'))
        header.next = head
        fast, slow = header, header
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        nexthead = slow.next # the split head
        slow.next = None # cut the linked list in half
        # recursion sort the left split and right split parts
        left_sort_head, right_sort_head = self.sortList(head), self.sortList(nexthead)
        
        return merge(left_sort_head, right_sort_head)
```

-----------

**Solution2: Inplace Merge Sort (Space O(1))**

```python
class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        """
        to achieve the requiredment O(1) space complexity, 
        we have to sort the linked list in place without recursion stack
        recall in merge sort, we first break the array into length=1 elements
        and merge 1 by 1 to a many sorted arrays of length=2, 
        and then merge pair by pair to get length=4, etc.
        we could simulate this process
        
        denote n := len(linked list)
        Time Complexity : O(n*logn) merge sort
        Space Complexity : O(1) 
        """
        # firstly, compute the length of this linked list
        length = 0
        walker = head
        while walker:
            walker = walker.next
            length += 1
        
        # init a fake header for recording keeping
        header = ListNode(val=float('inf'), next=head)
        interval = 1 # the starting pair array length
        while interval < length:
            prev, curr = header, header.next
            while curr:
                # try to get the header of a pair h1 and h2 linked list of length=interval
                h1, count1 = curr, 0
                while curr and count1 < interval:
                    curr = curr.next
                    count1 += 1
                if count1 < interval: # no need further search, h2 is None
                    break
                
                h2, count2 = curr, 0
                while curr and count2 < interval:
                    curr = curr.next
                    count2 += 1
                    
                # h2 might not be a full interval-length array, might be less
                length1, length2 = interval, count2
                
                # merge two sorted array into one
                while length1 > 0 and length2 > 0:
                    if h1.val <= h2.val:
                        prev.next = h1
                        h1 = h1.next
                        length1 -= 1
                    else:
                        prev.next = h2
                        h2 = h2.next
                        length2 -= 1
                    prev = prev.next
                        
                # get to the end of remaining
                prev.next = h1 if length1 > 0 else h2
                while length1 > 0 or length2 > 0:
                    prev = prev.next
                    length1 -= 1
                    length2 -= 1
                prev.next = curr
                
            interval *= 2 # double the pair array size
            
        # return the sorted list's head
        return header.next
```
