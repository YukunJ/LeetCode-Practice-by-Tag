**283. Move Zeroes**

```Tag : two pointers/swap```

**Description:**

Given an integer array ```nums```, move all ```0```'s to the end of it while maintaining the relative order of the non-zero elements.

**Note** that you must do this in-place without making a copy of the array.

**Example1**:

        Input: nums = [0,1,0,3,12]
        Output: [1,3,12,0,0]
        
**Example2**:      

        Input: nums = [0]
        Output: [0]
 
 **Hints:**
 
 + ```In-place``` means we should not be allocating any space for extra array. But we are allowed to modify the existing array. However, as a first step, try coming up with a solution that makes use of additional space. For this problem as well, first apply the idea discussed using an additional array and the in-place solution will pop up eventually.
 
 + A ```two-pointer``` approach could be helpful here. The idea would be to have one pointer for iterating the array and another pointer that just works on the non-zero elements of the array.
 
-----------

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        we maintain a pointer pt to store the position for next non-zero element
        and we traverl the list from left to do the swapping when necessary
        
        denote n := len(nums)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        n = len(nums)
        pt = 0
        i = 0
        while i < n:
            if nums[i] != 0:
                nums[i], nums[pt] = nums[pt], nums[i]
                pt += 1
            i += 1
```
