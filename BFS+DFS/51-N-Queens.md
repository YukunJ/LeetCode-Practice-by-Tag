**51. N-Queens**

```Tag : DFS/Backtracking```

**Description:**

The **n-queens** puzzle is the problem of placing ```n``` queens on an ```n x n``` chessboard such that no two queens attack each other.

Given an integer ```n```, return all **distinct** solutions to the **n-queens** puzzle. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where ```'Q'``` and ```'.'``` both indicate a queen and an empty space, respectively.


**Example1**:

![avatar](Fig/51-E1.jpeg)

		Input: n = 4
		Output: [[".Q..","...Q","Q...","..Q."], ["..Q.","Q...","...Q",".Q.."]]
		Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above

 
**Example2**:
 
		Input: n = 1
		Output: [["Q"]]
      
-----------

**Solution1: Not Optimized DFS Backtracking**

```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        """
        This is a difficult DFS & Backtracking problem
        A queen can go row-wise, column-wise, or diagonal-wise(two diagonals exist)
        essentially we are enumerating all possible positions for N queens, with pruning and early-stopping
        Clearly, every row can only have 1 queen, every column same
        We can iterate every row, to place a queen on that row at some column
        it's easier to test if the column is already occupied, but how about the diagonal
        we could observation that, given position(i, j)
        1) on same diagonal1(left top to right bottom) is such that i-j is constant
        2) on same diagonal2(right top to left bottom) is such that i+j is constant
        Therefore, we use 3 hashset to check memberships there for O(1) checking time
        first row queen has N choice, second row N-1, third row N-2, ... 
        there are in total N-Permutation N! possibility we need to search

        Time Complexity : O(n!)
        Space Complexity : O(n)
        """
        def generateBoard():
            """Helper function: generate a board based on queen row info"""
            board = []
            for i in range(n):
                row[queen[i]] = 'Q' # indicate the queen position
                board.append(''.join(row))
                row[queen[i]] = '.' # reset
            return board
        
        def dfs(row: int):
            if row == n: # end of a search, found solution
                board = generateBoard()
                res.append(board)
            else:
                for col in range(n):
                    if (col in column) or (row-col in diagonal1) or (row+col in diagonal2):
                        # queen collision
                        continue
                    queen[row] = col
                    column.add(col)
                    diagonal1.add(row-col)
                    diagonal2.add(row+col)
                    dfs(row+1) # further search next row
                    # reset all the positions
                    column.remove(col)
                    diagonal1.remove(row-col)
                    diagonal2.remove(row+col)
        
        res = []
        row = ['.'] * n
        queen = [-1] * n
        column, diagonal1, diagonal2 = set(), set(), set()
        dfs(0)
        return res
```

-----------

**Solution2: Optimized Bitwise DFS Backtracking**

```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        """
        We could use 3 integer to do bitwise operation instead of using 3 hashsets as in solution 1
        two important bitwise operations we need to know:
        1) x & (-x): retain only the last appearing '1'
        2) x & (x-1): cancel the last appearing '1'
        Time Complexity : O(n!)
        Space Complexity : O(n)
        """
        def generateBoard():
            """Helper function: generate a board based on queen row info"""
            board = []
            for i in range(n):
                row[queen[i]] = 'Q' # indicate the queen position
                board.append(''.join(row))
                row[queen[i]] = '.' # reset
            return board
        
        def dfs(row: int, columns: int, diagonal1: int, diagonal2: int):
            if row == n: # end of a search, found solution
                board = generateBoard()
                res.append(board)
            else:
                availablePositions = ((1 << n) -1) & (~(columns | diagonal1 | diagonal2))
                while availablePositions:
                    position = availablePositions & (-availablePositions) # retain the last '1'
                    availablePositions = availablePositions & (availablePositions-1) # cancel this position in available
                    col = bin(position-1).count('1') # start from right to left
                    queens[row] = col
                    dfs(row+1, columns | position, (diagonal1 | position) << 1, (diagonal2 | position) >> 1)

        res = []
        row = ['.'] * n
        queen = [-1] * n
        dfs(0, 0, 0, 0)
        return res
```