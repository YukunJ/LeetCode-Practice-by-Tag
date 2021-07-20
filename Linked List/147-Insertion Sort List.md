**147. Insertion Sort List**

```Tag: linked list/sorting```

**Description:**

Given the ```head``` of a singly linked list, sort the list using **insertion sort**, and return the sorted list's head.

The steps of the **insertion sort** algorithm:

1. Insertion sort iterates, consuming one input element each repetition and growing a sorted output list.

2. At each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list and inserts it there.

3. It repeats until no input elements remain.


**Example1:**

![avatar](Fig/147-E1.jpg)

        Input: head = [4,2,1,3]
        Output: [1,2,3,4]
        
**Example2:**

![avatar](Fig/147-E2.jpg)

        Input: head = [-1,5,3,4,0]
        Output: [-1,0,3,4,5]
                
-----------

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def insertionSortList(self, head: ListNode) -> ListNode:
        """
        To mimic the process of insertion sort, we maintain left part and right part
        left part is already sorted, and we are to place the first element in right part to its proper position in left part
        since we are using linkedin list, a single insert doesn't need to move all the folloing elements one position right
        but to find such a insertion place we need to travel from the head of linked list

        denote n := len(linked list)
        Time Complexity : O(n^2)
        Space Complexity : O(1)
        """
        # boundary case: empty or single node
        if (not head) or (not head.next):
            return head
        header = ListNode(float('inf')) # a fake header
        header.next = head
        curr = head.next
        lastSorted = head # first node is already sorted by default
        while curr:
            # find a position for curr
            if curr.val >= lastSorted.val: # move on
                lastSorted = lastSorted.next
            else:
                prev = header
                while prev.next.val <= curr.val:
                    # traversal to find insertion place
                    prev = prev.next 
                # break linking and re-link
                lastSorted.next = curr.next # jump
                curr.next = prev.next 
                prev.next = curr
            # move to next one
            # it's always the structure : "... -> lastSorted -> curr -> ...""
            curr = lastSorted.next

        return header.next
```
