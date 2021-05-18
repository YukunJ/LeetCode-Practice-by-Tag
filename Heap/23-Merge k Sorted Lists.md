**23. Merge k Sorted Lists**

```Tag : Heap```

**Description:**

You are given an array of ```k``` linked-lists ```lists```, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

**Example1**:

        Input: lists = [[1,4,5],[1,3,4],[2,6]]
        Output: [1,1,2,3,4,4,5,6]
        Explanation: The linked-lists are:
        [
          1->4->5,
          1->3->4,
          2->6
        ]
        merging them into one sorted list:
        1->1->2->3->4->4->5->6

**Example2**:

        Input: lists = []
        Output: []

**Example3**:

        Input: lists = [[]]
        Output: []
        
-----------

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        """
        We construct a min heap of size k
        Initially we put the first node of the k linked lists into the heap
        Then we also take the min from the k, and replace a new element into the heap
        
        denote n the sum of number of ListNode in these k linked lists
        Time Complexity : O(nlogk)
        Space Complexity : O(k) for min-heap
        """
        import heapq
        heap = list()
        head = curr = ListNode()
        for i, node in enumerate(lists):
            if node: # not initially empty
                # store in form of (value, index)
                heapq.heappush(heap, (node.val, i))
        
        while heap:
            value, index = heapq.heappop(heap)
            # create a new node for answer storage
            curr.next = ListNode(value)
            curr = curr.next
            # try to get a new node from the "index" linked list
            lists[index] = lists[index].next # point to next
            if lists[index]:
                heapq.heappush(heap, (lists[index].val, index))
                
        return head.next
                        
```
