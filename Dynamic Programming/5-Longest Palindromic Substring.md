**5. Longest Palindromic Substring**

```Tag: dynamic programming```

**Description:**

Given a string ```s```, return the longest palindromic substring in ```s```.

**Example1**:

        Input: s = "babad"
        Output: "bab"
        Note: "aba" is also a valid answer.
        
**Example2**:

        Input: s = "cbbd"
        Output: "bb"
        
**Example3**:

        Input: s = "a"
        Output: "a"

**Example4**:

        Input: s = "ac"
        Output: "a"

-----------

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        """
        We use dynamic programming technique to compute Palindrome
        Consturct a matrix dp, s.t. dp[i][j] indicates if s[i:j] is a palindrome
        in updating dp[i][j], dp[i][j] = (s[i] == s[j-1]) and (dp[i-1][j-1])
        
        Time Complexity : O(n^2) 
                    the matrix is of size O(n^2) and each entry checking step is O(1)
        Space Complexity : O(n^2) for matrix storage
        """
        ans, ans_len, n = "", 0, len(s)
        dp = [[False] * (n+1) for _ in range(n+1)] # init the dynamic matrix
        for j in range(1, n+1):
            for i in range(j):
                if s[i] != s[j-1]:
                    dp[i][j] = False
                else: # s[i] == s[j-1]
                    if j - i <= 2:
                        # either single character like "a"
                        # or double same character like "bb"
                        dp[i][j] = True
                    else:
                        dp[i][j] = dp[i+1][j-1]
                if dp[i][j]: # try to update the max length
                    if j - i >= ans_len:
                        ans_len = j - i
                        ans = s[i:j]
        return ans                        
```
