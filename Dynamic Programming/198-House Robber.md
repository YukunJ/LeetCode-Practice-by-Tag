**198. House Robber**

```Tag: dynamic programming```

**Description:**

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight **without alerting the police**.

**Example1**:

        Input: nums = [1,2,3,1]
        Output: 4
        Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
        Total amount you can rob = 1 + 3 = 4.

**Example2**:

        Input: nums = [2,7,9,3,1]
        Output: 12
        Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
        Total amount you can rob = 2 + 9 + 1 = 12.

-----------

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        """
        A dynamic programming problem
        At each position, only two choices avilable : 1) rob 2) skip
        And we cannot rob two consecutive houses
        denote dp[i] the max profit you can rob ending up with robbing house i
        then
        dp[i+1] := max(dp[i-1] + nums[i+1] , dp[i])
        we may use a rolling variable to replace the dp vector to save space
        
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        n = len(nums)
        if n == 1:
            return nums[0]
        # prev_prev := dp[i-1], prev := dp[i]
        # we compute dp[i+1] based on i-1 and i information
        prev_prev, prev = nums[0], max(nums[0], nums[1])
        for i in range(2, n):
            prev_prev, prev = prev, max(nums[i]+prev_prev, prev)
        return prev
```
