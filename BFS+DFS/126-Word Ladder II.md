**126. Word Ladder II**

```Tag: BFS```

**Description:**

A **transformation sequence** from word ```beginWord``` to word ```endWord``` using a dictionary ```wordList``` is a sequence of words ```beginWord -> s1 -> s2 -> ... -> sk``` such that:

+ Every adjacent pair of words differs by a single letter.
+ Every si for 1 <= i <= k is in wordList. Note that beginWord does not need to be in wordList.
+ sk == endWord

Given two words, ```beginWord``` and ```endWord```, and a dictionary ```wordList```, return all the **shortest transformation sequences** from ```beginWord``` to ```endWord```, or an empty list if no such sequence exists. Each sequence should be returned as a list of the words ```[beginWord, s1, s2, ..., sk]```.

**Example1**:

        Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
        Output: [["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]
        Explanation: There are 2 shortest transformation sequences:
        "hit" -> "hot" -> "dot" -> "dog" -> "cog"
        "hit" -> "hot" -> "lot" -> "log" -> "cog"

**Example2**:

        Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
        Output: []
        Explanation: The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.

-----------

```python
class Solution:
    def findLadders(self, beginWord: str, endWord: str, wordList: List[str]) -> List[List[str]]:
        """
        Exactly like question <127. Word Ladder>
        This is a BFS problem, we need to construct graph (undirected) to 
            find the shortest transformation between beginWord and endWord
        We first enumerate all possible '1-character' transform 
            and make an edge for it if present in the wordList
        Then we start BFS search level by level
        
        denote N := len(wordList), C := len(beginWord)
        Time Complexity : O(N*C^2) notice re-construction of a word transformed takes O(C)
        Space Complexity : O(N*C^2) the graph contains O(N) entries and each entries points to at most a set of size O(26*C), in which each word takes O(C)
        """
        
        if endWord not in wordList: # boundary checking
            return []
        wordList.append(beginWord)
        graph = {word : set() for word in wordList}
        ord_a = ord('a')
        for word in wordList:
            n = len(word)
            for pos in range(n):
                for j in range(26): # enumerate 26 characters in alphabet
                    trans = word[:pos] + chr(ord_a + j) + word[pos+1:]
                    if trans in graph and trans != word: # undirected edge, both sides added
                        graph[word].add(trans) 
                        graph[trans].add(word) 
                        
        answer = []
        success = False # indicator 
        visited = set()
        queue = [(beginWord, [beginWord])]
        while queue:
            newQueue = []
            for word, history in queue:
                if word == endWord:
                    success = True # will end once this level is over
                    answer.append(history)
                    continue
                visited.add(word)
                for adjacent in graph[word]:
                    if adjacent not in visited:
                        newQueue.append((adjacent, history+[adjacent]))
            if success: 
                # already find all the sequence with the shortest distance
                break
            queue = newQueue
            
        return answer
```
