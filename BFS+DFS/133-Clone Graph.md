**133. Clone Graph**

```Tag : dfs/hashtable/graph```

**Description:**

Given a reference of a node in a **connected** undirected graph.

Return a **deep copy** (clone) of the graph.

Each node in the graph contains a value (```int```) and a list (```List[Node]```) of its neighbors.

```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

**Test case format:**

For simplicity, each node's value is the same as the node's index (```1-indexed```). For example, the first node with ```val == 1```, the second node with ```val == 2```, and so on. The graph is represented in the test case using an adjacency list.

**An adjacency list** is a collection of unordered lists used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with ```val = 1```. You must return the **copy of the given node** as a reference to the cloned graph.

**Constraints:**

+ The number of nodes in the graph is in the range ```[0, 100]```.

+ ```1 <= Node.val <= 100```

+ ```Node.val``` is unique for each node.

+ There are no repeated edges and no self-loops in the graph.

+ The Graph is connected and all nodes can be visited starting from the given node.

**Example1**:

![avatar](Fig/133-E1.png)

        Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
        Output: [[2,4],[1,3],[2,4],[1,3]]
        Explanation: There are 4 nodes in the graph.
        1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
        2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
        3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
        4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
        
**Example2**:      

![avatar](Fig/133-E2.png)

        Input: adjList = [[]]
        Output: [[]]
        Explanation: Note that the input contains one empty list. The graph consists of only one node with val = 1 and it does not have any neighbors.
 
 **Example3**:      
 
         Input: adjList = []
         Output: []
         Explanation: This an empty graph, it does not have any nodes.
 
 **Example4**:      
 
 ![avatar](Fig/133-E4.png)
 
         Input: adjList = [[2],[1]]
         Output: [[2],[1]]
 
-----------

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""

class Solution:
    def cloneGraph(self, node: 'Node') -> 'Node':
        self.memory = dict() # init the memory
        return self.cloneNode(node)
    
    def cloneNode(self, node: 'Node') -> 'Node':
        """
        We run a DFS on the given node to deep copy the graph
        one thing in particular is that, 
        sometime the node created in self.memory is 'incomplete'
        we should not wait for it to complete before adding it to the meory,
        otherwise we might get stuck in infinite loop
        consider 1 -> [2], 2 -> [1] for example
        
        denote n := number of node in the graph
        Time Complexity : O(n) clone every node one by one
        Space Complexity : O(n) the hashtable storing reference
        """
        if node:
            if node.val in self.memory:
                # already created, return its reference
                return self.memory[node.val]
            else:
                # add to memory before go deep into recursion
                # otherwise, infinite loop
                newNode = Node(val=node.val)
                self.memory[newNode.val] = newNode
                if node.neighbors:
                    newNode.neighbors = []
                    for neighbor in node.neighbors:
                        # some neighbor might already be created
                        newNode.neighbors.append(self.memory.get(neighbor.val, self.cloneNode(neighbor)))
                return newNode
        return None
```
