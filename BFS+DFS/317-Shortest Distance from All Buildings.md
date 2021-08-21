**317. Shortest Distance from All Buildings**

```Tag : BFS/matrix/array```

**Description:**

You are given an ```m x n``` grid ```grid``` of values ```0```, ```1```, or ```2```, where:

+ each ```0``` marks **an empty land** that you can pass by freely

+ each ```1``` marks **a building** that you cannot pass through, and

+ each ```2``` marks **an obstacle** that you cannot pass through.

You want to build a house on an empty land that reaches all buildings in the **shortest total travel** distance. You can only move up, down, left, and right.

Return the **shortest travel distance** for such a house. If it is not possible to build such a house according to the above rules, return ```-1```.

The **total travel distance** is the sum of the distances between the houses of the friends and the meeting point.

The distance is calculated using *Manhattan Distance*, where ```distance(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|```.

**Example1:**

![avatar](Fig/317-E1.jpg)

		Input: grid = [[1,0,2,0,1],[0,0,0,0,0],[0,0,1,0,0]]
		Output: 7
		Explanation: Given three buildings at (0,0), (0,4), (2,2), and an obstacle at (0,2).
		The point (1,2) is an ideal empty land to build a house, as the total travel distance of 3+3+1=7 is minimal.
		So return 7.


**Example2:**

		Input: grid = [[1,0]]
		Output: 1

**Example3:**

		Input: grid = [[1]]
		Output: -1

-----------

```python
class Solution:
    def shortestDistance(self, grid: List[List[int]]) -> int:
        """
        We think backward: start with a house and travel all the empty lands
        for each empty land, we need to count how many houses can visit this land
        we care about the minimum sum distance only if all houses do reach here
        
        denote m, n := grid.shape
        Time Complexity : O(m^2 * n^2)
        Space Complexity : O(m * n)
        """
        m, n = len(grid), len(grid[0])
        records_visited = [[0] * n for _ in range(m)] # the visiting record count for empty lands
        records_dist = [[0] * n for _ in range(m)] # the total visiting distance from all buildings
        houses = [] # the list of houses to be processed
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    houses.append((i, j))
        house_num = len(houses)
        
        # start BFS from every house to every reachable land
        while houses:
            house = houses.pop()
            queue = [(house, 0)]
            visited = set()
            visited.add(house)
            while queue: # level by level BFS
                newQueue = []
                for node, dist in queue:
                    node_x, node_y = node
                    if grid[node_x][node_y] == 0: # an empty land
                        records_visited[node_x][node_y] += 1
                        records_dist[node_x][node_y] += dist
                    # try further visited other node
                    for x, y in [(node_x+1, node_y), (node_x-1, node_y), (node_x, node_y+1), (node_x, node_y-1)]:
                        if (0 <= x < m) and (0 <= y < n) and ((x, y) not in visited) and (grid[x][y] == 0):
                            # within boundary + not visited yet + is an empty land
                            visited.add((x, y))
                            newQueue.append(((x, y), dist+1))
                queue = newQueue
                
        min_dist = float('inf')
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 0 and records_visited[i][j] == house_num:
                    # must be reachable from all houses
                    min_dist = min(min_dist, records_dist[i][j])
        return min_dist if min_dist != float('inf') else -1
```