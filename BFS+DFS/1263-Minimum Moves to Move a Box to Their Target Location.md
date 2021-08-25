**1263. Minimum Moves to Move a Box to Their Target Location**

```Tag : bfs/matrix```

**Description:**

A storekeeper is a game in which the player pushes boxes around in a warehouse trying to get them to target locations.

The game is represented by an ```m x n``` grid of characters ```grid``` where each element is a wall, floor, or box.

Your task is to move the box ```'B'``` to the target position ```'T'``` under the following rules:

+ The character ```'S'``` represents the player. The player can move up, down, left, right in ```grid``` if it is a floor (empty cell).

+ The character ```'.'``` represents the floor which means a free cell to walk.

+ The character ```'#'``` represents the wall which means an obstacle (impossible to walk there).

+ There is only one box ```'B'``` and one target cell ```'T'``` in the grid.

+ The box can be moved to an adjacent free cell by standing next to the box and then moving in the direction of the box. This is a **push**.

+ The player cannot walk through the box.

Return the minimum number of **pushes** to move the box to the target. If there is no way to reach the target, return ```-1```.

**Example1:**

![avatar](Fig/1263-E1.png)

		Input: grid = [["#","#","#","#","#","#"],
               		["#","T","#","#","#","#"],
               		["#",".",".","B",".","#"],
               		["#",".","#","#",".","#"],
               		["#",".",".",".","S","#"],
               		["#","#","#","#","#","#"]]
		Output: 3
		Explanation: We return only the number of times the box is pushed.

**Example2:**

		Input: grid = [["#","#","#","#","#","#"],
               		["#","T","#","#","#","#"],
               		["#",".",".","B",".","#"],
               		["#","#","#","#",".","#"],
               		["#",".",".",".","S","#"],
               		["#","#","#","#","#","#"]]
		Output: -1

**Example3:**

		Input: grid = [["#","#","#","#","#","#"],
               		["#","T",".",".","#","#"],
               		["#",".","#","B",".","#"],
               		["#",".",".",".",".","#"],
               		["#",".",".",".","S","#"],
               		["#","#","#","#","#","#"]]
		Output: 5
		Explanation:  push the box down, left, left, up and up.

**Example4:**

		Input: grid = [["#","#","#","#","#","#","#"],
               		["#","S","#",".","B","T","#"],
               		["#","#","#","#","#","#","#"]]
		Output: -1

-----------


```python
class Solution:
    def minPushBox(self, grid: List[List[str]]) -> int:
        """
        This is a hard bfs question
        we follow the hint to represent the state as a 4-tuple (player_row, player_col, box_row, box_col)
        we only care about the number of pushes, therefore in each state we check if we could push the box one position forward
        to properly do BFS, we will hide the player's position, and only use the push count as the BFS's breadth
        The complexity in this question is quite tricky to analyze
        denote m, n := grid.shape
        every time we can "can_reach" this is a DFS search, which can at most takes O(m*n)
        In worst case, the box needs to be pushed O(m*n) positions before finding the target
        For the space storage, since we don't use recursion in this question, it's mainly the
        'stack' and 'visited', there can be at most m*n box position and m*n person position
        
        Time Complexity : O(m^2 * n^2)
        Space Complexity : O(m^2 * n^2) 
        """
        from collections import deque
        direct = [(0, 1), (0, -1), (1, 0), (-1, 0)]
        
        def is_valid(grid: List[List[str]], i: int, j: int) -> bool:
            """Helper function: exam if a postion is valid to step upon"""
            return 0 <= i < len(grid) and 0 <= j < len(grid[0]) and grid[i][j] != '#'
        
        def can_reach(start: Tuple, end: Tuple, box: Tuple) -> bool:
            """ Helper function: 
            given the starting person's position and ending desired and the current box
            exam if there is a valid DFS path to reach
            """
            stack = [start]
            visited = set([start])
            while stack:
                p = stack.pop()
                if p == end: # reach the desired position
                    return True
                p_x, p_y = p
                for dx, dy in direct:
                    # try four walking position
                    next_p = (p_x+dx, p_y+dy)
                    if is_valid(grid, *next_p) and next_p not in visited and next_p != box:
                        # remember can not pass through the box position
                        visited.add(next_p)
                        stack.append(next_p)
            return False
        
        m, n = len(grid), len(grid[0])
        # record the initialization info
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 'B':
                    box = (i, j)
                elif grid[i][j] == 'T':
                    target = (i, j)
                elif grid[i][j] == 'S':
                    pos = (i, j)
        visited = set([(pos, box)])
        queue = deque([(0, pos, box)])
        while queue:
            push, pos, box = queue.popleft()
            if box == target:
                # BFS success
                return push 
            for dx, dy in direct:
                next_box = (box[0]+dx, box[1]+dy)
                next_pos = (box[0]-dx, box[1]-dy)
                # 4 criteria to be valid to be searched
                if is_valid(grid, *next_box) and is_valid(grid, *next_pos) and \
                    (next_pos, next_box) not in visited and can_reach(pos, next_pos, box):
                    visited.add((next_pos, next_box))
                    queue.append((push+1, next_pos, next_box))
        return -1
```
