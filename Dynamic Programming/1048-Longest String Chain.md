**1048. Longest String Chain**

```Tag : dynamic programming/hashtable/recursion```

**Description:**

You are given an array of ```words``` where each word consists of lowercase English letters.

```wordA``` is a **predecessor** of ```wordB``` if and only if we can insert **exactly one** letter anywhere in ```wordA``` **without changing the order of the other characters** to make it equal to ```wordB```.

+ For example, ```"abc"``` is a **predecessor** of ```"abac"```, while ```"cba"``` is not a **predecessor** of ```"bcad"```.

A **word chain** is a sequence of words ```[word1, word2, ..., wordk]``` with ```k >= 1```, where ```word1``` is a **predecessor** of ```word2```, ```word2``` is a **predecessor** of ```word3```, and so on. A single word is trivially a word chain with ```k == 1```.

Return the **length** of the **longest possible word chain** with words chosen from the given list of ```words```.

**Example1:**

        Input: words = ["a","b","ba","bca","bda","bdca"]
        Output: 4
        Explanation: One of the longest word chains is ["a","ba","bda","bdca"].

**Example2:**

        Input: words = ["xbc","pcxbcf","xb","cxbc","pcxbc"]
        Output: 5
        Explanation: All the words can be put in a word chain ["xb", "xbc", "cxbc", "pcxbc", "pcxbcf"].

**Example3:**

        Input: words = ["abcd","dbqca"]
        Output: 1
        Explanation: The trivial word chain ["abcd"] is one of the longest word chains.
        ["abcd","dbqca"] is not a valid word chain because the ordering of the letters is changed.

**Hints*:

+ Instead of adding a character, try deleting a character to form a chain in reverse.

+ For each word in order of length, for each word2 which is word with one character removed, length[word2] = max(length[word2], length[word] + 1).

-----------

**Solution1: Up-Bottom Approach**

```python
class Solution:
    def longestStrChain(self, words: List[str]) -> int:
        """
        We observe that if a word A is the predecessor of word B
        then by removing any one character of word B, it should equal word A
        we will adopt a Top-down recursion approach with memorization using DFS
        
        denote n := len(words), L := max length of word in words
        Time Complexity: O(L^2*n)
        Space Complexity : O(n)
        """
        if not words: # boundary case
            return 0
        memory = {}
        words_set = set(words)
        
        def dfs(word):
            """Helper function: starting with word and shrinking in chain"""
            if word in memory: # in memory
                return memory[word]          
            # try deleting one character and further dfs search
            local = 1
            for i in range(len(word)): # O(L)
                deleted = word[:i] + word[i+1:] # slicing O(L)
                if deleted in words_set:
                    recur_chain_len = dfs(deleted)
                    local = max(local, 1 + recur_chain_len)
            memory[word] = local
            return memory[word]
        
        global_max = 0
        for word in words:
            global_max = max(global_max, dfs(word))
        return global_max
```

-----------

**Solution2: Bottom-Up Approach**

```python
class Solution:
    def longestStrChain(self, words: List[str]) -> int:
        """
        Bottom up approach
        denote n := len(words), L := max length of word in words
        Time Complexity: O(L^2*n + n*logn)
        Space Complexity : O(n)
        """
        if not words: # boundary case
            return 0
        memory = {}
        words_set = set(words)
        # sort in ascending order, chain on smaller words first
        words.sort(key=lambda x: len(x)) 
        global_max = 0
        for word in words:
            local_max = 1
            for i in range(len(word)):
                deleted = word[:i] + word[i+1:]
                if deleted in words_set:
                    local_max = max(local_max, 1 + memory.get(deleted, 0))
            memory[word] = local_max
            global_max = max(global_max, local_max)
            
        return global_max
```
