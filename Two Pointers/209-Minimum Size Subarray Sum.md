**209. Minimum Size Subarray Sum**

```Tag: two pointers/sliding window```

**Description:**

Given an array of positive integers ```nums``` and a positive integer ```target```, return the minimal length of a **contiguous subarray** ```[numsl, numsl+1, ..., numsr-1, numsr]``` of which the sum is greater than or equal to ```target```. If there is no such subarray, return ```0``` instead.

**Follow up**: If you have figured out the ```O(n)``` solution, try coding another solution of which the time complexity is ```O(nlog(n))```.

**Example1**:

        Input: target = 7, nums = [2,3,1,2,4,3]
        Output: 2
        Explanation: The subarray [4,3] has the minimal length under the problem constraint.

**Example2**:

        Input: target = 4, nums = [1,4,4]
        Output: 1
        
**Example3**:

        Input: target = 11, nums = [1,1,1,1,1,1,1,1]
        Output: 0

-----------

**Solution1: Sliding window**

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        """
        This is a sliding window problem, we maintain left & right pointers
        we first fix the left pointer, to expand the right pointer trying to get the sum
        After reaching the sum, we try to contract the window by moving the left pointer to right
        
        denote n := len(nums)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        length = float('inf')
        curr_sum = 0
        left, right = 0, -1
        n = len(nums)
        while right < n-1:
            # expand the window
            curr_sum += nums[right+1]
            right += 1
            # try contract the window
            while curr_sum >= target and left <= right:
                length = min(length, right-left+1)
                # move left pointer to right by 1 position
                curr_sum -= nums[left]
                left += 1
                
        return length if length != float('inf') else 0

```

-----------

**Solution2: Presum + Binary Search**

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        """
        Can also use pre-sum and binary search to solve this problem
        
        denote n := len(nums)
        Time Complexity : O(nlogn) for at most n times binary search
        Space Complexity : O(n) for pre-sum storage
        """
        def find(lst: List[int], target: int):
            """Helper Function: return the left-most position that's suitable for inserting target, keep the lst ascending"""
            left, right, ans = 0, len(lst)-1, -1
            while left <= right:
                mid = left + (right-left) // 2
                if lst[mid] >= target:
                    ans = mid
                    right = mid - 1
                else:
                    left = mid + 1
            return ans if ans != -1 else len(lst)
            
        if not nums:
            return 0
        n = len(nums)
        ans = n + 1
        presum = [0]
        for i in range(n):
            # define presum[i] := sum(nums[0:i])
            presum.append(presum[-1]+nums[i])
        for i in range(1, n+1):
            new_target = target + presum[i-1]
            idx = find(presum, new_target)
            if idx != len(presum):
                ans = min(ans, idx - (i-1))
        return ans if ans != n+1 else 0
```
