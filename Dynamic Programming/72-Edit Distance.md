**72. Edit Distance**

```Tag : dynamic programming```

**Description:**

Given two strings ```word1``` and ```word2```, return the *minimum number* of operations required to convert ```word1``` to ```word2```.

You have the following three operations permitted on a word:

+ Insert a character
+ Delete a character
+ Replace a character

**Example1:**

		Input: word1 = "horse", word2 = "ros"
		Output: 3
		Explanation: 
		horse -> rorse (replace 'h' with 'r')
		rorse -> rose (remove 'r')
		rose -> ros (remove 'e')

**Example2:**

		Input: word1 = "intention", word2 = "execution"
		Output: 5
		Explanation: 
		intention -> inention (remove 't')
		inention -> enention (replace 'i' with 'e')
		enention -> exention (replace 'n' with 'x')
		exention -> exection (replace 'n' with 'c')
		exection -> execution (insert 'u')

-----------

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        """
        This question is obviously a dynamic programming problem,
        but the state transition is not easy to find.
        we use two dimenisonal dp[i][j] := the min edit distance between word1[:i] and word2[:j]
        to compute dp[i][j], it relies on 3 previous states dp[i-1][j-1], dp[i-1][j] and dp[i][j-1]
        1) if word1[i-1] == word2[j-1]:    
            dp[i][j] = dp[i-1][j-1] get the extra pair (word[i-1] ~ word2[j-1]) for free
        2) if word[i-1] != word2[j-1]:
            dp[i][j] = 1 + min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1])
        
        denote n := len(word1), m := len(word2)
        Time Complexity : O(n*m)
        Space Complexity : O(n*m)
        """
        n, m = len(word1), len(word2)
        dp = [[0] * (m+1) for _ in range(n+1)]
        
        # initialization
        for j in range(m+1): # pure insertion
            dp[0][j] = j
        for i in range(n+1): # pure deletion
            dp[i][0] = i
        
        # dp process - state transition
        for i in range(1, n+1):
            for j in range(1, m+1):
                if word1[i-1] == word2[j-1]:
                    # get a free pair without any edit
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = 1 + min(dp[i-1][j-1], dp[i][j-1], dp[i-1][j])
                    
        return dp[n][m]
```

