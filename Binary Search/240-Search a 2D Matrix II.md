**154. Find Minimum in Rotated Sorted Array II**

```Tag: binary search```

**Description:**

Write an efficient algorithm that searches for a target value in an m x n integer matrix. The matrix has the following properties:

+ Integers in each row are sorted in ascending from left to right.
+ Integers in each column are sorted in ascending from top to bottom.

**Example1**:

![avatar](Fig/240-E1.jpg)

        Input: matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
        Output: true

**Example2**:

![avatar](Fig/240-E2.jpg)

        Input: matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
        Output: false

-----------

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        """
        To write an efficient algorithm,
        we use binary search starting at the up-right corner
        
        when standing at M[i][j], we discard M[:i][:j+1] the up-right space
        if M[i][j] > target: move to M[i][j-1]
        if M[i][j] < target: move to M[i+1][j]
        
        Time Complexity : O(m+n) since only going left or down
        Space Complexity : O(1)
        """
        if not matrix:
            return False
        i, j = 0, len(matrix[0])-1
        
        while i <= len(matrix)-1 and j >= 0:
            # search until step outside of matrix
            if target == matrix[i][j]:
                return True
            elif target > matrix[i][j]:
                i += 1
            else: # target < matrix[i][j]
                j -= 1
                
        # Not Found      
        return False 
```
