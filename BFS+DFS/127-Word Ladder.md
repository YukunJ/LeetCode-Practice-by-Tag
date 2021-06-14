**127. Word Ladder**

```Tag: BFS```

**Description:**

A **transformation sequence** from word ```beginWord``` to word ```endWord``` using a dictionary ```wordList``` is a sequence of words ```beginWord -> s1 -> s2 -> ... -> sk``` such that:

+ Every adjacent pair of words differs by a single letter.
+ Every ```si``` for ```1 <= i <= k``` is in ```wordList```. Note that ```beginWord``` does not need to be in ```wordList```.
+ ```sk == endWord```

Given two words, ```beginWord``` and ```endWord```, and a dictionary ```wordList```, return the **number of words** in the **shortest transformation sequence** from ```beginWord``` to ```endWord```, or ```0``` if no such sequence exists.

**Example1**:

        Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
        Output: 5
        Explanation: One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> cog", which is 5 words long.


**Example2**:

        Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
        Output: 0
        Explanation: The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.

-----------

```python
class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        """
        This is a BFS problem, we need to construct graph (undirected) to 
            find the shortest distance between beginWord and endWord
        We first enumerate all possible '1-character' transform 
            and make an edge for it if present in the wordList
        Then we start BFS search
        
        denote N := len(wordList), C := len(beginWord)
        Time Complexity : O(N*C^2) notice re-construction of a word transformed takes O(C)
        Space Complexity : O(N*C^2) the graph contains O(N) entries and each entries points to at most a set of size O(26*C), in which each word takes O(C)
        """
        
        if endWord not in wordList: # boundary checking
            return 0
        wordList.append(beginWord)
        graph = {word : set() for word in wordList}
        ord_a = ord('a')
        for word in wordList:
            n = len(word)
            for pos in range(n):
                for j in range(26): # enumerate 26 characters in alphabet
                    trans = word[:pos] + chr(ord_a + j) + word[pos+1:]
                    if trans in graph and trans != word: # undirected edge, both sides added
                        # notice here, check appearance in hashtable "graph", 
                        # don't go back to check in wordList
                        # otherwise, timelimit exceeds
                        graph[word].add(trans) 
                        graph[trans].add(word) 

        visited = set()
        queue = [(beginWord, 1)]
        while queue:
            newQueue = []
            for word, depth in queue:
                if word == endWord:
                    return depth
                visited.add(word)
                for adjacent in graph[word]:
                    if adjacent not in visited:
                        newQueue.append((adjacent, depth+1))
            queue = newQueue
            
        return 0
```
