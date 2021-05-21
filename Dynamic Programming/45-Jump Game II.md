**45. Jump Game II**

```Tag: dynamic programming, greedy```

**Description:**

Given an array of non-negative integers ```nums```, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

You can assume that you can always reach the last index.

**Example1**:

        Input: nums = [2,3,1,1,4]
        Output: 2
        Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
        
**Example2**:

        Input: nums = [2,3,0,1,4]
        Output: 2

-----------

```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        """
        use a dynamic (greedy) programming scheme, denote len(nums) = n
        After making a step, we maintain the farest distance we can jump to and try to update it as we go through the nums list
        if we have current_position == farest distance, we have no choice
        but to make one more step jump and continue
        This is a greedy approach.
        
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        n = len(nums)
        step, end, far = 0, 0, 0
        for curr in range(n-1):
            if end >= curr:
                # update the farest distance we may jump to
                far = max(far, curr + nums[curr])
                if curr == end: 
                    # reach the boundary of last step
                    # update the boudary, make one more step
                    end = far
                    step += 1
        return step
```


  
