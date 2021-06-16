**834. Sum of Distances in Tree**

```Tag: Tree/DFS```

**Description:**

An undirected, connected tree with ```n``` nodes labelled ```0...n-1``` and ```n-1``` edges are given.

The ```i```th edge connects nodes ```edges[i][0]``` and ```edges[i][1]``` together.

Return a list ```ans```, where ```ans[i]``` is the sum of the distances between node ```i``` and all other nodes.

**Example1**:

        Input: n = 6, edges = [[0,1],[0,2],[2,3],[2,4],[2,5]]
        Output: [8,12,6,10,10,10]
        Explanation: 
        Here is a diagram of the given tree:
           0
          /  \
         1    2
             /|\
            3 4 5
        We can see that dist(0,1) + dist(0,2) + dist(0,3) + dist(0,4) + dist(0,5)
        equals 1 + 1 + 2 + 2 + 2 = 8.  Hence, answer[0] = 8, and so on.

-----------

```python
class Solution:
    def sumOfDistancesInTree(self, N: int, edges: List[List[int]]) -> List[int]:
        """
        This is a dynamic programming problem on tree
        We will first compute out each node's depth and 
            how many nodes a subtree contains, with itself serving as a root
        The sum of Distance for the root is just the sum of depth of all nodes, starting with there,
        we can derive answers root's children
        going from root to its children, we 
        1) gain the edge from children to root X times
        2) lose the edge from root to children Y times
        where Y = the number of nodes contained in the subtree with children as root
              and X = the rest nodes not contained in the subtree with children as root = N - X
        therefore, the state transition equation is:
        dp[children] = dp[root] + N - 2 * count[children]

        Time Complexity : O(N)
        Space Complexity : O(N)
        """
        ans, depth, count = [0] * N, [0] * N, [0] * N
        graph = [list() for _ in range(N)]
        for parent, child in edges:
            graph[parent].append(child)
            graph[child].append(parent)

        def dfs_for_depth_count(child: int, parent: int) -> None:
            count[child] = 1
            for grandchild in graph[child]:
                if grandchild != parent: 
                    # caution, both parent and grandchild present in graph[child]
                    depth[grandchild] = depth[child] + 1
                    dfs_for_depth_count(grandchild, child)
                    count[child] += count[grandchild]
        
        def dfs_for_answer(child: int, parent: int) -> None:
            for grandchild in graph[child]:
                if grandchild != parent:
                    ans[grandchild] = ans[child] + N - 2 * count[grandchild]
                    dfs_for_answer(grandchild, child)
        
        dfs_for_depth_count(0, -1)
        ans[0] = sum(depth)
        dfs_for_answer(0, -1)
        return ans 
```
