**76. Minimum Window Substring**

```Tag: two pointers/sliding window```

**Description:**

Given two strings ```s``` and ```t``` of lengths ```m``` and ```n``` respectively, return the **minimum window substring** of ```s``` such that every character in ```t``` (**including duplicates**) is included in the window. If there is no such substring, return the empty string ```""```.

The testcases will be generated such that the answer is **unique**.

A **substring** is a contiguous sequence of characters within the string.

**Hints**:

+ Use two pointers to create a window of letters in S, which would have all the characters from T.

+ Since you have to find the minimum window in S which has all the characters from T, you need to expand and contract the window using the two pointers and keep checking the window for all the characters. This approach is also called Sliding Window Approach. 

        L ------------------------ R , Suppose this is the window that contains all characters of T 
                          
        L----------------- R , this is the contracted window. We found a smaller window that still contains all the characters in T

        When the window is no longer valid, start expanding again using the right pointer. 

**Follow up**: Could you find an algorithm that runs in ```O(m + n)``` time?

**Example1**:

        Input: s = "ADOBECODEBANC", t = "ABC"
        Output: "BANC"
        Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.

**Example2**:

        Input: s = "a", t = "a"
        Output: "a"
        Explanation: The entire string s is the minimum window.
        
**Example3**:

        Input: s = "a", t = "aa"
        Output: ""
        Explanation: Both 'a's from t must be included in the window.
        Since the largest window of s only has one 'a', return empty string.



-----------

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        """
        This is a sliding window problem, we maintain a parif of left & right pointers
        while fix left, we try expand right pointer to contain the whole t string
        then we try to contract the window by moving left pointer to right
        During the process, we need hashtable to record the occurence of characters in t
        notice the set of ASCII characters are finite, so checkin operating is O(1) operation
        
        denote n := len(s), m := len(t)
        Time Complexity : O(n+m)
        Space Complexity : O(1)
        """
        # initialization
        from collections import defaultdict
        window = defaultdict(int) # the char occurence in current window
        t_window = defaultdict(int) # the char occurence in t string
        for c in t:
            t_window[c] += 1
        
        def valid():
            """Helper function: check if the current window contains t string"""
            for c, occu in t_window.items():
                if window[c] < occu: # the current window invalid
                    return False
            return True
        
        n, m = len(s), len(t)
        start, length = -1, float('inf')
        left, right = 0, -1
        while right < n-1:
            # expand the window moving right ptr to right
            window[s[right+1]] += 1
            right += 1
            # contract the window moving left ptr to right
            while valid() and left <= right:
                # update 
                if right - left + 1 < length:
                    length = right - left + 1
                    start = left
                # contract
                window[s[left]] -= 1
                left += 1
        
        return s[start:start+length] if start != -1 else ""
```
