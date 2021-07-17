**54. Spiral Matrix**

```Tag: Math/Matrix```

**Description:**

Given a positive integer ```n```, generate an ```n x n``` matrix filled with elements from ```1``` to ```n^2``` in spiral order.

**Example1**:

![avatar](Fig/59-E1.jpg)

        Input: n = 3
        Output: [[1,2,3],[8,9,4],[7,6,5]]

**Example2**:

        Input: n = 1
        Output: [[1]]

-----------

```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        """
        This is much a similar question to Q54 <Spiral Matrix>
        after we initiate the matrix space, we again iterate the matrix "peeling the Onion" way
        (left, low) -> (right, low)
        (right, low) -> (right, high)
        (right, high) -> (left, high)
        (left, high) -> (left, low)
        and then we shrink the frame by left, low += 1, right, high -= 1
        
        denote n := matrix.length
        Time Complexity : O(n^2)
        Space Complexity : O(1) we don't use extra space except the one for answer storage
        """
        num = 1
        matrix = [[0] * n for _ in range(n)]
        left, right, low, high = 0, n-1, 0, n-1
        while left <= right and low <= high:
            # going right
            for col in range(left, right+1):
                matrix[low][col] = num
                num += 1
            # going down
            for row in range(low+1, high+1):
                matrix[row][right] = num
                num += 1
            if left < right and low < high:
                # going left
                for col in range(right-1, left-1, -1):
                    matrix[high][col] = num
                    num += 1
                # going up
                for row in range(high-1, low, -1):
                    matrix[row][left] = num
                    num += 1
            left, right, low, high = left+1, right-1, low+1, high-1
        return matrix
```
