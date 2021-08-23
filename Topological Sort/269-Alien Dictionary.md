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
        from collections import defaultdict, deque, Counter
        # first create in degree counting, adjacency list and node recording
        adj_lst = defaultdict(set)
        in_degree = Counter({c : 0 for word in words for c in word})
        
        # record all dependency edges and in degree counting
        for first_word, second_word in zip(words, words[1:]):
            early_stopping = True
            for c1, c2 in zip(first_word, second_word):
                if c1 != c2:
                    early_stopping = False
                    if c2 not in adj_lst[c1]:
                        adj_lst[c1].add(c2)
                        in_degree[c2] += 1
                    break
            if early_stopping and len(second_word) < len(first_word):
                # check if second word is a prefix of first one
                return ""
        
        # topological sorting
        topo = []
        queue = deque([c for c in in_degree if in_degree[c] == 0])
        while queue:
            c = queue.popleft()
            topo.append(c)
            for neighbor in adj_lst[c]:
                in_degree[neighbor] -= 1
                if in_degree[neighbor] == 0:
                    queue.append(neighbor)
        
        if len(topo) < len(in_degree):
            return ""
        return "".join(topo)
```
