**211. Design Add and Search Words Data Structure**

```Tag : Design/Trie/DFS```

**Description:**

Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the ```WordDictionary``` class:

+ ```WordDictionary()``` Initializes the object.

+ ```void addWord(word)``` Adds word to the data structure, it can be matched later.

+ ```bool search(word)``` Returns ```true``` if there is any string in the data structure that matches word or ```false``` otherwise. word may contain dots ```'.'``` where dots can be matched with any letter.

**Hints:**

+ You should be familiar with how a **Trie** works. If not, please work on this problem: <[Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)> first.

**Example1**:

        Input
        ["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
        [[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
        Output
        [null,null,null,null,false,true,true,true]

        Explanation
        WordDictionary wordDictionary = new WordDictionary();
        wordDictionary.addWord("bad");
        wordDictionary.addWord("dad");
        wordDictionary.addWord("mad");
        wordDictionary.search("pad"); // return False
        wordDictionary.search("bad"); // return True
        wordDictionary.search(".ad"); // return True
        wordDictionary.search("b.."); // return True
        
-----------

```python
from collections import defaultdict

class TrieNode:
    """Helper class: a single node in the Trie Tree"""

    def __init__(self, isWordEnd=False):
        self.children = defaultdict(TrieNode)
        self.isWordEnd = isWordEnd

class WordDictionary:
    """
    denote S := number of sum of characters stored in the WordDictionary
           n := length of a given word
    Time Complexity : addWord() O(n)
                      search() O(26^n) in worst case, every word is '.' 
                      and we have to do a BFS on every position, we are using 26 lower-case letters
    Space Complexity : O(S) for static storage if every character occupy a TrieNode
                       O(n) for recursion stack in search() if always encounter '.' character
    """
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = TrieNode()

    def addWord(self, word: str) -> None:
        # use the default dict behavior for easy word adding
        curr = self.root
        for char in word:
            # character by character adding
            curr = curr.children[char]
        # indicate the end of a word
        curr.isWordEnd = True

    def search(self, word: str) -> bool:
        # call helper function
        return self._search(self.root, word)
    
    def _search(self, node: TrieNode, word: str) -> bool:
        # helper function for search
        # starting with a TrieNode to search for a given word
        # when encounter '.', need to DFS search
        curr = node
        for idx, char in enumerate(word):
            if char == '.':
                # skip this position 
                for child in curr.children.keys():
                    if self._search(curr.children[child], word[idx+1:]):
                        # some path gets success
                        return True
                return False
            else: # normal character case
                if char not in curr.children:
                    return False
                curr = curr.children[char]
        return curr.isWordEnd
```
