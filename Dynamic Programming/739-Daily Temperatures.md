**739. Daily Temperatures**

```Tag: monotone stack```

**Description:**

Given an array of integers ```temperatures``` represents the daily temperatures, return an array ```answer``` such that ```answer[i]``` is the number of days you have to wait after the ```ith``` day to get *a warmer temperature*. If there is no future day for which this is possible, keep ```answer[i] == 0``` instead.

**Example1**:

		Input: temperatures = [73,74,75,71,69,72,76,73]
		Output: [1,1,4,2,1,1,0,0]
 
**Example2**:

		Input: temperatures = [30,40,50,60]
		Output: [1,1,1,0]

**Example3**:

		Input: temperatures = [30,60,90]
		Output: [1,1,0]

**Hints:**

+ If the temperature is say, 70 today, then in the future a warmer temperature must be either 71, 72, 73, ..., 99, or 100. We could remember when all of them occur next.
    
-----------

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        """
        We solve the problem in monotone stack way
        we maintain a stack of index with decreasing temperature associated from bottom to top
        when we see a new day's temperature, if it's higher than the stack top, then
        some previous day stored in the stack just find their "first warm day"

        denote n := len(temperatures)
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        n = len(temperatures)
        ans = [0] * n
        stack = []
        for idx, temperature in enumerate(temperatures):
            while stack and temperatures[stack[-1]] < temperature:
                prev_idx = stack.pop()
                ans[prev_idx] = idx-prev_idx
            stack.append(idx)
        return ans
```