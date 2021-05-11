**131. Palindrome Partitioning**

```Tag: DFS```

**Description:**

Given a string ```s```, partition ```s``` such that every substring of the partition is a **palindrome**. Return all possible palindrome partitioning of  ```s```.

A **palindrome** string is a string that reads the same backward as forward.

**Example1**:

        Input: s = "aab"
        Output: 1
        Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.

**Example2**:

        Input: s = "a"
        Output: 0 
        
**Example3**:

        Input: s = "ab"
        Output: 1

-----------

```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        """
        We use DFS and backtracking technique for this problem
        given a string s, assuming we already process the possible
            partition for s[:i] part, then we are interested in
        if s[i:j] is a palindrome, if so, we can reduce to process s[j:] 
            until it reaches the end of string

        Time Complexity : O(n*2^n) for worst case, if any substring is palindrome, 
                    we will have 2^(n-1) possible way of cutting, each of length O(n) to produce
                    and the preliminary pre-processing takes O(n^2) < O(n*2^n) in long term
        Space Complexity : O(n^2) for the isP substring-palindrome matrix 
                    and O(n) for stack-height for recursion
        """
        def dfs(s: str, idx: int, isP: List[List[bool]], path: List[str], res: List[List[str]]):
            if idx == len(s):
                res.append(path)
                return
            for index in range(idx, len(s)+1):
                if isP[idx][index]:
                    dfs(s, index, isP, path+[s[idx:index]], res)
        
        if not s: # boundary case
            return []
        
        # first do a preliminary dp to check substring-palindrome
        # isP[i][j] means if s[i:j] is palindrome, not including s[j]
        isP = [ [False] * (len(s)+1) for _ in range(len(s)+1)]
        for j in range(1, len(s)+1):
            for i in range(j):
                if j - i == 1: # single character
                    isP[i][j] = True
                elif s[i] == s[j-1]:
                    if j - i == 2 or isP[i+1][j-1]:
                        isP[i][j] = True
        res = []
        path = []
        dfs(s, 0, isP, path, res)
        return res
```
