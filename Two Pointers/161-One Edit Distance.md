**161. One Edit Distance**

```Tag : two pointers/string```

**Description:**

Given two strings ```s``` and ```t```, return ```true``` if they are both one edit distance apart, otherwise return ```false```.

A string ```s``` is said to be one distance apart from a string ```t``` if you can:

+ Insert **exactly one** character into ```s``` to get ```t.```

+ Delete **exactly one** character from ```s``` to get ```t```.

+ Replace **exactly one** character of ```s``` with a different character to get ```t```.

**Example1:**

		Input: s = "ab", t = "acb"
		Output: true
		Explanation: We can insert 'c' into s to get t.

**Example2:**
		
		Input: s = "", t = ""
		Output: false
		Explanation: We cannot get t from s by only one step.

**Example3:**

		Input: s = "a", t = ""
		Output: true

**Example4:**

		Input: s = "", t = "A"
		Output: true

-----------

```python
class Solution:
    def isOneEditDistance(self, s: str, t: str) -> bool:
        """
        This is a two pointer problem
        we maintain two pointers and make sure len(s) <= len(t) for consistency
        as we iterate through the array, 
        the first place where s[ptr1] != t[ptr2] we decide one 2 cases:
        1) if len(s) = len(t): we replace one of them, and continue
        2) if len(s) < len(t): we delete the character in t and continue
        if we never reach "s[ptr1] != t[ptr2]", to edit once we require len(t) = len(s) + 1
        
        denote n := len(s), m := len(t)
        Time Complexity : O(n) one pass, when abs(n - m) > 1 it's O(1)
        Space Complexity : O(n) for string reconstruction
        """
        n, m = len(s), len(t)
        if abs(n-m) > 1: # boundary checking
            return False
        if n > m: # swap to ensure len(s) <= len(t)
            return self.isOneEditDistance(t, s)
        
        for i in range(n):
            if s[i] != t[i]:
                if n == m:
                    # case 1
                    return s[i+1:] == t[i+1:]
                else:
                    # case 2
                    return s[i:] == t[i+1:]
                
        # one edit happened before, t has to be one-character longer than s
        return m == n+1
```

