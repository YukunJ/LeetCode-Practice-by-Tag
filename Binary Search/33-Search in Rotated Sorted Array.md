**33 . Search in Rotated Sorted Array**

There is an integer array ```nums``` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, ```nums``` is rotated at an unknown pivot index ```k (0 <= k < nums.length)``` such that the resulting array is ```[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]``` (**0-indexed**). For example, ```[0,1,2,4,5,6,7]``` might be rotated at pivot index ```3``` and become ```[4,5,6,7,0,1,2]```.

Given the array ```nums``` after the rotation and an integer ```target```, return the index of ```target``` if it is in nums, or ```-1``` if it is not in nums.

**Example1**:
        Input: nums = [4,5,6,7,0,1,2], target = 0
        Output: 4

**Example2**:
        Input: nums = [4,5,6,7,0,1,2], target = 3
        Output: -1

**Example3**:
        Input: nums = [1], target = 0
        Output: -1

------------

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        """
        The list looks like:
        [a0, +, + ,..., max, min, +, +, +, an] and a0 > an
        namely 2 phases of increasing, in between is a steep cliff
        We use binary search, and need to distinguish which phase we are on
        # first phase: the mid is not "under the cliff"
        # second phase: the mid is "under the cliff"
        
        Time Complexity: O(logn) by binary search
        Space Complexity: O(1) 
        """
        if not nums: # empty lst
            return -1
        l, r = 0, len(nums)-1
        while l <= r:
            mid = l + (r-l)//2
            if nums[mid] == target: # find
                return mid
            elif nums[l] <= nums[mid]: # first phase : not under the cliff
                if nums[l] <= target < nums[mid]:
                    r = mid - 1
                else:
                    # this 'else' has two cases
                    # either target > nums[mid]
                    # or target < nums[mid] & target < nums[l]
                    # need to search the "cliff" direction
                    l = mid + 1
            else: # second phase : under the cliff
                if nums[mid] < target <= nums[r]:
                    l = mid + 1
                else:
                    # this 'else' has two cases:
                    # either target < nums[mid] 
                    # or target > nums[mid] & target > nums[r]
                    # need to search the "non-cliff" direction
                    r = mid - 1
        return -1
```
