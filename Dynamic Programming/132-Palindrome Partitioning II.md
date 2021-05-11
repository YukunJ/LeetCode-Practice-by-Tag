**132. Palindrome Partitioning II**

```Tag: dynamic programming```

**Description:**

Given a string ```s```, partition ```s``` such that every substring of the partition is a palindrome.

Return the minimum cuts needed for a palindrome partitioning of ```s```.

**Example1**:

        Input: s = "aab"
        Output: 1
        Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.

**Example2**:

        Input: s = "a"
        Output: 0 
        
**Example3**:

        Input: s = "ab"
        Output: 1

-----------

```python
class Solution:
    def minCut(self, s: str) -> int:
        """
        We need to use dynamic programming in this question
        denote dp[i] : the min cut needed for s[:i]
        when move to dp[i+1]:
        for every j < i:
        consider 2 cases:
        if s[j:i+1] is palindrome, then dp[i+1] = min(dp[i+1], 1+dp[j])
        if s[j:i+1] not palindrome, then dp[i+1] = min(dp[i+1],i-j+dp[j])
        
        Time Complexity : O(n^2) to do preliminary palindrome check takes O(n^2), also the min-cut dp part takes O(n^2) as well
        Space Complexity : O(n^2) for storage  of isP and O(n) for dp
        """
        
        if not s: # boundary case
            return 0
        
        # first do a preliminary dp to check substring-palindrome
        # isP[i][j] means if s[i:j] is palindrome, not including s[j]
        isP = [ [False] * (len(s)+1) for _ in range(len(s)+1)]
        for j in range(1, len(s)+1):
            for i in range(j):
                if j - i == 1: # single character
                    isP[i][j] = True
                elif s[i] == s[j-1]:
                    if j - i == 2 or isP[i+1][j-1]:
                        isP[i][j] = True
                        
        # back to min-cut problem
        dp = [float("inf")] * (len(s)+1)
        for i in range(len(s)+1):
            if isP[0][i]: # no further need for cut
                dp[i] = 0
            else:
                for j in range(i+1):
                    if isP[j][i]:
                        dp[i] = min(dp[i], dp[j]+1)
                    else:
                        dp[i] = min(dp[i], dp[j]+i-j)
        return dp[len(s)]
```
