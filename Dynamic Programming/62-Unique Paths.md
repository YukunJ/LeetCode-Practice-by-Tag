**62. Unique Paths**

```Tag: dynamic programming```

**Description:**

A robot is located at the top-left corner of a ```m x n``` grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

**Example1**:

![avatar](Fig/62-E1.png)

        nput: m = 3, n = 7
        Output: 28
        
**Example2**:

        Input: m = 3, n = 2
        Output: 3
        Explanation:
        From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
        +1. Right -> Down -> Down
        +2. Down -> Down -> Right
        +3. Down -> Right -> Down

**Example3**:

        Input: m = 7, n = 3
        Output: 28
        
**Example4**:

        Input: m = 3, n = 3
        Output: 6
        
-----------

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        """
        Although can be solved mathematically, we adopt dynamic programming here
        since the robot can only go right or down, the dp equation is:
        dp[i][j] = dp[i-1][j] + dp[i][j-1] from either up or left
        and special treatment to boundary cases that
        dp[0][j] = 1 only go right
        dp[i][0] = 1 only go down
        
        Also notice we can use a rolling row storage to reduce space complexity
        Time Complexity : O(m*n)
        Space Complexity : O(min(m,n))
        """
        
        # init the dp process using roll row storage
        m, n = min(m,n), max(m,n)
        old = [1] * n
        new = [1] + [0] * (n-1)
        
        # start the dp process
        for i in range(m-1):
            for j in range(1, n):
                new[j] = old[j] + new[j-1]
            old = new
            new = [1] + [0] * (n-1)
        
        # return answer
        return old[n-1]
```


  
