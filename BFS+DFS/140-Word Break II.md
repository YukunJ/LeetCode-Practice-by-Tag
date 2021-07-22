**140. Word Break II**

```Tag : DFS/Backtracking```

**Description:**

Given a string ```s``` and a dictionary of strings ```wordDict```, add spaces in ```s``` to construct a sentence where each word is a valid dictionary word. Return all such possible sentences in **any order**.

Note that the same word in the dictionary may be reused multiple times in the segmentation.


**Example1**:


		Input: s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]
		Output: ["cats and dog","cat sand dog"]

 
**Example2**:
 
		Input: s = "pineapplepenapple", wordDict = ["apple","pen","applepen","pine","pineapple"]
		Output: ["pine apple pen apple","pineapple pen apple","pine applepen apple"]
		Explanation: Note that you are allowed to reuse a dictionary word.

**Example3**:

		Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
		Output: []
      
-----------

```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> List[str]:
        """
        Being different from Q139 <Word Break> that 
        here we are not only interested in if s could be broken into words in wordDict
        but we want to enumerate all such possible breaking methods
        This becomes a DFS backtracking problem

        denote n := len(s)
        the complexity here is hard to analyze, consider the worst case that
        any cut resides in the wordDict, then for every position of s, we can
        split or not split, resulting in O(2^n) possible way to cut the string s
        each answer need O(n) space to store, although we may ignore the space used for final ans
        but by memorization, we still need O(n*2^n) space to store them
        and the creation of such storage requires the same degree of time complexity for writing and manipulating strings
        Time Complexity : O(n*2^n)
        Space Complexity : O(n*2^n)
        """
        def dfs(index: int):
            if index == len(s):
                return [[]]
            elif index in search_history: # already searched
                return search_history[index]
            else:
                ans = []
                for i in range(index+1, len(s)+1):
                    word = s[index:i]
                    if word in wordSet:
                        nextWordBreaks = dfs(i) # dfs based on next index
                        for nextWordBreak in nextWordBreaks:
                            ans.append([word]+nextWordBreak) # concat current word found
                search_history[index] = ans # store the result in memory
                return ans 

        wordSet = set(wordDict) # hash the possible candidate
        search_history = dict()
        ans = dfs(0)
        return [" ".join(wordbreak) for wordbreak in ans]
```
