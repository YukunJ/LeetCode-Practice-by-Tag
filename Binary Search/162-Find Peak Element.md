**162. Find Peak Element**

```Tag: binary search```

**Description:**

A peak element is an element that is strictly greater than its neighbors.

Given an integer array ```nums```, find a peak element, and return its index. If the array contains multiple peaks, return the index to **any of the peaks**.

You may imagine that ```nums[-1] = nums[n] = -∞```.

**Example1**:

        Input: nums = [1,2,3,1]
        Output: 2
        Explanation: 3 is a peak element and your function should return the index number 2.

**Example2**:

        Input: nums = [1,2,1,3,5,6,4]
        Output: 5
        Explanation: Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.

-----------

```python
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        """
        A simple linear scan could give a solution of O(n) time complexity
        But if we wanna to go O(logn), we need binary search.
        But How? The List is unordered. 
        One key observation is that since we assume a[-1] = a[m] = -infty
        since we need only 1 peak to be returned,
            if on a decreasing slide, there will be a peak on the left
            if on an increasing slide, there will be a peak on the right
        
        Time Complexity : O(logn) by binary search
        Space Complexity: O(1)
        """

        l, r = 0, len(nums)-1
        while l < r:
            mid = l + (r-l)//2
            if nums[mid] < nums[mid+1]:
                l = mid + 1
            else: # nums[mid] >= nums[mid+1]
                r = mid
        return l
```
