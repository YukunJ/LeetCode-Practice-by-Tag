**416. Partition Equal Subset Sum**

```Tag : dynamic programming/array```

**Description:**

Given a **non-empty** array ```nums``` containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

**Example1:**

		input: nums = [1,5,11,5]
		Output: true
		Explanation: The array can be partitioned as [1, 5, 5] and [11].

**Example2:**
		
		Input: nums = [1,2,3,5]
		Output: false
		Explanation: The array cannot be partitioned into equal sum subsets.

-----------

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        """
        First observation is that, if we are to split the array in two equal-sum subset
        the subset_sum = sum(nums) // 2 and must be integer divisible
        And we adopt a dynamic programming scheme:
        denote dp[i][j]: the subset_sum = j could be achieve using elements in nums[:i]
        Then, the state transition goes like:
        1) if dp[i-1][j] is True, no need to include nums[i-1], dp[i][j] = dp[i-1][j] = True
        2) else, another alternative is if dp[i-1][j-nums[i-1]] is True, the dp[i][j] = True as well
        In the end, we check dp[len(nums)][subset_sum]
        and we realize that the dp state transition only depends on last row
        we could use compressed rolling row to save space complexity
        
        denote n := len(nums), m := sum(nums)
        Time Complexity : O(n*m)
        Space Complexity : O(m) by rolling vector
        """
        n = len(nums)
        total_sum = sum(nums)
        if total_sum % 2 != 0:
            # must be integer-2-divisible
            return False
        subset_sum = total_sum // 2
        rolling_row = [True] + [False] * subset_sum
        for i in range(1, n+1):
            new_row = [False] * (subset_sum+1)
            for j in range(subset_sum+1):
                condition1 = rolling_row[j] # not adding current num
                condition2 = rolling_row[j-nums[i-1]] if j-nums[i-1]>=0 else False # if curr num could fit in
                new_row[j] |= (condition1 | condition2)
            rolling_row = new_row # rolling vector to save space
        return rolling_row[subset_sum]
```

