**55. Jump Game**

```Tag : dynamic programming/greedy/array```

**Description:**

You are given an integer array ```nums```. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return ```true``` if you can reach the last index, or ```false``` otherwise.

**Example1:**

		Input: nums = [2,3,1,1,4]
		Output: true
		Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.

**Example2:**

		Input: nums = [3,2,1,0,4]
		Output: false
		Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.

-----------

**Solution1: Dynamic Programming**

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        """
        We could adopt a dynamic programming solution in this question
        since we jump from left to right,
        when we are at index i and we can jump d distance,
        when the success of index i depends on index (i+d)
        therefore we can construct the dp matrix from right to left as follows:
        dp[i] = any(dp[i+d]) for 1 <= d <= nums[i]
        
        denote n := len(nums)
        Time Complexity : O(n^2)
        Space Complexity : O(n)
        """
        n = len(nums)
        dp = [False] * n
        dp[-1] = True
        for i in range(n-2, -1, -1):
            # construct dp from right to left
            farthest = min(n-1-i, nums[i])
            for d in range(1, farthest+1):
                if dp[i+d]:
                    dp[i] = True
                    break # just need to activate dp[i] once, done with this index
        return dp[0]
```

-----------

**Solution2: Greedy**

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        """
        We use a greedy approach: try jump as far as possible
        we will keep track of the farthest distance we can reach in one jump
        when necessary, we update the distance and make a new jump
        if we can no longer update the farthest distance, i.e. the current traversal index surpass it 
        this means we cannot jumpy to the end
        to conveniently achieve the mentioned algorithm, we will start from right to left
        we denote "left_boundary" as the left most point that can be reach
        then for an index i, if "i + num[i] >= left_boundary", we can update "left_boundary = i"
        
        denote n := len(nums)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        n = len(nums)
        left_boundary = n - 1 # standing on the last index
        for i in range(n-2, -1, -1):
            # traversal right to left
            if i + nums[i] >= left_boundary:
                left_boundary = i
        return left_boundary == 0 
```
