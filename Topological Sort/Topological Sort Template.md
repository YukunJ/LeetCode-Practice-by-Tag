## Topological Sort

**Source**: chapter **14.5.1** of the book << **Data Structure and Algorithms in Python** >> by Michael T. Goodrich, Roberto Tamassia and Michael H. Goldwasser

**Proposition** : A graph G has a topological ordering **if and only if** it is **acyclic**.

**Complexity**: Let G be a directed graph with n vertices and m edges, using an adjacency list representation. The topological sort algorithm runs in **O(n+m)** time using **O(n)** auxiliary space.

```python
def topological_sort(g):
    """Return a list of vertices of directed acyclic graph g in topological order. 
    
    If graph g has a cycle, the result will be incomplete.
    """
    topo = [] # a list of vertices placed in topological order
    ready = [] # list of vertices that have no remaining constraints
    incount = {} # keep track of in-degree for each vertex
    for u in g.vertices():
        incount[u] = g.degree(u, False) # parameter requests incoming degree
        if incount[u] == 0: # if u has no incoming edges
            ready.append(u)
    
    # start topo sort
    while len(ready) > 0:
        u = ready.pop() # u is free of constraints
        topo.append(u) # add u to the topological order
        for e in g.incident_edges(u): # consider all outgoing neighbors of u
            v = e.opposite(u)
            incount[v] -= 1 # v has one less constraint without u
            if incount[v] == 0:
                ready.append(v)
    return topo
```
