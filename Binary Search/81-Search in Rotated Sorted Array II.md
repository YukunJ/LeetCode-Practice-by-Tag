**81. Search in Rotated Sorted Array II**

```Tag: binary search```

**Description:**

There is an integer array ```nums``` sorted in non-decreasing order (not necessarily with **distinct** values).

Before being passed to your function, ```nums``` is **rotated** at an unknown pivot index ```k (0 <= k < nums.length)``` such that the resulting array is ```[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] ```(**0-indexed**). For example, ```[0,1,2,4,4,4,5,6,6,7]``` might be rotated at pivot index ```5``` and become ```[4,5,6,6,7,0,1,2,4,4]```.

Given the array ```nums``` **after** the rotation and an integer ```target```, return ```true``` if ```target``` is in ```nums```, or ```false``` if it is not in ```nums```.

**Follow up**: This problem is the same as Search in Rotated Sorted Array, where nums may contain duplicates. Would this affect the runtime complexity? How and why?

**Example1**:

        Input: nums = [2,5,6,0,0,1,2], target = 0
        Output: true

**Example2**:

        Input: nums = [2,5,6,0,0,1,2], target = 3
        Output: false

-----------

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        """
        The only difference between this question and question 33 
        is that the list may contain duplicates
        And when that happens, i.e. nums[left] = nums[mid],
        we are not sure if the left-hand side is well sorted or right-hand side
        consider for example [1,2,1,1,1](right sorted) and [1,1,1,2,1](left sorted)
        In this case, we have to make left boundary move forward one step
        
        Time Complexity: O(n) in worst case, we need to iterate through the whole list
                         for example [1,1,1,1,1,1,2] and target = 2
                         But on average, we expect to get O(logn) by binary search
                         if there are not that much of duplicates
        Space Complexity: O(1) 
        """
        if not nums: # empty lst
            return False
        l, r = 0, len(nums)-1
        while l <= r:
            mid = l + (r-l)//2
            if nums[mid] == target: # find
                return True
            elif nums[l] == nums[mid]: # not sure case, move the left boundary
                l += 1
            # the rest is the same as in question 33. 
            elif nums[l] < nums[mid]: # first phase : not under the cliff
                if nums[l] <= target < nums[mid]:
                    r = mid - 1
                else:
                    l = mid + 1
            else: # second phase : under the cliff
                if nums[mid] < target <= nums[r]:
                    l = mid + 1
                else:
                    r = mid - 1
        return False
```
