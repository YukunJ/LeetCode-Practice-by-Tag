**37. Sudoku Solver**

```Tag: DFS```

**Description:**

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:

+ Each of the digits 1-9 must occur exactly once in each row.
+ Each of the digits 1-9 must occur exactly once in each column.
+ Each of the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.

The ```'.'``` character indicates empty cells.

**Example1**:

![avatar](Fig/37-E1.png)

        Input: board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
        Output: [["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
        Explanation: The input board is shown above and the only valid solution is shown below:

![avatar](Fig/37-E1-ans.png)

-----------

```python
class Solution:
    def solveSudoku(self, board: List[List[str]]) -> None:
        """
        A DFS problem.
        denote m, n := board.shape
        When we want to fill in a slot, there are 3 conditions to be specified:
        1) This row doesn't contain this number yet
        2) This col doesn't contain this number yet
        3) This 3x3 box doesn't contain this number yet
        When any one of these conditions fail, we end the current dfs search
        we proceed in a row-major order manner, so the success condition is that
        we reach the m+1 row 1st col, meaning we've finished the Sudoku and no failure yet
        we use hashtable to take a recordings of 9 rows, 9 cols and 9 boxes.
        Time Complexity : O(9^81) there are at most 81 slots to be filled, each with at most 9 possibilities. Hash table look up takes O(1)
        Space Complexity : O(3*81) we use row, col, box order to store the whole table using hashtables
        """
        def get_box(row: int, col: int) -> int:
            """ helper func. Given row index and col index, return the box number it belongs to """
            # a bit tedious, but should be ok
            return row//3 * 3 + col//3

        from collections import defaultdict
        self.signal = False # signal to notify if cancelling the board is needed
        m, n = len(board), len(board[0])
        rows = [defaultdict(bool) for _ in range(9)] 
        cols = [defaultdict(bool) for _ in range(9)]
        boxes = [defaultdict(bool)for _ in range(9)]
        pick = ['1', '2', '3', '4', '5', '6', '7', '8', '9']
        # record the pre-given numbers appearance in hashtable
        for row in range(m):
            for col in range(n):
                num = board[row][col]
                if num != '.':
                    box = get_box(row, col)
                    boxes[box][num] = True
                    rows[row][num] = True
                    cols[col][num] = True

        def dfs(index: int):
            """ the index is from 0 to 80, in the row-major order """
            if index == m*n: # success reached
                self.signal = True
                return 
            # get the current position
            row, col = index // 9, index % 9
            box = get_box(row, col)
            num = board[row][col]

            if num != '.': # already filled, just proceed
                dfs(index+1)
            else:
                # we can pick a number to fill in
                for i in pick:
                    if (not rows[row][i]) and (not cols[col][i]) and (not boxes[box][i]):
                        # currently, could pick and fill
                        board[row][col] = i
                        rows[row][i] = True
                        cols[col][i] = True
                        boxes[box][i] = True

                        dfs(index+1) # continue the dfs search
                        if self.signal:
                            return 
                        board[row][col] = '.'
                        rows[row][i] = False
                        cols[col][i] = False
                        boxes[box][i] = False
                    else:
                        continue

        dfs(0)
```
