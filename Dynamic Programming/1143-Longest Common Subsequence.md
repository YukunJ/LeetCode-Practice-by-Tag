**1143. Longest Common Subsequence**

```Tag: dynamic programming```

**Description:**

Given two strings ```text1``` and ```text2```, return the length of their longest **common subsequence**. If there is no common subsequence, return ```0```.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

For example, ```"ace"``` is a subsequence of ```"abcde"```.
A **common subsequence** of two strings is a subsequence that is common to both strings.

**Example1**:

        Input: text1 = "abcde", text2 = "ace" 
        Output: 3  
        Explanation: The longest common subsequence is "ace" and its length is 3.

**Example2**:

        Input: text1 = "abc", text2 = "abc"
        Output: 3
        Explanation: The longest common subsequence is "abc" and its length is 3.   
        
**Example3**:

        Input: text1 = "abc", text2 = "def"
        Output: 0
        Explanation: There is no such common subsequence, so the result is 0.  

-----------

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        """
        A classic dynamic programming problem
        
        since we don't require the subsequence be consecutive
        let dp[i][j] denote the longestCommonSubsequence of text1[:i] and text2[:j]
        2 cases:
            #1. if text1[i] == text2[j] : we get length += 1 in this case from last step
            #2. if text1[i] != text2[j] : we may get rid of any one of text1[i] or text2[j]
            and the answer should remain same as last step, but which one? Take max
        
        Time Complexity: O(m*n) where m=len(text1), n=len(text2)
        Space Complexity: O(m*n) need store the whole matrix dp
        """
        
        m, n = len(text1), len(text2)
        dp = [[0] * (n+1) for i in range(m+1)] # init the dp matrix
        for i in range(1, m+1):
            for j in range(1, n+1):
                if text1[i-1] == text2[j-1]:
                    # new char matched, plus 1
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    # may get rid any of new char ending
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
        return dp[m][n]
```
