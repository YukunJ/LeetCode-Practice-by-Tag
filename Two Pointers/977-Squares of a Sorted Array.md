**977. Squares of a Sorted Array**

```Tag : two pointers/array/sorting```

**Description:**

Given an integer array ```nums``` sorted in **non-decreasing** order, return an array of **the squares of each number** sorted in non-decreasing order.

**Follow up**: Squaring each element and sorting the new array is very trivial, could you find an ```O(n)``` solution using a different approach?

**Example1:**

		Input: nums = [-4,-1,0,3,10]
		Output: [0,1,9,16,100]
		Explanation: After squaring, the array becomes [16,1,0,9,100].
		After sorting, it becomes [0,1,9,16,100].

**Example2:**

		Input: nums = [-7,-3,2,3,11]
		Output: [4,9,9,49,121]

-----------

```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        """
        This is a variant of two pointer question
        loosely speaking the already sorted array is
        [big_negative, ... small_negative, small_positive, ..., big_positive]
        we maintain two pointers from left and from right, to fill the answer vector backward, biggest first
        
        denote n := len(nums)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        if not nums: # boundary checking 
            return []
        
        n = len(nums)
        res = [0] * n
        position, left_ptr, right_ptr = n-1, 0, n-1
        while left_ptr <= right_ptr:
            if abs(nums[left_ptr]) < abs(nums[right_ptr]):
                pick = nums[right_ptr]
                right_ptr -= 1
            else:
                pick = nums[left_ptr]
                left_ptr += 1
            # fill the result vector starting from the biggest squared value
            res[position] = pick * pick
            position -= 1
        return res
```