**647. Palindromic Substrings**

```Tag : dynamic programming```

**Description:**

Given a string ```s```, return the number of **palindromic substrings** in it.

A string is a **palindrome** when it reads the same backward as forward.

A **substring** is a contiguous sequence of characters within the string.

**Example1**:

        Input: s = "abc"
        Output: 3
        Explanation: Three palindromic strings: "a", "b", "c".
        
**Example2**:      

        Input: s = "aaa"
        Output: 6
        Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
 
 **Hints:**
 
 + How can we reuse a previously computed palindrome to compute a larger palindrome?
 
 + If “aba” is a palindrome, is “xabax” and palindrome? Similarly is “xabay” a palindrome?
 
 + Complexity based hint: If we use brute-force and check whether for every start and end position a substring is a palindrome we have ```O(n^2)``` start - end pairs and ```O(n)``` palindromic checks. Can we reduce the time for palindromic checks to ```O(1)``` by reusing some previous computation?
 
-----------

**Solution1: Standard Dynamic Programming**

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        """
        Essentially this is a problem asking if s[i:j] is a Palindrome
        we use dynamic programming to solve this problem,
        for i < j:
        denote d[i][j] := if s[i:j] is a Palindrome
        1) if j-i == 1: single-character, True
        2) if j-i == 2: two-character, s[i] == s[j-1]
        3) if j-i > 2: (s[i]==s[j-1]) and dp[i+1][j-1]
        
        denote n := len(s), notice we could save space complexity by only storing 1 column and iteratively update it
        Time Complexity : O(n^2)
        Space Complexity : O(n)
        """
        n = len(s)
        ans = 0
        col = [False] * (n+1)
        # column-wise iteration
        for j in range(1, n+1):
            new_col = [False] * (n+1)
            for i in range(j):
                if (j - i) == 1:
                    new_col[i] = True
                elif (j - i) == 2:
                    new_col[i] = True if s[i] == s[j-1] else False
                else:
                    new_col[i] = (s[i] == s[j-1] and col[i+1])
                
                if new_col[i]:
                    ans += 1
            col = new_col # state compression
        return ans
```
-----------

**Solution2: Center-based search**

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        """
        Essentially this is a problem asking if s[i:j] is a Palindrome
        we use dynamic programming to solve this problem,
        for i < j:
        denote d[i][j] := if s[i:j] is a Palindrome
        1) if j-i == 1: single-character, True
        2) if j-i == 2: two-character, s[i] == s[j-1]
        3) if j-i > 2: (s[i]==s[j-1]) and dp[i+1][j-1]
        However, in this way even if use state compression, 
        it is O(n^2) time complexity and O(n) space complexity
        we could reduce it to O(1) space compleity by "enumerate" the center of Palindrome
        the Palindrome could be 1-character or 2-character, we do 2 pass-through
        and expand towards to side using two pointers
        
        denote n := len(s)
        Time Complexity : O(n^2)
        Space Complexity : O(1)
        """
        ans = 0
        n = len(s)
        
        # odd, 1-character center pass-through
        for center in range(n):
            ans += 1 # 1-character is always a Palindrome
            i, j = center-1, center+1
            while 0 <= i and j < n and s[i] == s[j]:
                i, j = i-1, j+1
                ans += 1
        
        # even, 2-character center pass-through
        for left_center in range(n-1):
            if s[left_center] == s[left_center+1]:
                ans += 1 # a valid 2-character center found
                i, j = left_center-1, left_center+2
                while 0 <= i and j < n and s[i] == s[j]:
                    i, j = i-1, j+1
                    ans += 1
        
        return ans
```
