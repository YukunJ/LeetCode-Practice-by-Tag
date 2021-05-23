**63. Unique Paths II**

```Tag: dynamic programming```

**Description:**

A robot is located at the top-left corner of a ```m x n``` grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and space is marked as ```1``` and ```0``` respectively in the grid.

**Example1**:

![avatar](Fig/63-E1.jpg)

        Input: obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
        Output: 2
        Explanation: There is one obstacle in the middle of the 3x3 grid above.
        There are two ways to reach the bottom-right corner:
        1. Right -> Right -> Down -> Down
        2. Down -> Down -> Right -> Right
        
**Example2**:

![avatar](Fig/63-E2.jpg)

        Input: obstacleGrid = [[0,1],[0,0]]
        Output: 1
        
-----------

```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        """
        Similar to question 62 Unique Path
        use dynamic programming and rolling vector to save space
        
        Time Complexity : O(m*n)
        Space Complexity : O(n)
        """
        m, n = len(obstacleGrid), len(obstacleGrid[0])
        old = ([1] if not obstacleGrid[0][0] else [0]) + [0] * (n-1)
        for j in range(1, n):
            if obstacleGrid[0][j]:
                break
            old[j] = old[j-1]
        
        for i in range(1, m):
            new = ([0] if obstacleGrid[i][0] else [old[0]]) + [0] * (n-1)
            for j in range(1, n):
                if not obstacleGrid[i][j]:
                    new[j] = new[j-1] + old[j]
            old = new
        
        return old[n-1]
```


  
