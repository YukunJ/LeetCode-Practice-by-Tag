**139. Word Break**

```Tag: dynamic programming```

**Description:**

Given a string ```s``` and a dictionary of strings ```wordDict```, return ```true``` if ```s``` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

**Example1**:

        Input: s = "leetcode", wordDict = ["leet","code"]
        Output: true
        Explanation: Return true because "leetcode" can be segmented as "leet code".

**Example2**:

        Input: s = "applepenapple", wordDict = ["apple","pen"]
        Output: true
        Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
        Note that you are allowed to reuse a dictionary word.
        
**Example3**:

        Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
        Output: false

-----------

```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        """
        A dynamic programing scheme
        let dp[i] denote if s[:i] could bd separated by the given wordDict
        we could grow the dp vector as follows:
        given dp[i] is true if s[i:j] is in wordDict, then dp[j] is also True
        
        denote n := len(s)
        Time Complexity : O(n^2) for nested loop
        Space Complexity : O(n) by dp vector
        """
        # init the dp vector
        # the empty string is assumed to be separable
        n = len(s)
        dp = [True] + [False] * n
        
        for i in range(n):
            for j in range(i+1, n+1):
                dp[j] |= (dp[i] and (s[i:j] in wordDict))
                
        return dp[-1]
```
