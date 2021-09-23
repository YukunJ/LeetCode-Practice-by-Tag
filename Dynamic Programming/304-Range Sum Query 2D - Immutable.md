**304. Range Sum Query 2D - Immutable**

```Tag : dynamic programming/presum/array/design```

**Description:**

Given a 2D matrix ```matrix```, handle multiple queries of the following type:

+ Calculate the **sum** of the elements of ```matrix``` inside the rectangle defined by its **upper left corner** ```(row1, col1)``` and lower right corner (row2, col2).

Implement the NumMatrix class:

+ ```NumMatrix(int[][] matrix)``` Initializes the object with the integer matrix ```matrix```.

+ ```int sumRegion(int row1, int col1, int row2, int col2)``` Returns the **sum** of the elements of ```matrix``` inside the rectangle defined by its **upper left corner** ```(row1, col1)``` and **lower right corner** ```(row2, col2)```.

**Example1:**

![avatar](Fig/304-E1.jpg)

        Input
        ["NumArray", "sumRange", "sumRange", "sumRange"]
        [[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
        Output
        [null, 1, -1, -3]

        Explanation
        NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
        numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1
        numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1
        numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3

-----------

```python
class NumMatrix:
    """
    Similarly to Question 303, we use dynamic programming to store 2D version of presum
    denote presum[i][j] := the sum of the rectangle using matrix[i-1][j-1] as bottom right corner
    then sumRegion(row1, col1, row2, col2) = presum[row2+1][col2+1] - presum[row1][col2+1] - presum[row2+1][col1] + presum[row1][col1]
    (you double subtract the top-left small rectangle, need to make up for it)
    
    denote n, m := matrix.shape
    Time Complexity := __init__() takes O(n*m), sumRegion() takes O(1)
    Space Complexity := O(n*m)
    """
    def __init__(self, matrix: List[List[int]]):
        n, m = len(matrix), len(matrix[0])
        self.presum_matrix = [[0] * (m+1)]
        for i in range(n):
            curr_level = [0] # shallow copy
            for j in range(m):
                curr_level.append(curr_level[-1] + matrix[i][j])

            for k in range(m+1):
                curr_level[k] += self.presum_matrix[-1][k]

            self.presum_matrix.append(curr_level)
        print(self.presum_matrix)
        
    def sumRegion(self, row1: int, col1: int, row2: int, col2: int) -> int:
        area1 = self.presum_matrix[row1][col1]
        area2 = self.presum_matrix[row1][col2+1]
        area3 = self.presum_matrix[row2+1][col1]
        area4 = self.presum_matrix[row2+1][col2+1]

        return area4 - area3 - area2 + area1
        


# Your NumMatrix object will be instantiated and called as such:
# obj = NumMatrix(matrix)
# param_1 = obj.sumRegion(row1,col1,row2,col2)
```
