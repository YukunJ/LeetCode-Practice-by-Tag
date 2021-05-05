** 34. Find First and Last Position of Element in Sorted Array**

    Tag: binary search

Given an array of integers ```nums``` sorted in ascending order, find the starting and ending position of a given ```target``` value.

If ```target``` is not found in the array, return ```[-1, -1]```.

**Follow up**: Could you write an algorithm with ```O(log n)``` runtime complexity?

**Example1**:

        Input: nums = [5,7,7,8,8,10], target = 8
        Output: [3,4]

**Example2**:

        Input: nums = [5,7,7,8,8,10], target = 6
        Output: [-1,-1]

**Example3**:

        Input: nums = [], target = 0
        Output: [-1,-1]

------------

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        """
        We use 2 versions of binary search 
        to find the first and last position
        if not exists, naturally we get [-1, -1]
        
        Time Complexity : O(logn) by binary search twice
        Space Complexity : O(1)
        """
        def binary_search_first_pos(nums: List[int], target: int) -> int:
            ans, l, r = -1, 0, len(nums)-1
            while l <= r:
                mid = l + (r-l)//2
                if target == nums[mid]:
                # keep go left to find the "first" appearance
                # even if already find one
                    ans = mid
                    r = mid - 1
                elif target < nums[mid]: 
                    r = mid - 1
                else:
                    l = mid + 1
            return ans
        
        def binary_search_last_pos(nums: List[int], target: int) -> int:
            ans, l, r = -1, 0, len(nums)-1
            while l <= r:
                mid = l + (r-l)//2
                if target == nums[mid]:
                # keep go right to find the "last" appearance
                # even if already find one
                    ans = mid
                    l = mid + 1
                elif target < nums[mid]: 
                    r = mid - 1
                else:
                    l = mid + 1
            return ans
        
        if not nums:
            return [-1, -1]
        
        first = binary_search_first_pos(nums, target)
        last = binary_search_last_pos(nums, target)
        return [first, last]
```
