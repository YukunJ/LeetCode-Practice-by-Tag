**680. Valid Palindrome II**

```Tag : Two Pointers/Greedy```

**Description:**

Given a string ```s```, return ```true``` if the ```s``` can be palindrome after deleting **at most one** character from it.

**Example1:**

		Input: s = "aba"
		Output: true

**Example2:**

		Input: s = "abca"
		Output: true
		Explanation: You could delete the character 'c'.

**Example3:**

		Input: s = "abc"
		Output: false

-----------

```python
class Solution:
    def validPalindrome(self, s: str) -> bool:
        """
        we are entitled one chance of deletion in this question
        We proceeds as normal Palindrome validation problem, with two pointers approach
        when s[left] != s[right], one of them must be deleted
        Then if either deleting s[left] yields a Palindrome or deleting s[right]
        
        denote n := len(s)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        def subPalindrome(s: str, left:int, right:int) -> bool:
            """Helper function: validate if a string s[left:right+1] is Palindrome"""
            while left < right:
                if s[left] != s[right]:
                    return False
                left += 1
                right -= 1
            return True
        
        left, right = 0, len(s)-1
        while left < right:
            if s[left] == s[right]:
                left += 1
                right -= 1
            else:
                # must do deletion
                # try either s[left] or s[right]
                return subPalindrome(s, left+1, right) or subPalindrome(s, left, right-1)
        # reach here means it's originally a Palindrome without any deletion
        return True
```