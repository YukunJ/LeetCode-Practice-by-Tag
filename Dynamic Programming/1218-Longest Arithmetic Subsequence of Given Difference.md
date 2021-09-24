**1218. Longest Arithmetic Subsequence of Given Difference**

```Tag : dynamic programming/hashtable/array```

**Description:**

Given an integer array ```arr``` and an integer ```difference```, return the length of the longest subsequence in ```arr``` which is an arithmetic sequence such that the difference between adjacent elements in the subsequence equals ```difference```.

A **subsequence** is a sequence that can be derived from ```arr``` by deleting some or no elements without changing the order of the remaining elements.

**Example1:**

        Input: arr = [1,2,3,4], difference = 1
        Output: 4
        Explanation: The longest arithmetic subsequence is [1,2,3,4].

**Example2:**

        Input: arr = [1,3,5,7], difference = 1
        Output: 1
        Explanation: The longest arithmetic subsequence is any single element.

**Example3:**

        Input: arr = [1,5,7,8,5,3,4,2,1], difference = -2
        Output: 4
        Explanation: The longest arithmetic subsequence is [7,5,3,1].

-----------

**Solution1: O(n^2) (TLE)**
```python
class Solution:
    def longestSubsequence(self, arr: List[int], difference: int) -> int:
        """
        We adopt a dynamic programming approach
        denote dp[i] := the LAS sequence length ending with arry[i]
        Then, 
        for all j < i:
        if arr[j] + difference == arr[i]: dp[i] = max(dp[i], 1+dp[j])
        
        and for initialization, dp[i] = 1 for 1-element subarray
        
        denote n := len(arr)
        Time Complexity  : O(n^2)
        Space Complexity : O(n)
        """
        n = len(arr)
        dp = [1] * n
        for i in range(1, n):
            for j in range(i):
                if arr[j] + difference == arr[i]:
                    dp[i] = max(dp[i], 1+dp[j])
        return max(dp)
```

-----------

**Solution2: O(n)**
```python
class Solution:
    def longestSubsequence(self, arr: List[int], difference: int) -> int:
        """
        built upon the hints, denote dp[i] the LAS length ending with number i
        we could do one-pass through the array and
        dp[i] = 1 + dp[i-difference]
        
        denote n := len(arr)
        Time Complexity  : O(n)
        Space Complexity : O(n)
        """
        from collections import defaultdict
        dp = defaultdict(int)
        ans = 1
        for num in arr:
            dp[num] = dp[num-difference] + 1
            ans = max(ans, dp[num])
        return ans
```
