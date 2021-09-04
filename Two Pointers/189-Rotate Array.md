**189. Rotate Array**

```Tag : two pointers/array/math```

**Description:**

Given an array, rotate the array to the right by ```k``` steps, where ```k``` is non-negative.

**Example1:**

		Input: nums = [1,2,3,4,5,6,7], k = 3
		Output: [5,6,7,1,2,3,4]
		Explanation:
		rotate 1 steps to the right: [7,1,2,3,4,5,6]
		rotate 2 steps to the right: [6,7,1,2,3,4,5]
		rotate 3 steps to the right: [5,6,7,1,2,3,4]

**Example2:**
		
		Input: nums = [-1,-100,3,99], k = 2
		Output: [3,99,-1,-100]
		Explanation: 
		rotate 1 steps to the right: [99,-1,-100,3]
		rotate 2 steps to the right: [3,99,-1,-100]

**Follow up**:

+ Try to come up with as many solutions as you can. There are at least three different ways to solve this problem.

+ Could you do it in-place with O(1) extra space?

**Hints**:

+ The easiest solution would use additional memory and that is perfectly fine.

+ The actual trick comes when trying to solve this problem without using any additional memory. This means you need to use the original array somehow to move the elements around. Now, we can place each element in its original location and shift all the elements around it to adjust as that would be too costly and most likely will time out on larger input arrays.

+ One line of thought is based on reversing the array (or parts of it) to obtain the desired result. Think about how reversal might potentially help us out by using an example.

+ The other line of thought is a tad bit complicated but essentially it builds on the idea of placing each element in its original position while keeping track of the element originally in that position. Basically, at every step, we place an element in its rightful position and keep track of the element already there or the one being overwritten in an additional variable. We can't do this in one linear pass and the idea here is based on **cyclic-dependencies** between elements.

-----------

```python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        """
        To solve this problem without using extra memory is a bit tricky
        Take an example for illustraion:
        [1, 2, 3, 4, 5, 6, 7] k=3
        after the rotation, it becomes [5, 6, 7, 1, 2, 3, 4]
        we found that the last k elements become the head of the new array, while the first n-k elements shift rightward
        and they are in the original order
        We observe that if we just completely reverse the array, it would be [7, 6, 5, 4, 3, 2, 1]
        and then we could further reverse the first k and latter n-k elements respectively.
        It will become [5, 6, 7, 1, 2, 3, 4]
        
        denote n := len(nums)
        Time Complexityy : O(n)
        Space Complexity : O(1)
        """
        def reverse(nums: List[int], begin: int, end: int):
            """Helper function: reverse a given array from begin index to end index inclusively"""
            ptr1, ptr2 = begin, end
            while ptr1 < ptr2:
                nums[ptr1], nums[ptr2] = nums[ptr2], nums[ptr1]
                ptr1 += 1
                ptr2 -= 1
        k %= len(nums) # wrap around extra multiple of len(nums)
        reverse(nums, 0, len(nums)-1) # completely array reversal
        reverse(nums, 0, k-1) # reverse the first k elements
        reverse(nums, k, len(nums)-1) # reverse the latter n-k elements
```

