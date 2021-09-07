**886. Possible Bipartition**

```Tag : bfs/dfs/graph```

**Description:**

We want to split a group of ```n``` people (labeled from ```1``` to ```n```) into two groups of **any size**. Each person may dislike some other people, and they should not go into the same group.

Given the integer n and the array dislikes where ```dislikes[i] = [a_i, b_i]``` indicates that the person labeled ```a_i``` does not like the person labeled ````b_i```, return ```true``` if it is possible to split everyone into two groups in this way.

**Example1:**

        Input: n = 4, dislikes = [[1,2],[1,3],[2,4]]
        Output: true
        Explanation: group1 [1,4] and group2 [2,3].

**Example2:**

        Input: n = 3, dislikes = [[1,2],[1,3],[2,3]]
        Output: false
        
**Example3:**

        Input: n = 5, dislikes = [[1,2],[2,3],[3,4],[4,5],[1,5]]
        Output: false

-----------

```python
class Solution:
    def possibleBipartition(self, n: int, dislikes: List[List[int]]) -> bool:
        """
        We will model this problem in a "painting" setting
        Each person is a node in the graph
        for each dislike [a_i, b_i] we model it as an edge of a graph
        and there are 2 colors "Red" and "Blue"
        we require that adjanct nodes could not be painted the same color
        Then this problem becomes a DFS/BFS problem
        
        denote m := len(dislikes)
        Time Complexity : O(n+m)
        Space Complexity : O(n+m)
        """
        from collections import defaultdict, deque
        
        # construct the graph the bi-directional edges
        graph = {}
        edge = defaultdict(list)
        for hater, hated in dislikes:
            edge[hater].append(hated)
            edge[hated].append(hater)
        
        def bfs(starter: int, edge: dict) -> bool:
            """Helper function: run a BFS try to paint bipartition from the starter"""
            nonlocal graph
            graph[starter] = "Red"
            queue = deque([starter])
            while queue:
                node = queue.popleft()
                color = graph[node]
                for neighbor in edge[node]:
                    if neighbor not in graph:
                        graph[neighbor] = "Blue" if color == "Red" else "Red"
                        queue.append(neighbor)
                    else:
                        if color == graph[neighbor]:
                            return False
            return True
            
        # the graph may not be connected, need to run on every node to enumerate
        for starter in range(1, n+1):
            if starter not in graph:
                if not bfs(starter, edge):
                    return False
        return True
```

