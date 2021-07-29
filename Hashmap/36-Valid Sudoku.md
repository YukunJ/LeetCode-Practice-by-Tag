**36. Valid Sudoku**

```Tag : hashtable```

**Description:**

Determine if a ```9 x 9``` Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

1. Each row must contain the digits ```1-9``` without repetition.

2. Each column must contain the digits ```1-9``` without repetition.

3. Each of the nine ```3 x 3``` sub-boxes of the grid must contain the digits ```1-9``` without repetition.

**Note:**

+ A Sudoku board (partially filled) could be valid but is not necessarily solvable.

+ Only the filled cells need to be validated according to the mentioned rules.


**Example1**:

![avatar](Fig/36-E1.png)

        Input: board = 
        [["5","3",".",".","7",".",".",".","."]
        ,["6",".",".","1","9","5",".",".","."]
        ,[".","9","8",".",".",".",".","6","."]
        ,["8",".",".",".","6",".",".",".","3"]
        ,["4",".",".","8",".","3",".",".","1"]
        ,["7",".",".",".","2",".",".",".","6"]
        ,[".","6",".",".",".",".","2","8","."]
        ,[".",".",".","4","1","9",".",".","5"]
        ,[".",".",".",".","8",".",".","7","9"]]
        Output: true
 
**Example2**:
 
         Input: board = 
         [["8","3",".",".","7",".",".",".","."]
         ,["6",".",".","1","9","5",".",".","."]
         ,[".","9","8",".",".",".",".","6","."]
         ,["8",".",".",".","6",".",".",".","3"]
         ,["4",".",".","8",".","3",".",".","1"]
         ,["7",".",".",".","2",".",".",".","6"]
         ,[".","6",".",".",".",".","2","8","."]
         ,[".",".",".","4","1","9",".",".","5"]
         ,[".",".",".",".","8",".",".","7","9"]]
         Output: false
         Explanation: Same as Example 1, except with the 5 in the top left corner being modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.

-----------

```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        """
        This is a question playing with hashSet
        we main 9 sets for row, 9 sets for column and 9 sets for blocks
        and iterate through every position of the board to validate it
        
        Since the board is 9 x 9 fixed size
        Time Complexity : O(1)
        Space Complexity : O(1)
        """
        row_sets = [set() for _ in range(9)]
        column_sets = [set() for _ in range(9)]
        block_sets = [set() for _ in range(9)]
        
        for i in range(9):
            for j in range(9):
                e = board[i][j]
                if e != ".":
                    block_index = 3 * (i // 3) + j // 3
                    if (e in row_sets[i]) or (e in column_sets[j]) or (e in block_sets[block_index]):
                        # already present, invalid Sudoku
                        return False
                    # update this element to memory
                    row_sets[i].add(e)
                    column_sets[j].add(e)
                    block_sets[block_index].add(e)
        return True
                        
```
