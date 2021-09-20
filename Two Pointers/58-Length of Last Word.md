**58. Length of Last Word**

```Tag : string```

**Description:**

Given a string ```s``` consisting of some words separated by some number of spaces, return the length of the **last** word in the string.

A **word** is a maximal substring consisting of non-space characters only.


**Example1:**

        Input: s = "Hello World"
        Output: 5
        Explanation: The last word is "World" with length 5.
        
**Example2:**

        Input: s = "   fly me   to   the moon  "
        Output: 4
        Explanation: The last word is "moon" with length 4.
        
**Example3:**

        Input: s = "luffy is still joyboy"
        Output: 6
        Explanation: The last word is "joyboy" with length 6.

-----------

```python
class Solution:
    def lengthOfLastWord(self, s: str) -> int:
        """
        we iterate through the string s backward, one point sliding approach
        we will skip the first few spaces and stop after
        we read the last word and meet another space
        
        denote n := len(s)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        ptr, length = len(s)-1, 0
        while ptr >= 0:
            if not s[ptr].isspace():
                length += 1
            elif length > 0:
                # this "length > 0" enables us to skip the last few spaces
                # before meeting any words
                return length 
            ptr -= 1
        return length
```
