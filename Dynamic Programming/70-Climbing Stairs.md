**70. Climbing Stairs**

```Tag: dynamic programming```

**Description:**

You are climbing a staircase. It takes ```n``` steps to reach the top.

Each time you can either climb ```1``` or ```2``` steps. In how many distinct ways can you climb to the top?

**Example1**:

        Input: n = 2
        Output: 2
        Explanation: There are two ways to climb to the top.
        1. 1 step + 1 step
        2. 2 steps
        
**Example2**:

        Input: n = 3
        Output: 3
        Explanation: There are three ways to climb to the top.
        1. 1 step + 1 step + 1 step
        2. 1 step + 2 steps
        3. 2 steps + 1 step
        
-----------

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        """
        This is a typical dynamic programming problem
        denote P(n) := the number of ways to climb n steps
        Then the state transition equation is : P(n) = P(n-1) + P(n-2)
        in interpretation, the last step you take is either a 1-step or 2-step
        we use a bottom-up approach
        
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        
        prev, curr = 1, 2 # init the P(1) and P(2)
        if n <= 2:
            return {1 : prev, 2 : curr}[n]
        
        for i in range(n - 2):
            prev, curr = curr, curr + prev
            
        return curr
```


  
