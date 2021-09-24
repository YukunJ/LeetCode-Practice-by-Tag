**120. Triangle**

```Tag : dynamic programming/array```

**Description:**

Given a ```triangle``` array, return the *minimum path sum from top to bottom*.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index ```i``` on the current row, you may move to either index ```i``` or index ```i + 1``` on the next row.

**Example1:**

        Input: triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
        Output: 11
        Explanation: The triangle looks like:
        2
        3 4
        6 5 7
        4 1 8 3
        The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above).

**Example2:**

        Input: triangle = [[-10]]
        Output: -10

**Follow up**: Could you do this using only ```O(n)``` extra space, where ```n``` is the total number of rows in the triangle?

-----------

```python
class Solution:
    def minimumTotal(self, triangle: List[List[int]]) -> int:
        """
        We adopt a dynamic programming approach to top-down sift
        to fully optimize space complexity, we realize that
        the current row only depends on last row and no further previous
        it indicates that we could adopt a rotation vector to space space
        
        denote n := len(triangle)
        Time Complexity : O(n^2) there are n*(n+1)/2 slots in the triangle
        Space Complexity : O(n) each row is at most of O(n) length
        """
        n = len(triangle)
        lastrow = triangle[0]
        for i in range(1, n):
            m = len(triangle[i])
            newrow = [0] * m
            for j in range(m):
                # careful about the left-most and right-most boundary on each row
                upper = min(j, len(lastrow)-1)
                upper_left = max(0, j-1)
                newrow[j] = triangle[i][j] + min(lastrow[upper], lastrow[upper_left])
            lastrow = newrow
        return min(lastrow)
```
