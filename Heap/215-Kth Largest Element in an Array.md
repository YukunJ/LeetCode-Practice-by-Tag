**215. Kth Largest Element in an Array**

```Tag : Heap```

**Description:**

Given an integer array ```nums``` and an integer ```k```, return the ```k-th``` largest element in the array.

Note that it is the ```k-th``` largest element in the sorted order, not the ```k-th``` distinct element.

**Example1**:

        Input: nums = [3,2,1,5,6,4], k = 2
        Output: 5

**Example2**:

        Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
        Output: 4

-----------

```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        """
        To find the k-th largest element, 
        we maintain a min-heap of size k, 
        so that each operation could be done in O(logk) time
        The criteria for an element to join the heap is 
            to exceed the top(k-th largest) element, and replace it
        
        Time Complexity : O(nlogk)
        Space Complexity : O(k)
        """
        import heapq
        myheap = []
        for num in nums:
            if len(myheap) < k:
                # not full size k yet, directly add
                heapq.heappush(myheap, num)
            else:
                # already full
                if num > myheap[0]: # exceed current k-th largest
                    heapq.heapreplace(myheap, num)
                #else: pass to next
        return myheap[0]
```
