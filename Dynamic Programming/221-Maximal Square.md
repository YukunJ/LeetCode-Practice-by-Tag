**221. Maximal Square**

```Tag: dynamic programming```

**Description:**

Given an ```m x n``` binary ```matrix``` filled with ```0```'s and ```1```'s, find the largest square containing only ```1```'s and return its area.

**Example1**:

![avatar](Fig/221-E1.jpg)

        Input: matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
        Output: 4

**Example2**:

![avatar](Fig/221-E2.jpg)

        Input: matrix = [["0","1"],["1","0"]]
        Output: 1
        
**Example3**:

        Input: matrix = [["0"]]
        Output: 0

-----------

```python
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        """
        we use a dynamic programming scheme
        denote dp[i][j] := the max square edge length with matrix[i][j] as bottom-right element
        assume dp[i][j] = x
        then, by deleting a row or a column, we see at least we should have:
        I) dp[i-1][j] >= x-1
        II) dp[i][j-1] >= x-1
        III) dp[i-1][j-1] >= x-1
        therefore we conclude that dp[i][j] = min(I, II, III) + 1
        be careful about the boundary first row and first column
        and we may use a rolling vector to save space Complexity
        
        Time Complexity : O(m*n)
        Space Complexity : O(n) by rolling row vector
        """
        # init the dp vector
        # be careful, the input matrix is of type "str", change to "int"
        m, n = len(matrix), len(matrix[0])
        old = [int(ele) for ele in matrix[0]]
        max_edge = max(old)
        
        for i in range(1, m):
            new = [int(ele) for ele in matrix[i]]
            for j in range(1, n):
                if matrix[i][j] == '1':
                    new[j] = min(old[j-1], old[j], new[j-1]) + 1
            max_edge = max(max_edge, max(new))
            old = new
        
        # return max area as square of max edge length
        return max_edge ** 2
```
