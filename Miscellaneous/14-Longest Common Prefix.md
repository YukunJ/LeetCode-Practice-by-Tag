**14. Longest Common Prefix**

```Tag: string/single pointer sliding```

**Description:**

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string ```""```.

**Example1**:

		Input: strs = ["flower","flow","flight"]
		Output: "fl"
        
**Example2**:

		Input: strs = ["dog","racecar","car"]
		Output: ""
		Explanation: There is no common prefix among the input strings.
        
-----------

```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        """
        The optimal case is that one string is a complete prefix subset of another
        namely the longest prefix length is min(len(str) for str in strs)
		we use a single pointer to check every character of each string, 
		if not matched directly return
		if matched, try check next character

		denote n := len(strs), m := min(len(string)) in strs
		Time Complexity : O(n*m)
		Space Complexity : O(1)
        """
        max_prefix_length = min([len(string) for string in strs]) if strs else 0
        n = len(strs)
        prefix = ""
        for ptr in range(max_prefix_length):
            prefix_char, flag = strs[0][ptr], True
            for i in range(1, n):
                if strs[i][ptr] != prefix_char:
                    flag = False
                    break
            if not flag:
                break
            prefix += prefix_char
        return prefix
```