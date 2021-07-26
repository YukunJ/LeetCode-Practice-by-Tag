**1007. Minimum Domino Rotations For Equal Row**

```Tag : swap```

**Description:**

In a row of dominoes, ```tops[i]``` and ```bottoms[i]``` represent the top and bottom halves of the ```i-th``` domino. (A domino is a tile with two numbers from 1 to 6 - one on each half of the tile.)

We may rotate the ```i-th``` domino, so that ```tops[i]``` and ```bottoms[i]``` swap values.

Return the minimum number of rotations so that all the values in tops are the same, or all the values in bottoms are the same.

If it cannot be done, return ```-1```.

**Example1**:

![avatar](Fig/1007-E1.png)

        Input: tops = [2,1,2,4,2,2], bottoms = [5,2,6,2,3,2]
        Output: 2
        Explanation: 
        The first figure represents the dominoes as given by tops and bottoms: before we do any rotations.
        If we rotate the second and fourth dominoes, we can make every value in the top row equal to 2, as indicated by the second figure.
        
**Example2**:      

        Input: tops = [3,5,1,2,3], bottoms = [3,6,3,3,4]
        Output: -1
        Explanation: 
        In this case, it is not possible to rotate the dominoes to make one row of values equal.
 
-----------

```python
class Solution:
    def minDominoRotations(self, tops: List[int], bottoms: List[int]) -> int:
        """
        A quick observation is that, if a line could be all the same,
        the number has to be either tops[0] or bottoms[0]
        we could use tops[0] and bottoms[0] as representative
        to check if there is a way to make a line same to tops[0] or bottoms[0]
        need to be careful when at some index i, tops[i] == bottoms[i]
        need to use "elif", not "else" in condition evaluation
        
        denote n := len(tops)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        
        def rotate(target: int) -> int:
            """Helper function: try to make a line same as target"""
            rotate_top, rotate_bottom = 0, 0
            for i in range(len(tops)):
                if tops[i] != target and bottoms[i] != target:
                    # impossinle to rotate
                    return -1
                elif tops[i] != target:
                    # top to bottom
                    rotate_bottom += 1
                elif bottoms[i] != target:
                    # bottom to top
                    rotate_top += 1
            return min(rotate_top, rotate_bottom)
        
        target_up, target_bottom = tops[0], bottoms[0]
        rotate_top = rotate(target_up)
        if target_up == target_bottom or rotate_top != -1:
            return rotate_top
        else:
            return rotate(target_bottom)
```
