**498. Diagonal Traverse**

```Tag: Math/Matrix```

**Description:**

Given an ```m x n``` matrix ```mat```, return an array of all the elements of the array in a **diagonal order**.

**Example1:**

![avatar](Fig/498-E1.jpeg)

		Input: mat = [[1,2,3],[4,5,6],[7,8,9]]
		Output: [1,2,4,7,5,3,6,8,9]


**Example2:**

		Input: mat = [[1,2],[3,4]]
		Output: [1,2,3,4]


-----------

```python
class Solution:
    def findDiagonalOrder(self, mat: List[List[int]]) -> List[int]:
        """
        One key observation in this question is,
        when you on a diagonal direction, 
        anywhere differ by (module 2=0) is on the same direction, and differ by (module 2=1) is on the other direction
        in specific, when at index (i,j), (i+j)%2=0 is on up-right direction, (i+j)%2=1 is on the down-left direction
        The we can follow that direction all the way until we go out of boundary and change direction again
        be careful here that the matrix might not be a square matrix, we should be careful about row matrix and column matrix shape

        denote m, n := mat.shape
        Time Complexity : O(m*n)
        Space Complexity : O(1)
        """
        if (not mat) or (not mat[0]): # boundary checking
            return []
        row_max, col_max = len(mat)-1, len(mat[0])-1 # the boundary
        row, col = 0, 0 # current position
        ans = [] # answer storage
        while row <= row_max and col <= col_max:
            ans.append(mat[row][col])
            if (row+col) % 2 == 0: # on the up-right direction
                # try make further up-right step
                while 0 <= row-1 and col+1 <= col_max:
                    row, col = row-1, col+1
                    ans.append(mat[row][col])
                if col+1 <= col_max:
		    # unless at last column, go right first
                    col += 1
                else:
                    row += 1
            elif (row+col) % 2 == 1: # on the down-left direction
                # try make further down-left step
                while row+1 <= row_max and 0 <= col-1:
                    row, col = row+1, col-1
                    ans.append(mat[row][col])
                if row+1 <= row_max: 
		    # unless at last row, go down first
                    row += 1
                else:
                    col += 1
        return ans  
```
