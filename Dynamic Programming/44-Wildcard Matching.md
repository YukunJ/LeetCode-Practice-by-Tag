**44. Wildcard Matching**

```Tag: dynamic programming```

**Description:**

Given an input string (```s```) and a pattern (```p```), implement wildcard pattern matching with support for '```?```' and '```*```' where:

+ '```?```' Matches any single character.
+ '```*```' Matches any sequence of characters (including the empty sequence).

The matching should cover the **entire** input string (not partial).

**Example1**:

        Input: s = "aa", p = "a"
        Output: false
        Explanation: "a" does not match the entire string "aa".
        
**Example2**:

        Input: s = "aa", p = "*"
        Output: true
        Explanation: '*' matches any sequence.
        
**Example3**:

        Input: s = "cb", p = "?a"
        Output: false
        Explanation: '?' matches 'c', but the second letter is 'a', which does not match 'b'.

**Example4**:

        Input: s = "adceb", p = "*a*b"
        Output: true
        Explanation: The first '*' matches the empty sequence, while the second '*' matches the substring "dce".

**Example5**:

        Input: s = "acdcb", p = "a*c?b"
        Output: false

-----------

```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        """
        Use a similar dynamic programming technique as in 
                Question 10: regular Expression Matching
        let f[i][j] denotes if s[:i] and p[:j] could match
        the transition equation goes as follows:
        1. (p[j-1] is character)
            f[i][j] = ((s[i-1]==p[j-1]) and f[i-1][j-1])
        2. (p[j-1] == "?")
            f[i][j] = f[i-1][j-1]
        3. (p[j-1] == "*") step back 1 step
            it matches emtpy or something
            f[i][j] = f[i][j-1] | f[i-1][j]
        boundary case: f[0][0] = True
        
        denote n = len(s), m = len(p)
        Time Complexity : O(mn)
        Space Complexity : O(m) by storing only rolling dp vectors
        """

        def match(i: int, j: int) -> bool:
            """Helper Function for matching"""
            if i <= 0 or j <= 0:
                return False
            elif p[j-1] == '?':
                return True
            else:
                return s[i-1] == p[j-1]
            
        """
        # the full 2-D matrix version
        n, m = len(s), len(p)
        f = [ [False] * (m+1) for _ in range(n+1) ]
        f[0][0] = True
        
        for i in range(n+1):
            for j in range(1, m+1):
                if p[j-1] == '*':
                    f[i][j] = f[i][j-1] | f[i-1][j]
                else:
                    f[i][j] = (match(i, j) and f[i-1][j-1])
        return f[n][m]
        """
        
        # the space-efficient 1-D rolling dp vector version
        n, m = len(s), len(p)
        old_f, new_f = [False] * (m+1), [False] * (m+1)
        old_f[0] = True
        
        for j in range(1, m+1):
            # Careful here, boundary case handling, 
            # all '*' could match empty string
            if p[j-1] == '*':
                old_f[j] = True
            else:
                break
                
        for i in range(1, n+1):
            for j in range(1, m+1):
                if p[j-1] == '*':
                    new_f[j] = new_f[j-1] | old_f[j]
                else:
                    new_f[j] = (match(i, j) and old_f[j-1])
            old_f = new_f # update rolling
            new_f = [False] * (m+1) # re-initialization
            
        return old_f[m]
```
