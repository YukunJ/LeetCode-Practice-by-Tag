**300. Longest Increasing Subsequence**

```Tag: greedy, binary search```

**Description:**

Given an integer array ```nums```, return the length of the longest strictly increasing subsequence.

A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, ```[3,6,2,7]``` is a subsequence of the array ```[0,3,1,6,2,2,7]```.

**Example1**:

        Input: nums = [10,9,2,5,3,7,101,18]
        Output: 4
        Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
        
**Example2**:

        Input: nums = [0,1,0,3,2,3]
        Output: 4
        
**Example3**:

        Input: nums = [7,7,7,7,7,7,7]
        Output: 1

-----------

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        """
        We construct a O(nlogn) greedy algorithm with binary search
        define d[i] to represent the last element in LIS of length i
        notice the greedyness lies in the fact that,
        in order to find the longest increasing subsequence, we want
        the sequence increase as slow as possible.
        The d[i] sequence is an increasing sequence, because
        if i < j yet d[i] > d[j], by cutting the last (j-i) elements from d[j],
        we achieve an increasing sequence of lengthh i yet < d[i],
        a contradiction to the defintion of d[i]
        
        We iterate through the sequence nums, and main current max len.
        if nums[i] > d[len], directly add it 
        else, fit the nums[i] into d to achieve the "slowest increase"
        
        Time Complexity : O(nlogn) 1-time iteration with binary search
        Space Complexity : O(n) for storage of the array d
        """
        if not nums: # boundary case
            return 0
        d = [] # d[i] := the ending element of LIS of length i
        len = 0 # init the max LIS length
        for i, num in enumerate(nums):
            if not d or num > d[-1]:
                d.append(num)
                len += 1
            else:
                ans = -1
                l, r = 0, len-1
                while l <= r:
                    mid = l + (r-l)//2
                    if d[mid] >= num: # go left
                        r = mid - 1
                    else: # d[mid] < num
                        ans = mid
                        l = mid + 1
                d[ans+1] = num
                
        return len
```
