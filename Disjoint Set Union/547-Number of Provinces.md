**547. Number of Provinces**

```Tag : disjoint set union/graph/dfs/bfs```

**Description:**

There are ```n``` cities. Some of them are connected, while some are not. If city ```a``` is connected directly with city ```b```, and city ```b``` is connected directly with city ```c```, then city ```a``` is connected indirectly with city ```c```.

A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an ```n x n``` matrix ```isConnected``` where ```isConnected[i][j] = 1``` if the ```i-th``` city and the ```j-th``` city are directly connected, and ```isConnected[i][j] = 0``` otherwise.

Return the total number of **provinces**.

**Example1:**

![avatar](Fig/547-E1.jpeg)

        Input: isConnected = [[1,1,0],[1,1,0],[0,0,1]]
        Output: 2

**Example2:**

![avatar](Fig/547-E2.jpeg)

        Input: isConnected = [[1,0,0],[0,1,0],[0,0,1]]
        Output: 3

-----------

**Solution1: Disjoint Set Union**

```python
class DSU:
    """
    Helper class: Disjoint Set Union
    """
    def __init__(self, grid: List[List[int]]):
        self.n = len(grid)
        self.parent = [i for i in range(self.n)]
        self.rank = [0 for i in range(self.n)]
        self.count = self.n
    
    def find(self, i: int) -> int:
        # Path Compression
        if i != self.parent[i]:
            self.parent[i] = self.find(self.parent[i])
        return self.parent[i]
    
    def union(self, p: int, q: int) -> None:
        leaderp, leaderq = self.find(p), self.find(q)
        if leaderp != leaderq:
            if self.rank[leaderp] > self.rank[leaderq]:
                # make sure the leaderq is smaller cluster
                leaderp, leaderq = leaderq, leaderp
            self.parent[leaderp] = leaderq
            self.rank[leaderq] += self.rank[leaderp]
            self.count -= 1
    
    def getCount(self):
        return self.count

class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        """
        We adopt a Disjoint Set Union approach to solve this problem
        we cluster any two connected cities into a same cluster
        and at the end count how many clusters are left out there
        
        denote n := len(isConnected)
        notice each operation of disjoint set union is close to O(1) constant operation
        Time Complexity : O(n^2) iterate through the grid
        Space Complexity : O(n) for DSU storage
        """
        from collections import defaultdict
        graph = defaultdict(set)
        myDSU = DSU(isConnected)
        n = len(isConnected)
        for i in range(n):
            for j in range(n):
                if i != j and isConnected[i][j] == 1:
                    myDSU.union(i, j)
        return myDSU.getCount()
```

-----------

**Solution2: DFS**

```python
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        """
        We could simply adopt a DFS search to get a maximal connected component count
        
        denote n := len(isConnected)
        Time Complexity : O(n^2)
        Space Complexity : O(n)
        """
        from collections import defaultdict
        
        n = len(isConnected)
        graph = defaultdict(set)
        province = 0
        for i in range(n):
            for j in range(n):
                if i != j and isConnected[i][j] == 1:
                    # not necessarily bi-directional graph
                    # need take care both sides
                    graph[i].add(j)
                    graph[j].add(i)
        
        visited = set()
        
        def dfs(i: int):
            visited.add(i)
            for neighbor in graph[i]:
                if neighbor not in visited:
                    dfs(neighbor)
        
        # dfs-visiting the graph and count maximal connected components
        for i in range(n):
            if i not in visited:
                province += 1
                dfs(i)
        return province
```
