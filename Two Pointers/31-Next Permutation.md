**31. Next Permutation**

```Tag: two pointers/math```

**Description:**

Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such an arrangement is not possible, it must rearrange it as the lowest possible order (i.e., sorted in ascending order).

The replacement must be **in place** and use only constant extra memory.

**Example1**:

        Input: nums = [1,2,3]
        Output: [1,3,2]

**Example2**:

        Input: nums = [3,2,1]
        Output: [1,2,3]
  
**Example3**:

        Input: nums = [1,1,5]
        Output: [1,5,1]
        
**Example4**:

        Input: nums = [1]
        Output: [1]
          
-----------

```python
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        We want to find a pair (i,j) such that i < j yet nums[i] < nums[j]
        and we can swap them to make a bigger permutation
        here, to make it the "least bigger", we want the swap to happen as right position at possible
        we start search from the end of the list and search for first i, s.t. nums[i] < nums[i+1]
        and we find the right most j such that nums[i] < nums[j] and swap them
        then we make [i+1, n) to be increasing sequence (by our way of process, it's decreasing now)
        we use two pointers directly swap this slice [i+1, n) to make it increasing
        
        denote n := len(nums)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        left = len(nums) - 2
        while left >= 0 and nums[left] >= nums[left+1]:
            # try find i s.t. nums[i] < nums[i+1]
            left -= 1
            
        if left >= 0:
            # if enter here, means a reversion pair is found
            # otherwise directly swap the whole array nums from 0 to len(nums)
            right = len(nums) - 1
            while right >= 0 and nums[right] <= nums[left]:
                right -= 1
            # swap
            nums[left], nums[right] = nums[right], nums[left]
            
        i, j = left+1, len(nums)-1
        while i < j:
            nums[i], nums[j] = nums[j], nums[i]
            i += 1
            j -= 1
        
        return
```
