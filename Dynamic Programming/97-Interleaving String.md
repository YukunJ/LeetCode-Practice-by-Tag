**97. Interleaving String**

```Tag: dynamic programming```

**Description:**

Given strings ```s1```, ```s2```, and ```s3```, find whether ```s3``` is formed by an interleaving of ```s1``` and ```s2```.

An **interleaving** of two strings ```s``` and ```t``` is a configuration where they are divided into **non-empty** substrings such that:

+ s = s1 + s2 + ... + sn
+ t = t1 + t2 + ... + tm
+ |n - m| <= 1
+ The ```interleaving``` is ```s1 + t1 + s2 + t2 + s3 + t3 + ...``` or ```t1 + s1 + t2 + s2 + t3 + s3 + ...```

**Note**: ```a + b``` is the concatenation of strings ```a``` and ```b```.

**Example1**:

![avatar](Fig/97-E1.jpg)

        Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
        Output: true

**Example2**:

        Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
        Output: false

**Example3**:

        Input: s1 = "", s2 = "", s3 = ""
        Output: true

-----------

```python
class Solution:
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        """
        use a dynamic programming scheme
        consider the dp[i][j] denote if s1[:i] and s2[:j] could make up interleaving of s3[:i+j-1]
        and the state-transition depends on two things:
        dp[i][j] = (dp[i-1][j] and s1[i-1] == s3[i+j-1]) or 
                    (dp[i][j-1] and s2[j-1] == s3[i+j-1])
        
        and we can use a rolling matrix to save space Complexity
        denote m := len(s1), n := len(s2)
        Time Complexity : O(m*n)
        Space Complexity : O(n)
        """
        m, n = len(s1), len(s2)
        if (m + n) != len(s3): # basic length check
            return False
        old_dp = [True] + [False] * n
        
        for j in range(1, n+1): # init the first row
            if old_dp[j-1] and s2[j-1] == s3[j-1]:
                old_dp[j] = True
            else:
                break
        
        for i in range(1, m+1): # rolling dp vector implementation
            # careful the first entry inherits from last one[0] position
            new_dp = [(old_dp[0] and s1[i-1] == s3[i-1])] + [False] * n
            for j in range(1, n+1):
                new_dp[j] = (old_dp[j] and s1[i-1] == s3[i+j-1]) or (new_dp[j-1] and s2[j-1] == s3[i+j-1])
            old_dp = new_dp
        
        return old_dp[n]
```
