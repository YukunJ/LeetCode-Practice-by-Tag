**269. Alien Dictionary**

```Tag : topological sorting```

**Description:**

There is a new alien language that uses the English alphabet. However, the order among the letters is unknown to you.

You are given a list of strings ```words``` from the alien language's dictionary, where the strings in ```words``` are **sorted lexicographically** by the rules of this new language.

Return a string of the unique letters in the new alien language sorted in **lexicographically increasing order** by the new language's rules. If there is no solution, return ```""```. If there are multiple solutions, return **any of them**.

A string ```s``` is **lexicographically smaller** than a string ```t``` if at the first letter where they differ, the letter in ```s``` comes before the letter in ```t``` in the alien language. If the first ```min(s.length, t.length)``` letters are the same, then s is smaller if and only if ```s.length < t.length```.


**Example1:**

		Input: words = ["wrt","wrf","er","ett","rftt"]
		Output: "wertf"

**Example2:**

		Input: words = ["z","x"]
		Output: "zx"

**Example3:**

		Input: words = ["z","x","z"]
		Output: ""
		Explanation: The order is invalid, so return "".

-----------

```python
class Solution:
    def alienOrder(self, words: List[str]) -> str:
        """
        This is a tricky topological sort question
        the pre-processing is really easy to mess up
        we create dependency edge between characters
        
        denote S := sum of characters in all words
               U := size of alphabet, here it's 26 lower case letters
               N := len(words)
        Time Complexity : O(S)
        Space Complexity : O(U+min(U^2, N))
        """
        from collections import defaultdict, deque
        offset = ord('a')
        node_set = set() # all the appearing node
        indegree_count = [0] * 26
        outedge = defaultdict(set)
        for word in words:
            for char in word:
                node_set.add(char)
        for i in range(len(words)-1):
            signal = True
            for j in range(min(len(words[i]), len(words[i+1]))):
                if words[i][j] == words[i+1][j]:
                    continue
                else:
                    # a new dependency edge
                    if words[i+1][j] not in outedge[words[i][j]]:
                        indegree_count[ord(words[i+1][j])-offset] += 1
                        outedge[words[i][j]].add(words[i+1][j])
                    signal=False
                    break
            if signal and len(words[i+1]) < len(words[i]):
                # early stopping
                # if second one is a prefix substring of first one
                return ""
             
        topo = []
        ready = deque()
        visited = set()
        # search for initially ready node
        for node in node_set:
            if indegree_count[ord(node)-offset] == 0:
                ready.append(node)
        while ready:
            ready_node = ready.popleft()
            topo.append(ready_node)
            visited.add(ready_node)
            # decrease in degree for connected node
            for neighbor in outedge[ready_node]:
                indegree_count[ord(neighbor)-offset] -= 1
                if indegree_count[ord(neighbor)-offset] == 0:
                    # this neighbor node is ready
                    ready.append(neighbor) 
                    
        if len(topo) == len(node_set):
            # a successful topological sorting
            return "".join(topo)
        # a cycle dependency found, failure
        return ""
```