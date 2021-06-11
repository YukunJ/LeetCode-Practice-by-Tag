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

**Solution1(Disjoint Set Union)**:

```python
class DUS:
    """Disjoint Set Union Data Structure Tailored"""
    def __init__(self, grid):
        self.m, self.n = len(grid), len(grid[0])
        self.count = 0 # number of connected component
        self.parent = [-1] * (self.m*self.n)
        self.rank = [0] * (self.m*self.n)
        for row in range(self.m):
            for col in range(self.n):
                if grid[row][col] == '1': # island
                    self.parent[row*self.n+col] = row*self.n+col
                    self.count += 1
    
    def find(self, i): # path compression
        if self.parent[i] != i:
            self.parent[i] = self.find(self.parent[i])
        return self.parent[i]
    
    def union(self, p, q): # union by rank
        leaderp, leaderq = self.find(p), self.find(q)
        if leaderp != leaderq:
            if self.rank[leaderp] > self.rank[leaderq]:
                # make leaderp is lower-rank one
                leaderp, leaderq = leaderq, leaderp
            self.parent[leaderp] = leaderq
            self.count -= 1
            if self.rank[leaderp] == self.rank[leaderq]:
                self.rank[leaderq] += 1
    
    def getCount(self):
        return self.count

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        """
        We use a Disjoint Set Union data structure to solve this problem
        denote m, n := grid.shape
        
        Time Complexity : O(m*n*a(m*n)),
            where a is the inverse of "tower-of-two" function, which is basically constant
        Space Complexity : O(m*n) for the extra data structure storage
        """
        myDUS = DUS(grid)
        m, n = len(grid), len(grid[0])
        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1': # island, start union merging
                    grid[i][j] = '0'
                    for x, y in [(i-1,j), (i+1,j), (i,j-1), (i,j+1)]:
                        if (0 <= x < m) and (0 <= y < n) and grid[x][y] == '1':
                            myDUS.union(i*n+j, x*n+y)
        return myDUS.getCount()
```


-----------

**Solution2(DFS)**:

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
