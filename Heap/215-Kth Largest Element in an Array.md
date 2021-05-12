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

**Solution1(Min Heap)**

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

**Solution2(Qucik Sort)**

```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        """
        To find the k-th largest element, 
        we apply a similar idea as in quicksort
        we randomly pick an element and partition the array accordingly
        If the index of this element < k, we go right
        If index of this element > k, we go left
        otherwise, we find the k-th largest and we are done.
        
        Time Complexity : O(n) *(in expectation)*, 
                        worst case (n^2), if each time reduce by 1
        Space Complexity : O(logn) *(in expectation)*, 
                        worst case O(n) recursion stack
        """
        
        def partition(nums: List[int], l: int, r: int, k: int) -> int:
            """
            @param l: the left range(included)
            @param r: the right range(included)
            @param k: the k-th element in nums[l:r] we look for
            """
            # assume the pivot chosen is nums[r]
            pt = l - 1
            for i in range(l, r):
                if nums[i] > nums[r]:
                    pt += 1
                    nums[pt], nums[i] = nums[i], nums[pt]
            pt += 1
            nums[pt], nums[r] = nums[r], nums[pt]
            return pt # return the pivot's index
        
        def random_q(nums: List[int], l: int, r: int, k: int) -> int:
            # randomly pick a pivot in [l,r]
            pivot = random.randint(l, r)
            nums[pivot], nums[r] = nums[r], nums[pivot] # swap
            index = partition(nums, l, r, k) # partition, get index
            if index == k-1: # the k-th largest
                return nums[index]
            elif index < k-1:
                return random_q(nums, index+1, r, k)
            else: # index > k-1:
                return random_q(nums, l, index-1, k)
        
        return random_q(nums, 0, len(nums)-1, k)
```
