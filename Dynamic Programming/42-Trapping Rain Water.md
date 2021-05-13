**42. Trapping Rain Water**

```Tag: dynamic programming```

**Description:**

Given ```n``` non-negative integers representing an elevation map where the width of each bar is ```1```, compute how much water it can trap after raining.

**Example1**:

![avatar](Fig/42-E1.png)

        Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
        Output: 6
        Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.

**Example2**:

        Input: height = [4,2,0,3,2,5]
        Output: 9

-----------

**Solution1: dynamic programming**

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        """
        We use will dynamic programming to solve this question
        A first observation is that, for an index i
        it can hold min(max(height[:i]), max(height[i+1:])) - height[i] amount of rain
        i.e. check the left highest leftMax_i, right highest rightMax_i, 
        things in between should contain a rain surface level of min(leftMax_i, rightMax_i)
        then subtract the height[i] of itself.

        Time Complexity : O(n) by dynamic programming, linear traversal
        Space Complexity: O(n) by 2 array of length O(n)
        """
        n = len(height)
        if n <= 2: # boundary case
            return 0
        
        leftMax, rightMax, ans = [0] * n, [0] * n, 0 # init
        leftMax[0], rightMax[n-1] = height[0], height[n-1]
        for i in range(1, n):
            # left highest, max(height[:i])
            leftMax[i] = max(leftMax[i-1], height[i])
        for i in range(n-2, -1, -1):
            # right highest, max(height[i:])
            rightMax[i] = max(rightMax[i+1], height[i])

        # calculate answer
        for i in range(n):
            ans += min(leftMax[i], rightMax[i])-height[i]
        return ans
```
