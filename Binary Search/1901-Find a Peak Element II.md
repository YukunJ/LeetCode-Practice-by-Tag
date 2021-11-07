**1901. Find a Peak Element II**

```Tag : binary search/matrix/divide and conquer```

**Description:**

A **peak** element in a 2D grid is an element that is **strictly greater** than all of its **adjacent** neighbors to the left, right, top, and bottom.

Given a **0-indexed** ```m x n``` matrix mat where **no two adjacent cells are equal**, find any peak element ```mat[i][j]``` and return the length 2 array ```[i,j]```.

You may assume that the entire matrix is surrounded by an **outer perimeter** with the value ```-1``` in each cell.

You must write an algorithm that runs in ```O(m log(n))``` or ```O(n log(m))``` time.

**Example1:**

![avatar](Fig/1901-E1.png)

        Input: mat = [[1,4],[3,2]]
        Output: [0,1]
        Explanation: Both 3 and 4 are peak elements so [1,0] and [0,1] are both acceptable answers.

**Example2:**

![avatar](Fig/1901-E2.png)

        Input: mat = [[10,20,15],[21,30,14],[7,16,32]]
        Output: [1,1]
        Explanation: Both 30 and 32 are peak elements so [1,1] and [2,2] are both acceptable answers.

-----------

```python
class Solution:
    def findPeakGrid(self, mat: List[List[int]]) -> List[int]:
        """
        Follow the hint, we will do binary search on Columns
        By fully iterating each row, we essentially have checked vertically
        We mimic the solution in 1D-find peak element problem
        
        denote n, m = mat.shape
        Time Complexity : O(n*log(m))
        Space Complexity : O(1)
        """
        n, m = len(mat), len(mat[0])
        left, right = 0, m-1
        while left <= right:
            mid = left + (right-left) // 2
            go_left = False
            for i in range(n):
                if i < n-1 and mat[i+1][mid] >= mat[i][mid]:
                    continue
                if i > 0 and mat[i-1][mid] >= mat[i][mid]:
                    continue
                if mid < m-1 and mat[i][mid+1] >= mat[i][mid]:
                    continue
                if mid > 0 and mat[i][mid-1] >= mat[i][mid]:
                    go_left = True
                    continue
                # if reach here, current searching point is already a peak
                return [i, mid]
            if go_left:
                right = mid - 1
            else:
                left = mid + 1
        return []
```
