**85. Maximal Rectangle**

```Tag: dynamic programming/monotone stack```

**Description:**

Given a ```rows x cols``` binary ```matrix``` filled with ```0```'s and ```1```'s, find the largest rectangle containing only ```1```'s and return *its area*.

**Example1**:

![avatar](Fig/85-E1.jpeg)

		Input: matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
		Output: 6
		Explanation: The maximal rectangle is shown in the above picture.
 
**Example2**:

		Input: matrix = []
		Output: 0

**Example3**:

		Input: matrix = [["0"]]
		Output: 0

**Example4**:
		
		Input: matrix = [["1"]]
		Output: 1

**Example5**:   

		Input: matrix = [["0","0"]]
		Output: 0	
    
-----------

**Solution1: Dynamic Programming**

```python
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        """
        we could use dynamic programming technique to solve the problem
        we use a dp[n][m] matrix to store the number of contiguous '1' element left to matrix[n][m] (including itself)
        then, we fix every matrix[n][m] as the right-bottom end point of a rectangle
        we try to "increase the rectangle" the height by possibly shrink the width of the rectangle
        i.e. 
        initially, width = dp[n][m], height = 1
        we move upward, width = min(width, dp[n-1][m]), height = 2
        until we hit a '0' or the upper boundary of the matrix

        denote n, m := matrix.shape
        Time Complexity : O(n^2*m) each position's search takes O(n), there are O(n*m) position to search
        Space Complexity : O(n*m) dp matrix storage
        """
        if not matrix: # boundary case
            return 0
        n, m = len(matrix), len(matrix[0])
        # the dp matrix creation
        dp = [[0] * m for _ in range(n)]
        for i in range(n):
            for j in range(m):
                if matrix[i][j] == '0':
                    dp[i][j] = 0
                    continue
                if j == 0:
                    dp[i][j] = 1
                else:
                    dp[i][j] = dp[i][j-1] + 1 # increment on last position
        
        ans = 0
        # search each position
        for i in range(n):
            for j in range(m):
                if dp[i][j] != 0:
                    height, width = 1, dp[i][j]
                    ans = max(ans, height * width)
                    for k in range(i-1, -1, -1): # search upward for better height
                        if dp[k][j] == 0: # hit a dead point, stop searching upward anymore
                            break
                        height += 1
                        width = min(width, dp[k][j]) # shrink the width
                        ans = max(ans, height * width)
        
        return ans
```

-----------

**Solution2: Monotone Stack**

```python
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        """
        A more optimized solution is to use Monotone stack, similar to Question 84.
        We apply the 1-D solution of Question 84 to every row of the matrix here

        denote n, m := matrix.shape
        Time Complexity : O(n*m)
        Space Complexity : O(m)
        """
        if not matrix: # boundary case
            return 0
        n, m = len(matrix), len(matrix[0])
        heights = [0] * m
        ans = 0
        for row in range(n):
            for col in range(m):
                # first update the height of columns
                if matrix[row][col] == '0':
                    heights[col] = 0
                else:
                    heights[col] += 1 # accumulate on last row
            # use the 1-D solution repeatedly
            ans = max(ans, self.largestRectangleArea(heights))
        return ans

    def largestRectangleArea(self, heights: List[int]) -> int:
        """
        import from Question 84. Solving a 1-D max rectangle area problem

        denote m := len(heights)
        Time Complexity : O(m)
        Space Complexity : O(m)
        """
        n = len(heights)
        left, right = [0] * n, [0] * n
        mono_stack = []

        # find the first left lower position
        for i in range(n):
            while mono_stack and heights[mono_stack[-1]] >= heights[i]:
                mono_stack.pop()
            # use -1 as the left-most guard
            left[i] = mono_stack[-1] if mono_stack else -1
            mono_stack.append(i)
        
        # find thr first right lower position
        mono_stack.clear()
        for i in range(n-1, -1, -1):
            while mono_stack and heights[mono_stack[-1]] >= heights[i]:
                mono_stack.pop() 
            # use n as the right-most guard
            right[i] = mono_stack[-1] if mono_stack else n
            mono_stack.append(i)
        
        ans = [(right[i]-left[i]-1) * heights[i] for i in range(n)]
        return max(ans)
```
