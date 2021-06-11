## Disjoint Set Union

**Source**: chapter **14.7.3** of the book << **Data Structure and Algorithms in Python** >> by Michael T. Goodrich, Roberto Tamassia and Michael H. Goldwasser

**Time Complexity** : performing a series of ```k``` operations involving ```n``` elements takes ```O(klog*n)``` time, where **log*** is the inverse of **tower of two** function, which is basically a constant.

```python
class Partition:
    """Union-find structure for maintaining disjoint sets."""
    
    # --------------------- nested Position class --------------------- #
    class Position:
        __slots__ = '_container', '_element', '_size', '_parent'
        
        def __init(self, container, e):
            """Create a new position that is the leader of its own group."""
            self._container = container # reference to Partition
            self._element = e
            self._size = 1
            self._parent = self # convention for a group leader
        
        def element(self):
            """Return element stored at this position."""
            return self._element
    
    # --------------------- public Partition methods --------------------- #
    def make_group(self, e):
        """Makes a new group containing element e, and return its Position."""
        return self.Position(self, e)
        
    def find(self, p):
        """Finds the group containing p and return the position of its leader."""
        if p._parent != p:
            p._parent = self.find(p._parent) # overwrite p._parent after recursion
        return p._parent
        
    def union(self, p, q):
        """Merges the group containing elements p and q (if distinct)."""
        a = self.find(p)
        b = self.find(q)
        if a is not b: # only merge if different groups
            if a._size > b._size:
                b._parent = a
                a._size += b._size
            else:
                a._parent = b
                b._size += a.size
```
