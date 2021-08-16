**1428. Leftmost Column with at Least a One**

```Tag : binary search```

**Description:**

(This problem is an **interactive problem**.)

A **row-sorted binary matrix** means that all elements are ```0``` or ```1``` and each row of the matrix is sorted in non-decreasing order.

Given a **row-sorted binary matrix** ```binaryMatrix```, return the index (0-indexed) of the **leftmost column** with a 1 in it. If such an index does not exist, return ```-1```.

**You can't access the Binary Matrix directly**. You may only access the matrix using a ```BinaryMatrix``` interface:

+ ```BinaryMatrix.get(row, col)``` returns the element of the matrix at index ```(row, col)``` (0-indexed).

+ ```BinaryMatrix.dimensions()``` returns the dimensions of the matrix as a list of 2 elements ```[rows, cols]```, which means the matrix is ```rows x cols```.

Submissions making more than ```1000``` calls to ```BinaryMatrix.get``` will be judged Wrong Answer. Also, any solutions that attempt to circumvent the judge will result in disqualification.

For custom testing purposes, the input will be the entire binary matrix ```mat```. You will not have access to the binary matrix directly.

**Example1:**

[!avatar](Fig/1428-E1.jpg)

		Input: mat = [[0,0],[1,1]]
		Output: 0

**Example2:**

[!avatar](Fig/1428-E2.jpg)

		Input: mat = [[0,0],[0,1]]
		Output: 1

**Example3:**

[!avatar](Fig/1428-E3.jpg)

		Input: mat = [[0,0],[0,0]]
		Output: -1

**Example4:**

[!avatar](Fig/1428-E4.jpg)

		Input: mat = [[0,0,0,1],[0,0,1,1],[0,1,1,1]]
		Output: 1



**Hints:**

+ 1. (Binary Search) For each row do a binary search to find the leftmost one on that row and update the answer.

+ 2. (Optimal Approach) Imagine there is a pointer p(x, y) starting from top right corner. p can only move left or down. If the value at p is 0, move down. If the value at p is 1, move left. Try to figure out the correctness and time complexity of this algorithm.

-----------

```python
"""
# """
# This is BinaryMatrix's API interface.
# You should not implement it, or speculate about its implementation
# """
#class BinaryMatrix(object):
#    def get(self, row: int, col: int) -> int:
#    def dimensions(self) -> list[]:

class Solution:
    def leftMostColumnWithOne(self, binaryMatrix: 'BinaryMatrix') -> int:
        """
        We follow the hints:
        starting with a pointer at the top right position
        If the value is 0, we can move down 1 position, 
        i.e. abandon the entire row because the row is ascendingly sorted
        If the value is 1, we record the answer and move left 1 position,
        i.e. abandon all column index larger than this one, because we already had a "smaller" answer found
        denote n, m := binaryMatrix.shape
        Time Complexity : O(n+m)
        Space Complexity : O(1)
        """
        n, m = binaryMatrix.dimensions()
        row, col = 0, m-1
        ans = -1
        while row <= n-1 and col >= 0:
            # traversal and matrix by either go left or go down
            peek = binaryMatrix.get(row, col)
            if peek == 1:
                ans = col
                col -= 1
            else:
                row += 1
        return ans
```
