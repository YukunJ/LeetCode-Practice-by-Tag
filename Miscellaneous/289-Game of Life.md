**289. Game of Life**

```Tag : array/matrix/simulation```

**Description:**

According to *Wikipedia's article*: "The **Game of Life**, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

The board is made up of an ```m x n``` grid of cells, where each cell has an initial state: **live** (represented by a ```1```) or **dead** (represented by a ```0```). Each cell interacts with its *eight neighbors* (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

+ Any live cell with fewer than two live neighbors dies as if caused by under-population.

+ Any live cell with two or three live neighbors lives on to the next generation.

+ Any live cell with more than three live neighbors dies, as if by over-population.

+ Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously. Given the current state of the ```m x n``` grid board, return the **next state**.

**Example1:**

![avatar](Fig/289-E1.jpeg)

        Input: board = [[0,1,0],[0,0,1],[1,1,1],[0,0,0]]
        Output: [[0,0,0],[1,0,1],[0,1,1],[0,1,0]]

**Example2:**

![avatar](Fig/289-E2.jpeg)

        Input: board = [[1,1],[1,0]]
        Output: [[1,1],[1,1]]

**Hints*:

+ Could you solve it in-place? Remember that the board needs to be updated simultaneously: You cannot update some cells first and then use their updated values to update other cells.

+ In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches upon the border of the array (i.e., live cells reach the border). How would you address these problems?

-----------

```python
class Solution:
    def gameOfLife(self, board: List[List[int]]) -> None:
        """
        The rule of this game is clear in itself
        But we need to solve it in O(1) space
        we could store extra information by small tricks as follows:
        dead -> dead = 0
        live -> live = 1
        dead -> live = 2
        live -> dead = 3
        In this way, we only need to take modulo 2 on this cell
        without need to know if it's modified or not.
        If the modulo 2 gives 0, it's a dead cell previously
        If the module 2 gives 1, it's a live cell previously
        
        denote n, m := board.shape
        Time Complexity : O(n*m)
        Space Complexity : O(1)
        """
        n, m = len(board), len(board[0])
        def countNeighbor(x: int, y: int) -> int:
            directions = [(-1, -1), (-1, 0), (-1, 1),
                          (0, -1), (0, 1), 
                          (1, -1), (1, 0), (1, 1)]
            live_neighbor = 0
            for dx, dy in directions:
                neighbor_x, neighbor_y = x+dx, y+dy
                if 0 <= neighbor_x < n and 0 <= neighbor_y < m:
                    live_neighbor += board[neighbor_x][neighbor_y] % 2
            return live_neighbor
        
        # one pass-through the board
        for i in range(n):
            for j in range(m):
                live_neighbor = countNeighbor(i, j)
                if board[i][j] % 2 == 1:
                    if live_neighbor < 2 or live_neighbor > 3:
                        board[i][j] = 3
                    else:
                        board[i][j] = 1
                elif board[i][j] % 2 == 0 and live_neighbor == 3:
                    # a dead cell originally, now live
                    board[i][j] = 2   
                    
        # clear extra information, generate next states for all cells
        for i in range(n):
            for j in range(m):
                if 1 <= board[i][j] <= 2:
                    board[i][j] = 1
                else:
                    board[i][j] = 0
        return
```


