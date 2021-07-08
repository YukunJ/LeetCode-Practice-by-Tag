**64. Minimum Path Sum**

```Tag: dynamic programming```

**Description:**

Given a ```m x n``` ```grid``` filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

**Note**: You can only move either down or right at any point in time.

**Example1**:

![avatar](Fig/64-E1.jpg)

        Input: grid = [[1,3,1],[1,5,1],[4,2,1]]
        Output: 7
        Explanation: Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.
        
**Example2**:

        Input: grid = [[1,2,3],[4,5,6]]
        Output: 12

-----------

```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        """
        A very basic dynamic programming problem
        State transition is:
        denote dp[i][j]:=the min cost reaching row i col j
        since we can only move right or down,
        dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])
        
        Notice the transition only depends on last row, so we can optimize space complexity
        denote m, n = grid.shape
        Time Complexity : O(m*n)
        Space Complexity : O(n)
        """
        m, n = len(grid), len(grid[0])
        old = [grid[0][0]] + [0] * (n-1)
        for col in range(1, n): # first row, only going right
            old[col] = grid[0][col] + old[col-1]
        
        for row in range(1, m):
            # first column only going down
            new = [old[0]+grid[row][0]] + [0] * (n-1)
            for col in range(1, n):
                new[col] = min(new[col-1], old[col]) + grid[row][col]
            old = new
        
        return old[-1]
```
