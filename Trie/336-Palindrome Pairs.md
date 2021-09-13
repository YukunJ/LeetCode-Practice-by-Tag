**336. Palindrome Pairs**

```Tag : hashtable/string/trie```

**Description:**

Given a list of **unique** words, return all the pairs of the **distinct** indices ```(i, j)``` in the given list, so that the concatenation of the two words ```words[i] + words[j]``` is a palindrome.


**Example1:**

        Input: words = ["abcd","dcba","lls","s","sssll"]
        Output: [[0,1],[1,0],[3,2],[2,4]]
        Explanation: The palindromes are ["dcbaabcd","abcddcba","slls","llssssll"]
        
**Example2:**

        Input: words = ["bat","tab","cat"]
        Output: [[0,1],[1,0]]
        Explanation: The palindromes are ["battab","tabbat"]
        
**Example3:**

        Input: words = ["a",""]
        Output: [[0,1],[1,0]]

-----------

**Solution1: Hashtable**

```python
class Solution:
    def palindromePairs(self, words: List[str]) -> List[List[int]]:
        """
        One perspective to deal with this question is to think:
        How can two words be concatenated to form a Palindrome
        let's call wordA and wordB
        1) if len(wordA) == len(wordB), essentially we need wordA == wordB[::-1]
        2) if len(wordA) > len(wordB), then we neeed
            wordA[:len(wordB)] == wordB[::-1] and wordA[len(wordB):] is Palindrome itself
        3) Vice versa, if len(wordA) < len(wordB), then we need
            wordA == wordB[-len(wordA):][::-1] and wordB[:-len(wordA)] is Palindrome
        
        denote n := len(words), k := the longest word length in the array
        Time Complexity : O(n * k^2) 
                        the word_map takes O(n * k), 
                        for each word its complete reverse takes O(k), 
                        and for the generation of prefix/suffix function call, it takes O(k^2).
        Space Complexity: O(k^2 + n * k)
                        hashtable size is O(n * k)
                        for each word, in worst case the there are k prefix/suffix words to be returned of size O(k)
                        we discard the size of output return though
        """             
        
        def safe_prefix(word: str) -> List[str]:
            # Helper function: return a list of "safe prefix" subword that the rest is Palindrome by itself
            safe = []
            for i in range(len(word)):
                if word[i:] == word[i:][::-1]:
                    safe.append(word[:i])
            return safe
        
        def safe_suffix(word: str) -> List[str]:
            # Helper function: return a list of "safe suffix" subword that the rest is Palindrome by itself
            safe = []
            for i in range(len(word)):
                if word[:i+1] == word[:i+1][::-1]:
                    safe.append(word[i+1:])
            return safe       
        
        # store word <-> index mapping to avoid self-duplicate
        word_map = {word : idx for idx, word in enumerate(words)}
        
        res = []
        for idx, word in enumerate(words):
            reversed_word = word[::-1]
            
            # case 1
            if reversed_word in word_map and idx != word_map[reversed_word]:
                res.append([idx, word_map[reversed_word]])
            
            # case 2
            for prefix in safe_prefix(word):
                reversed_prefix = prefix[::-1]
                if reversed_prefix in word_map:
                    res.append([idx, word_map[reversed_prefix]])
            
            # case 3
            for suffix in safe_suffix(word):
                reversed_suffix = suffix[::-1]
                if reversed_suffix in word_map:
                    res.append([word_map[reversed_suffix], idx])
        
        return res
```

-----------

**Solution2: Trie Tree**

```python
from collections import defaultdict

class TrieNode:
    """Helper class: A Trie tree node data structure"""
    def __init__(self):
        self.next = defaultdict(lambda: TrieNode())
        self.ending_word = -1
        self.suffix = []

class Solution:
    def palindromePairs(self, words: List[str]) -> List[List[int]]:
        """
        We could also use a Trie data structure to simuate the process of Palindrome
        
        denote n := len(words), k := the longest word length in the array
        Time Complexity : O(n * k^2) 
        Space Complexity: O(k^2 + n * k)
        """             
        # create Trie Tree and add all word's reverse into it
        root = TrieNode()
        for idx, word in enumerate(words):
            word = word[::-1] # reverse the word
            curr = root
            for j, c in enumerate(word):
                # check if the remaining is a Palindrome
                if word[j:] == word[j:][::-1]:
                    curr.suffix.append(idx)
                curr = curr.next[c]
            curr.ending_word = idx
        
        res = []
        for idx, word in enumerate(words):
            curr = root
            for j, c in enumerate(word):
                # case 3
                if curr.ending_word != -1:
                    if word[j:] == word[j:][::-1]:
                        res.append([idx, curr.ending_word])
                if c not in curr.next:
                    break
                curr = curr.next[c]
            else:
                # enter here if no "break" is activated
                # case 1
                if curr.ending_word != -1 and curr.ending_word != idx:
                    res.append([idx, curr.ending_word])
                # case 2
                for j in curr.suffix:
                    res.append([idx, j]) 
        return res
```
