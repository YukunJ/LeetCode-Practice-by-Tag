**839. Similar String Groups**

```Tag : disjoint set union/dfs/string```

**Description:**

Two strings ```X``` and ```Y``` are similar if we can swap two letters (in different positions) of ```X```, so that it equals ```Y```. Also two strings ```X``` and ```Y``` are similar if they are equal.

For example, ```"tars"``` and ```"rats"``` are similar (swapping at positions ```0``` and ```2```), and ```"rats"``` and ```"arts"``` are similar, but ```"star"``` is not similar to ```"tars"```, ```"rats"```, or ```"arts"```.

Together, these form two connected groups by similarity: ```{"tars", "rats", "arts"}``` and ```{"star"}```.  Notice that ```"tars"``` and ```"arts"``` are in the same group even though they are not similar.  Formally, each group is such that a word is in the group if and only if it is similar to at least one other word in the group.

We are given a list ```strs``` of strings where every string in ```strs``` is an anagram of every other string in ```strs```. How many groups are there?

**Example1:**

        Input: strs = ["tars","rats","arts","star"]
        Output: 2

**Example2:**

        Input: strs = ["omv","ovm"]
        Output: 1
    
-----------

**Solution1: Disjoint Set Union**

```python
class DSU:
    """Helper class: Disjoint Set Union"""
    def __init__(self, strs: List[str]) -> None:
        self.n = len(strs)
        self.parent = [i for i in range(self.n)]
        self.rank = [1 for i in range(self.n)]
    
    def find(self, i: int) -> int:
        if self.parent[i] != i:
            self.parent[i] = self.find(self.parent[i])
        return self.parent[i]
    
    def union(self, p: int, q: int) -> None:
        if p != q:
            leaderp, leaderq = self.find(p), self.find(q)
            if leaderp != leaderq:
                # to avoid unnecessary operation
                # make sure p and q are not already in the same group
                if self.rank[leaderp] > self.rank[leaderq]:
                    # make sure leaderp is the smaller group to be absorbed
                    leaderp, leaderq = leaderq, leaderp
                self.parent[leaderp] = leaderq
                self.rank[leaderq] += self.rank[leaderp]
    
    def getGroup(self) -> int:
        return sum([1 for idx, parent in enumerate(self.parent) if idx == parent])
            

class Solution:
    def numSimilarGroups(self, strs: List[str]) -> int:
        """
        We could either adopt a Disjoint Set Union or DFS approach in solving this problem
        The way to check if two strings are "similar" is that they are of same length and
        only exactly two positions are different (or completely same)
        
        denote n := len(strs) and s := length of string in strs
        Time Complexity : O(n^2*s) each check_similar takes O(s) and there are O(n^2) pairs we need to try union
        Space Complexity : O(n) for the DSU data structure
        """
        def check_similar(string1: str, string2: str) -> bool:
            """Helper function: check if two stirngs are similar"""
            if len(string1) != len(string2):
                return False
            ptr = 0
            diff_count = 0
            while ptr < len(string1):
                if string1[ptr] != string2[ptr]:
                    diff_count += 1
                    if diff_count > 2: # early stopping
                        return False
                ptr += 1
            # either exact same or could be resolved by a single swap
            return True
        
        n = len(strs)
        my_DSU = DSU(strs)
        for i in range(n):
            for j in range(i+1, n):
                if my_DSU.parent[i] == my_DSU.parent[j]:
                    continue
                if check_similar(strs[i], strs[j]):
                    my_DSU.union(i, j)
        return my_DSU.getGroup()
```

-----------

**Solution2: DFS**

```python
class Solution:
    def numSimilarGroups(self, strs: List[str]) -> int:
        """
        We could either adopt a Disjoint Set Union or DFS approach in solving this problem
        The way to check if two strings are "similar" is that they are of same length and
        only exactly two positions are different (or completely same)
        
        denote n := len(strs) and s := length of string in strs
        Time Complexity : O(n^2*s) each check_similar takes O(s) and there are O(n^2) pairs to be checked in graph construction
        Space Complexity : O(n) for the visited set storage and at most dfs recursion stack height
        """
        from collections import defaultdict
        def check_similar(string1: str, string2: str) -> bool:
            """Helper function: check if two stirngs are similar"""
            if len(string1) != len(string2):
                return False
            ptr = 0
            diff_count = 0
            while ptr < len(string1):
                if string1[ptr] != string2[ptr]:
                    diff_count += 1
                    if diff_count > 2: # early stopping
                        return False
                ptr += 1
            # either exact same or could be resolved by a single swap
            return True
    
        def dfs(i: int):
            """Helper function: do a depth-first-search on connected graph"""
            nonlocal graph, visited
            visited.add(i)
            for neighbor in graph[i]:
                if neighbor not in visited:
                    dfs(neighbor)
                    
        n = len(strs)    
        
        # connected graph edge construction
        graph = defaultdict(set)
        for i in range(n):
            for j in range(i+1, n):
                if check_similar(strs[i], strs[j]):
                    graph[i].add(j)
                    graph[j].add(i)
        visited = set()
        group = 0
        for i in range(n):
            if i not in visited:
                # a new connected component encountered
                dfs(i)
                group += 1
                    
        return group
```
