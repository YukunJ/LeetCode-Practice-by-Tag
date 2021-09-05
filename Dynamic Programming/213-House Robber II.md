**213. House Robber II**

```Tag : dynamic programming/array```

**Description:**

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle**. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array ```nums``` representing the amount of money of each house, return the maximum amount of money you can rob tonight **without alerting the police**.

**Example1:**

		Input: nums = [2,3,2]
		Output: 3
		Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.

**Example2:**
		
		Input: nums = [1,2,3,1]
		Output: 4
		Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
		Total amount you can rob = 1 + 3 = 4.

**Example3:**

		Input: nums = [1,2,3]
		Output: 3

-----------

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        """
        The only difference between this question and Q198 <House Robber>
        is that now the houses are arranged in a circle not an array
        we could not rob the first and last house all together
        However, for the rest houses the operation and dynamic programming scheme should still work
        We just need to consider 2 cases separately:
        1) rob first house, not the last one
        2) rob the last house, not the first one
        
        denote n := len(nums)
        Time Complexity : O(n) solve the problem Q198 twice
        Space Complexity : O(1) by using compressed rolling variable
        """
        def sub_rob(nums: List[int], start: int, end: int) -> int:
            if end-start <= 1:
                # boundary case : either single house or double houses
                return max(nums[start], nums[start+1])
            prev_prev, prev = nums[start],  max(nums[start], nums[start+1])
            for i in range(start+2, end+1):
                prev_prev, prev = prev, max(prev, nums[i]+prev_prev)
            return prev
        
        n = len(nums)
        if n <= 2:
            # boundary case : either single house or double houses
            return max(nums)
        
        return max(sub_rob(nums, 0, n-2), sub_rob(nums, 1, n-1))
```

