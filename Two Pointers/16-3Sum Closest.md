**16. 3Sum Closest**

```Tag: Sorting/Two pointers```

**Description:**

Given an array ```nums``` of *n* integers and an integer ```target```, find three integers in ```nums``` such that the sum is closest to ```target```. Return the sum of the three integers. You may assume that each input would have exactly one solution.

**Example1:**

        Input: nums = [-1,2,1,-4], target = 1
        Output: 2
        Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

-----------

```python
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        """
        A Brute force solution would take n^3 to enumerate all possible combination
        a clever approach is to first do a sorting on the array, and
        on condition that fix the first element, do a sliding window two pointers approach
        to solve the rest two positions
        
        denote n := len(nums)
        Time Complexity : O(n^2) sorting takes O(n*logn), there are n iteration of each sliding window takes O(n), yields O(n^2)
        Space Complexity : O(logn) we may use O(logn) stack height in sorting, 
                            or if we are not allowed to sort in-place, we might need O(n) for extra storage
        """
        nums.sort()
        n = len(nums)
        close_sum = None
        for first in range(n):
            first_sum = nums[first]
            # init two sliding pointers
            second, third = first+1, n-1
            while second < third:
                temp_sum = first_sum + nums[second] + nums[third]
                if close_sum is None or abs(temp_sum-target) < abs(close_sum-target):
                    # update a 'closer' result
                    close_sum = temp_sum
                # sliding window two pointers approach
                if temp_sum == target: # perfectly solve the problem
                    return temp_sum
                elif temp_sum > target:
                    third -= 1
                else:
                    second += 1
        return close_sum    
```
