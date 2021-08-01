**35. Search Insert Position**

```Tag : binary search```

**Description:**

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with ```O(log n)``` runtime complexity.

**Example1:**

		Input: nums = [1,3,5,6], target = 5
		Output: 2

**Example2:**

		Input: nums = [1,3,5,6], target = 2
		Output: 1

**Example3:**

		Input: nums = [1,3,5,6], target = 7
		Output: 4

**Example4:**

		Input: nums = [1,3,5,6], target = 0
		Output: 0

**Example5:**

		Input: nums = [1], target = 0
		Output: 0

-----------

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        """
        A very basic binary search question
        
        denote n := len(nums)
        Time Complexity : O(logn)
        Space Complexity : O(1)
        """
        ans = len(nums)
        left, right = 0, len(nums) - 1
        while left <= right:
            mid = left + (right - left) // 2
            if nums[mid] == target:
                # found the number
                return mid
            elif nums[mid] > target:
                ans = mid
                right = mid - 1
            else: # nums[mid] < target
                left = mid + 1
        return ans
```