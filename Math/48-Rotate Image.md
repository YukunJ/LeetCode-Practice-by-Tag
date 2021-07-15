**48. Rotate Image**

```Tag: Math/Matrix```

**Description:**

You are given an ```n x n``` 2D ```matrix``` representing an image, rotate the image by **90** degrees (clockwise).

You have to rotate the image **in-place**, which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

**Example1:**

![avatar](Fig/48-E1.jpeg)

		Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
		Output: [[7,4,1],[8,5,2],[9,6,3]]


**Example2:**

![avatar](Fig/48-E2.jpeg)

		Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
		Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]

**Example3:**

		Input: matrix = [[1]]
		Output: [[1]]

**Example4:**

		Input: matrix = [[1,2],[3,4]]
		Output: [[3,1],[4,2]]


-----------

```python
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        The single fact of rotating by 90 degree is:
        rotate[i, j] = origin[(n-1)-j, i]
        the only problem we are having here is that, we are required to rotate the matrix in place,
        for example consider a 3x3 matrix, 
        we want to do matrix[0,0] = matrix[2, 0]
        but after this, when we want to do 'matrix[0, 2] = matrix[0, 0]', the value of 'matrix[0, 0]' is polluted
        we need to remembr the value that got replaced by us in rotation
        we notice that there is a one-to-one mapping between every position in original matrix and rotate matrix
        therefore, we could do 4-pair-swap all at once
        consider the matrix as a x-y axis, we just need to iterate 1 quarter of the plane
        denote edge := n - 1
        1) rotate[i, j] = matrix[edge-j, i]
        2) rotate[edge-j, i] = matrix[edge-i, edge-j]
        3) rotate[edge-i, edge-j] = matrix[j, edge-i]
        4) rotate[j, edge-i] = matrix[i, j]
        also be careful about whether the side is odd or even

        denote n := len(matrix)       
        Time Complexity : O(n^2)
        Space Complexity : O(1)
        """
        edge = len(matrix) - 1
        quarter = (len(matrix)) // 2
        # this part shared by both odd or even edge length
        for i in range(quarter):
            for j in range(quarter):
                # 4 parallel assignments at once, thanks to Python
                matrix[i][j], matrix[edge-j][i], matrix[edge-i][edge-j], matrix[j][edge-i] = \
                matrix[edge-j][i], matrix[edge-i][edge-j], matrix[j][edge-i], matrix[i][j]

        if len(matrix) % 2 == 1:
            # this part is for odd edge length
            j = len(matrix) // 2 # fix column
            for i in range(len(matrix) // 2):
                 # 4 parallel assignments at once, thanks to Python
                matrix[i][j], matrix[edge-j][i], matrix[edge-i][edge-j], matrix[j][edge-i] = \
                matrix[edge-j][i], matrix[edge-i][edge-j], matrix[j][edge-i], matrix[i][j] 
         
        return
```