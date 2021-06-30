**410. Split Array Largest Sum**

```Tag: greedy, binary search```

**Description:**

Given an array ```nums``` which consists of non-negative integers and an integer ```m```, you can split the array into ```m``` non-empty continuous subarrays.

Write an algorithm to minimize the largest sum among these ```m``` subarrays.



**Example1**:

        Input: nums = [7,2,5,10,8], m = 2
        Output: 18
        Explanation:
        There are four ways to split nums into two subarrays.
        The best way is to split it into [7,2,5] and [10,8],
        where the largest sum among the two subarrays is only 18.
        
**Example2**:

        Input: nums = [1,2,3,4,5], m = 2
        Output: 9
        
**Example3**:

        Input: nums = [1,4,4], m = 3
        Output: 4

-----------

**Solution1: Dynamic Programming(Time Limit Exceeds)**

```python
class Solution:
    def splitArray(self, nums: List[int], m: int) -> int:
        """
        We could solve the problem in dynamic programming scheme
        we don't care how to split it into m subarrays,
        in dynamic programing, we only care how to do the first cut,
        and leave the rest m-1 cuts to recursion
        denote n := len(nums)
        we will store a memory dp of size n * m
        where dp[i][j] := the minimize largest subarray sum of spliting nums[i:] into j pieces
        and we will utilize a pre-sum array to avoid repetitve computation

        Time Complexity : O(n^2*m) there are O(n*m) state, each state takes O(n)
        Space Complexity: ; O(n*m)
        """
        # boundary case checking
        if not nums: 
            return 0
        if len(nums) == m:
            return max(nums)
        
        # construct pre-sum array and memory array
        n = len(nums)
        # dp[i][j] means the result for spliting nums[i:] into j pieces
        dp = [[float('-inf')] * (m+1) for _ in range(n)]
        presum = [0]
        for i in range(n):
            presum.append(presum[-1]+nums[i])
        
        # build helper function to recursively solve the problem
        def helper(start: int, piece: int, n: int, dp: List[List[int]], presum: List[int]) -> int:
            """Helper function:
                Solve the subproblem nums[start:] into "piece" pieces and return answer
            """
            # already calculated
            if dp[start][piece] != float('-inf'):
                return dp[start][piece]
            # only 1 piece remain
            if piece == 1:
                return presum[n] - presum[start]
            # try determine the first split and recursively deal with the rest piece-1 splits
            res = float('inf')
            for first_split in range(start, n-piece+1):
                # after this split, still need piece-1 split, need at least piece-1 elements to be splited upon
                # the last first_split index is on n-piece, so that there are piece-1 elements remaining for rest split
                # presum[first_split+1] is nums from 0 to first_split (including)
                # presume[start] is nums from 0 to start-1 (including)
                # diff is  nums from start to first_spit (including)
                left = presum[first_split+1] - presum[start]
                # right hand recursively
                right = helper(first_split+1, piece-1, n, dp, presum)
                res = min(res, max(left, right))
            dp[start][piece] = res
            return res
        
        return helper(0, m, n, dp, presum)    
```
-----------

**Solution2: Greedy+Binary Search**

```python
class Solution:
    def splitArray(self, nums: List[int], m: int) -> int:
        """
        A more efficient way is to use greedy and binary search
        the greediness lies in we try to get an answer "x", so 
        we traversal the list nums, whenever the current sum exceeds x,
        we make another group and increase the group count by 1
        when we finish, if the group count is smaller than m, we could aim for some better "x", i.e. smaller
        else the "x" we set is too small, we should increase it.

        The max value of "x" is sum(nums), min value of "x" is max(nums),
        we could do a binary search within this range
        Time Complexity : O(n*log(sum-max))
        Space Complexity: ; O(1)
        """
        def check(x: int) -> bool:
            """Helper function: check if a pre-fix "x" result is reachable or not"""
            group_count, curr = 1, 0
            for num in nums:
                if curr + num > x:
                    group_count += 1
                    curr = 0
                curr += num
            return group_count <= m
        # binary search for the answer
        left, right = max(nums), sum(nums)
        while left < right:
            mid = left + (right-left) // 2
            if check(mid):
                # can aim for smaller(or equal) answer
                right = mid
            else:
                # we are essentailly search for the "right-most" failure and plus 1 to it
                left = mid + 1

        return left 
```