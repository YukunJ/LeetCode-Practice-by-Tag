**3. Longest Substring Without Repeating Characters**

```Tag: two pointers/sliding window```

**Description:**

Given a string ```s```, find the length of the **longest substring** without repeating characters.

**Example1**:

        Input: s = "abcabcbb"
        Output: 3
        Explanation: The answer is "abc", with the length of 3.

**Example2**:

        Input: s = "bbbbb"
        Output: 1
        Explanation: The answer is "b", with the length of 1.
        
**Example3**:

        Input: s = "pwwkew"
        Output: 3
        Explanation: The answer is "wke", with the length of 3.
        Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

**Example4**:

        Input: s = ""
        Output: 0

-----------

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        """
        This is a two-pointer sliding window question
        we maintain two pointers prev and aft, where 
        the substring s[prev:aft] is under our consideration
        while fixing a prev pointer, we try to expand our substring, i.e. move aft forward
        we want to add s[aft] into current substring, we need to check if it appears before
        if not, add it, and move on
        else, we may move the prev pointer forward
        
        Time complexity : O(n)
        Space complexity : O(1)
        """
        if not s:
            return 0
        memory = set() # record the occurence
        longest = 0
        n = len(s)
        right = 1 # the right boundary, we access s[right-1]
        for left in range(n):
            if left != 0: # delete the left boundary character
                memory.remove(s[left-1])
            while right <= n and s[right-1] not in memory:
                memory.add(s[right-1])
                right += 1
            longest = max(longest, right-left-1)
            if longest >= n-left: # early-stopping
                break
        return longest
```
