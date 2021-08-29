**53. Maximum Subarray**

```Tag : dynamic programming```

**Description:**

Given an integer array ```nums```, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

A **subarray** is a **contiguous** part of an array.

**Example1:**
	
		Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
		Output: 6
		Explanation: [4,-1,2,1] has the largest sum = 6.

**Example2:**

		Input: nums = [1]
		Output: 1

**Example3:**

		Input: nums = [5,4,-1,7,8]
		Output: 23

**Follow up**: If you have figured out the ```O(n)``` solution, try coding another solution using the **divide and conquer** approach, which is more subtle.

-----------

**Solution1: Dynamic Programming**

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        """
        We can use a dynamic programming skill to solve this problem
        we can denote dp[i] := the maxSubarray ending with nums[i]
        then dp[i] has two choices:
        it either stand alone dp[i] = nums[i]
        or if the previous sum is greater than zero, it could do dp[i] = dp[i-1] + nums[i]
        And we realize that the state transition only depends on last single state
        
        denote n := len(nums)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        n = len(nums)
        dp = [0] * n
        dp[0] = nums[0]
        maxSum = dp[0]
        for i in range(1, n):
            dp[i] = max(nums[i], dp[i-1] + nums[i])
            maxSum = max(maxSum, dp[i])
        return maxSum
```

-----------

**Solution2: Divide and Conquer**
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        """
        we could use an alternative and beautiful Divide and Conquer approach
        given the array nums, the maxSubarray falls into 3 cases:
        1) only use elements in the left half array
        2) only use elements in the right half array
        3) use both elements in the left and right half array (must be connected array)
        This essentially mimic a merge sort procedure
        
        denote n := len(nums)
        Time Complexity : O(nlogn)
        Space Complexity : O(logn)
        """
        def recur(nums: List[int], left: int, right: int) -> int:
            """Helper function: recursively divide and conquer"""
            if left > right: # boundary case checking
                return float('-inf')
            
            curr, best_left_sum, best_right_sum = 0, 0, 0
            mid = left + (right - left) // 2
            for i in range(mid-1, left-1, -1): # consecutively going left
                curr += nums[i]
                best_left_sum = max(best_left_sum, curr)
            curr = 0
            for i in range(mid+1, right+1): # consecutively going right
                curr += nums[i]
                best_right_sum = max(best_right_sum, curr)
            best_combined_sum = nums[mid] + best_left_sum + best_right_sum
            only_left_sum = recur(nums, left, mid-1)
            only_right_sum = recur(nums, mid+1, right)
            return max([best_combined_sum, only_left_sum, only_right_sum])
            
        return recur(nums, 0, len(nums)-1)
```
