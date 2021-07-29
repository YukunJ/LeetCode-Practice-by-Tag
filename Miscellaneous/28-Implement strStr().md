**28. Implement strStr()**

```Tag : two pointers/string matching/KMP```

**Description:**

Implement ```strStr()```.

Return the index of the first occurrence of ```needle``` in ```haystack```, or ```-1``` if ```needle``` is not part of ```haystack```.

**Clarification:**

What should we return when ```needle``` is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return ```0``` when ```needle``` is an empty string. This is consistent to C's ```strstr()``` and Java's ```indexOf()```.


**Example1**:

        Input: haystack = "hello", needle = "ll"
        Output: 2
 
**Example2**:
 
        Input: haystack = "aaaaa", needle = "bba"
        Output: -1

**Example3**:

        Input: haystack = "", needle = ""
        Output: 0

-----------

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        """
        A brute force matching would take O(n*m)
        Here we use KMP algorithm to do the string pattern matching
        for a detailed instruction on KMP algorithm, may refer to :
        https://leetcode-cn.com/problems/implement-strstr/solution/shua-chuan-lc-shuang-bai-po-su-jie-fa-km-tb86/

        denote n := len(haystack), m := len(needle)
        Time Complexity : O(n+m)
        Space Complexity : O(m)
        """
        i, j = 0, 0
        nxt = self.generate_next_vector(needle)
        while i < len(haystack) and j < len(needle):
            if haystack[i] == needle[j]:
                i += 1
                j += 1
            elif j > 0:
                j = nxt[j-1]
            else:
                i += 1
        if j == len(needle): # match needle
            return i - j
        else: # haystack all searched, no result matching
            return -1
        
       
    def generate_next_vector(self, substr: str) -> List[int]:
        """
        Helper function: generate the 'next' vector in KMP algorithm
        Takes O(m) time and O(m) space, where m := len(substr)
        """
        # nxt[i] := the longest prefix and postfix substring in substr[:i+1], i.e. substr[i] is included
        # and the prefix and postfix should not overlap with each other
        j, i = 0, 1
        nxt = [0] * len(substr)
        while i < len(nxt):
            if substr[j] == substr[i]:
                nxt[i] = j + 1
                i += 1
                j += 1
            elif j > 0:
                j = nxt[j-1]
            else:
                nxt[i] = 0
                i += 1
        return nxt   
```
