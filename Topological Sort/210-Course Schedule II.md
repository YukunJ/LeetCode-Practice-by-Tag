**210. Course Schedule II**

```Tag: topological sort```

**Description:**

There are a total of ```numCourses``` courses you have to take, labeled from ```0``` to ```numCourses - 1```. You are given an array ```prerequisites``` where ```prerequisites[i] = [ai, bi]``` indicates that you must take course ```bi``` first if you want to take course ```ai```.

+ For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.

Return the ordering of courses you should take to finish all courses. If there are many valid answers, return **any** of them. If it is impossible to finish all courses, return **an empty array**.

**Example1**:

        Input: numCourses = 2, prerequisites = [[1,0]]
        Output: [0,1]
        Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].

**Example2**:

        Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
        Output: [0,2,1,3]
        Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
        So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].
    
-----------

```python
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        """
        This is the typical topological sort problem, exactly the same as <207. Course Schedule>
        to find if the course graph is acyclic, it's equivalent to find
        if there is a proper topological ordering of the nodes
        
        denote V := numCourses, E := len(prerequities)
        Time Complexity :  O(V+E), every node get visited and popped once, 
                           every edges visited twice (construction and topo sort)
        Space Complexity : O(V+E) for topo list O(V), 
                           out_edges O(E) and in_count storage O(E)
        """

        # graph construction
        from collections import defaultdict, deque
        in_count = [0] * numCourses # the incoming degree count
        out_edges = defaultdict(list) # the outgoing edge
        topo, ready = [], deque()
        for pair in prerequisites:
            out_edges[pair[1]].append(pair[0]) # record an edge pair[0] -> pair[1]
            in_count[pair[0]] += 1 # increase the incoming degree for pair[0]
            
        for i in range(numCourses):
            if in_count[i] == 0: # free of constraint at very beginning
                ready.append(i)
        
        # topo sort
        while ready:
            node = ready.popleft() 
            topo.append(node)
            for out_node in out_edges[node]: # decrease incoming degree by 1 
                in_count[out_node] -= 1
                if in_count[out_node] == 0: # become ready
                    ready.append(out_node)
        
        return topo if len(topo) == numCourses else []   
```
