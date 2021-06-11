**200. Number of Islands**

```Tag: DFS/BFS/Disjoint Set Union```

**Description:**

Given an ```m x n``` 2D binary grid ```grid``` which represents a map of '```1```'s (land) and '```0```'s (water), return the number of islands.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example1**:

        Input: grid = [
        ["1","1","1","1","0"],
        ["1","1","0","1","0"],
        ["1","1","0","0","0"],
        ["0","0","0","0","0"]
        ]
        Output: 1


**Example2**:

        Input: grid = [
        ["1","1","0","0","0"],
        ["1","1","0","0","0"],
        ["0","0","1","0","0"],
        ["0","0","0","1","1"]
        ]
        Output: 3

-----------

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        """
        This is a DFS/BFS problem on multiple starting sources
        We start from any "island" position and go as far as possible until we reach sea
        In a sense, we are counting the number of maximum connected component this the graph

        denote m, n := grid.shape
        Time Complexity : O(m*n) we visit each cell at most once
        Space Complexity : O(m*n) the dfs recursion stack is at most m*n height if all are islands == '1'
        """
        Islands = 0 # number of islands
        m, n = len(grid), len(grid[0])

        def dfs(row: int, col: int):
            """
            DFS search starting on current position,
            go as far as possible
            """
            if not (0 <= row < m and 0 <= col < n) or (grid[row][col] != '1'):
                # either go beyond grid scope, or already visited
                return 
            
            grid[row][col] = '0' # change to water, don't visit it again
            # try go 4 directions
            dfs(row-1, col)
            dfs(row+1, col)
            dfs(row, col-1)
            dfs(row, col+1)
            return 
        
        for row in range(m):
            for col in range(n):
                if grid[row][col] == '1': 
                    # the beginning of an island, dfs/bfs search on it
                    dfs(row, col)
                    # record the current connected component
                    Islands += 1
        return Islands
```
