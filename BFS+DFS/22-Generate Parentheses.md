**22. Generate Parentheses**

```Tag: DFS```

**Description:**

Given ```n``` pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

**Example1**:

        Input: n = 3
        Output: ["((()))","(()())","(())()","()(())","()()()"]

**Example2**:

        Input: n = 1
        Output: ["()"]

-----------

```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        """
        This is a DFS problem backtracking
        In the construction of a valid parenthesis, 
        we can add left '(' as long as not exceed n
        The condition for adding a right ')' is that,
        there must be more left '(' than right ')' currently
        
        Time Complexity : O(4^n/n^{0.5}) 
            asymptotically the number of valid parenthesis is Catalan number
            and each ans's construction takes O(n)
        Space Complexity : O(n) recursion stack
        """
        parenthesis = []
        parentheses = []
        if not n:
            return []
        def dfs(n: int, left: int, right: int):
            if left + right == 2 * n:
                parentheses.append("".join(parenthesis))
            else:
                # the condition to add a left parenthesis
                if left < n:
                    parenthesis.append('(')
                    dfs(n, left+1, right)
                    parenthesis.pop()
                # the condtion to add a right parenthesis
                if left > right and right < n:
                    parenthesis.append(')')
                    dfs(n, left, right+1)
                    parenthesis.pop()
        dfs(n, 0, 0)
        return parentheses
```
