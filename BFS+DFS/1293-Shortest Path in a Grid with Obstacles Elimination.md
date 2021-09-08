**1293. Shortest Path in a Grid with Obstacles Elimination**

```Tag : bfs/matrix/array```

**Description:**

You are given an ```m x n``` integer matrix ```grid``` where each cell is either ```0``` (empty) or ```1``` (obstacle). You can move up, down, left, or right from and to an empty cell in **one step**.

Return the *minimum number* of **steps** to walk from the upper left corner ```(0, 0)``` to the lower right corner ```(m - 1, n - 1)``` given that you can eliminate **at most** ```k``` obstacles. If it is not possible to find such walk return ```-1```.


**Example1:**

        Input: 
        grid = 
        [[0,0,0],
        [1,1,0],
        [0,0,0],
        [0,1,1],
        [0,0,0]], 
        k = 1
        Output: 6
        Explanation: 
        The shortest path without eliminating any obstacle is 10. 
        The shortest path with one obstacle elimination at position (3,2) is 6. Such path is (0,0) -> (0,1) -> (0,2) -> (1,2) -> (2,2) -> (3,2) -> (4,2).
        
**Example2:**

        Input: 
        grid = 
        [[0,1,1],
        [1,1,1],
        [1,0,0]], 
        k = 1
        Output: -1
        Explanation: 
        We need to eliminate at least two obstacles to find such a walk.

**Hints**:

+ Use BFS

+ BFS on (x,y,r) x,y is coordinate, r is remain number of obstacles you can remove.

-----------

```python
class Solution:
    def shortestPath(self, grid: List[List[int]], k: int) -> int:
        """
        To find the shortest path, we will consider BFS algorithm
        However, here we must also consider the problem of "elimination obstacles"
        to quantify a state in the search, we will write (x, y, r) where x, y are coordinates and r is remaining elinimations
        when we try to move a new position, after checking it's within the boundary
        we check:
        1) if it's 0,  we are free to go 
        2) if it's 1, we need at least 1 remaining elinimation chances to move there
        
        denote n, m := grid.shape
        there are at most n*m*k states to be searched and stored in hashset, each search step is O(1)
        Time Complexity : O(n*m*k)
        Space Complexity : O(n*m*k)
        """
        
        from collections import deque
        visited = set()
        n, m = len(grid), len(grid[0])
        
        # a preliminary check, in straight manhattan distance
        # we need to move n + m -2 steps
        if k >= n + m -2:
            return n + m -2
        
        # BFS search
        queue = deque([(0, 0, 0, k)])
        visited.add((0, 0, k))
        while queue:
            x, y, step, r = queue.popleft()
            if x == n - 1 and y == m - 1: 
                # reach the destination, by BFS this is the minimum step
                return step
            for next_x, next_y in [(x-1, y), (x+1, y), (x, y-1), (x, y+1)]:
                if 0 <= next_x < n and 0 <= next_y < m: # within boundary check
                    if grid[next_x][next_y] == 0 and (next_x, next_y, r) not in visited: 
                        # no obstacle, free to go, not visited yet
                        visited.add((next_x, next_y, r))
                        queue.append((next_x, next_y, step+1, r))
                    if grid[next_x][next_y] == 1 and r-1 >= 0 and (next_x, next_y, r-1) not in visited:
                        # has obstacle, can eliminate, not visited yet
                        visited.add((next_x, next_y, r-1))
                        queue.append((next_x, next_y, step+1, r-1))
                        
        # failure to reach the destination
        return -1
```


