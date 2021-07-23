**214. Shortest Palindrome**

```Tag : hashing/string```

**Description:**

You are given a string ```s```. You can convert ```s``` to a **palindrome** by adding characters in front of it.

Return the *shortest palindrome* you can find by performing this transformation.


**Example1**:

		Input: s = "aacecaaa"
		Output: "aaacecaaa"

 
**Example2**:
 
		Input: s = "abcd"
		Output: "dcbabcd"
      
-----------

**Solution1: String Hashing(not 100% safe)**

```python
class Solution:
    def shortestPalindrome(self, s: str) -> str:
        """
        One key observation in this question is that,
        we can always reverse and append the string s[1:][::-1] to the left
        and make it into a Palindrome
        Therefore, we are essentially trying to find the longest original palindrome starting from left of the stirng s
        Brute force search would take O(n^2), here we first present a "unsafe" hashing solution
        if a string s is Palindrome, then hash(s) and hash(s[::-1]) should be same, notice
        the "unsatefy" here is about hashing collision, even if hash(s1) and hash(s2) are equal, 
        we are not 100% sure s1 equal s2
        Here in solution, we are increment the hash value in a "dynamic programming" way, built upon last one
        
        denote n := len(s)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        n = len(s)
        base, mod, mul = 131, 10**9 + 7, 1
        leftHash, rightHash = 0, 0
        longest = -1
        for i in range(n):
            leftHash = (leftHash * base + ord(s[i])) % mod # read to the left, s[i] lowest digit
            rightHash = (rightHash + mul * ord(s[i])) % mod # read to the right, s[i] highest digit
            if leftHash == rightHash:
                longest = i
            mul = (mul * base) % mod

        toAdd = "" if longest == n-1 else s[longest+1:]
        return toAdd[::-1] + s   
```

-----------

**Solution2: KMP algorithm**

```python
class Solution:
    def shortestPalindrome(self, s: str) -> str:
        """
        We use part of KMP algorithm to solve this question
        In detail, we construct the "next" vector of string s in KMP algorithm such that
        next[i] = the max length j such the first j and last j length substring of s are equal
        since we want to find the longest Palindrome of s starting from left
        we concate s + "|" + s', where s' = s[::-1] and run "next" KMP algoirthm on it.
        then next[-1]=j mean s[:j] = s'[-j:] = s[::-1][-j:] = s[:j][::-1], the longest left Palindrome substring of s
        and it is known and the "next" vector construction takes O(len(s)) time

        denote n := len(s)
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        def generate_next_vector(s: str) -> List[int]:
            """Helper function: generate the 'next' vector in KMP"""
            n = len(s)
            nxt = [-1] + [0] * n
            for i in range(2, n+1):
                j = nxt[i-1]
                # if s[i-1] = s[j], directly plus 1
                # otherwise, j = nxt[j]
                while j != -1 and s[i-1] != s[j]:
                    j = nxt[j]
                nxt[i] = j + 1
            return nxt
        nxt = generate_next_vector(s+"|"+s[::-1])
        longest = nxt[-1]
        return s[longest:][::-1] + s
```