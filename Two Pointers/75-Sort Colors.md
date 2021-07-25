**75. Sort Colorse**

```Tag : two pointers/sorting```

**Description:**

Given an array ```nums``` with ```n``` objects colored red, white, or blue, sort them **in-place** so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers ```0```, ```1```, and ```2``` to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

**Example1:**

		Input: nums = [2,0,2,1,1,0]
		Output: [0,0,1,1,2,2]

**Example2:**

		Input: nums = [2,0,1]
		Output: [0,1,2]

**Example3:**

		Input: nums = [0]
		Output: [0]

**Example4:**

		Input: nums = [1]
		Output: [1]

-----------

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        We want to come up with a one-pass O(n) solution,
        essentially we want to place '0's at beginning, '2's at end
        We maintain two pointers p0 and p2 to achieve this.
        Notice that if nums[i] == 2, when we swap nums[i] and nums[p2]
        but a problem could occur that if nums[p2] originally is '2', 
        after swap, nums[i] is still '2'
        then if we directly move forward, we miss one
        therefore we need to keep swapping until nums[i] is no longer '2'
		
	denote n := len(nums)
	Time Complexity : O(n)
	Space Complexity : O(1)
        """
        n = len(nums)
        i, p0, p2 = 0, 0, n-1
        while i <= p2:
            while i <= p2 and nums[i] == 2:
                # use while loop, make sure nums[i] is no longer '2'
                nums[i], nums[p2] = nums[p2], nums[i]
                p2 -= 1
            if nums[i] == 0:
                nums[i], nums[p0] = nums[p0], nums[i]
                p0 += 1
            i += 1 # increment the left traverl pointer
```
