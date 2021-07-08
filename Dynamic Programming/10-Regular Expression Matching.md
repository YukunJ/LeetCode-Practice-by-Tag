**10. Regular Expression Matching**

```Tag: dynamic programming```

**Description:**

Given an input string (```s```) and a pattern (```p```), implement regular expression matching with support for '```.```' and '```*```' where: 

+ '```.```' Matches any single character.​​​​

+ '```*```' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

**Example1**:

        Input: s = "aa", p = "a"
        Output: false
        Explanation: "a" does not match the entire string "aa".
        
**Example2**:

        Input: s = "aa", p = "a*"
        Output: true
        Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
        
**Example3**:

        Input: s = "ab", p = ".*"
        Output: true
        Explanation: ".*" means "zero or more (*) of any character (.)".

**Example4**:

        Input: s = "aab", p = "c*a*b"
        Output: true
        Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore, it matches "aab".

**Example5**:

        Input: s = "mississippi", p = "mis*is*p*."
        Output: false

-----------

```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        """
        We design a dynamic programming scheme to solve the problem
        use f[i][j] to denote if s[:i] and p[:j] could match in regular expression
        Two cases:
        1. (p[j-1]!="*") f[i][j] = (s[i-1]==p[j-1]) && (f[i-1][j-1]) 
            only true if last characters match and previous substrings match
        2. (p[j-1]=="*") 
            if (p[j-2]==s[i-1]): f[i][j] = f[i][j-2] | f[i-1][j]
            elif (p[j-2]!=s[i-1]): f[i][j] = f[i][j-2]
        and the boundary case is f[0][0] = True, empty string matches empty string
        
        denote n = len(s), m = len(p)
        Time Complexity : O(mn)
        Space Complexity : O(mn)
        """
        
        def match(i: int, j: int) -> bool:
            """Helper function"""
            if i <= 0 or j <= 0:
                return False
            return (p[j-1] == '.') or (s[i-1] == p[j-1])
        
        # init
        n, m = len(s), len(p)
        f = [ [False] * (m+1) for _ in range(n+1)]
        f[0][0] = True
        
        # dp process
        for i in range(n+1):
            for j in range(1, m+1):
                if p[j-1] != '*':
                    f[i][j] = (match(i,j) and f[i-1][j-1])
                else: # p[j-1] == '*'
                    if match(i,j-1):
                        f[i][j] = f[i][j-2] | f[i-1][j]
                    else:
                        f[i][j] = f[i][j-2]
        return f[n][m]                      
```
