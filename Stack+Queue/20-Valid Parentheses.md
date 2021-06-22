**20. Valid Parentheses**

```Tag: Stack```

**Description:**

Given a string ```s``` containing just the characters ```'('```, ```')'```, ```'{'```, ```'}'```, ```'[`'``` and ```']'```, determine if the input string is valid.

An input string is valid if:

+ Open brackets must be closed by the same type of brackets.

+ Open brackets must be closed in the correct order.

**Example1**:

        Input: s = "()"
        Output: true

**Example2**:

        Input: s = "()[]{}"
        Output: true
        
**Example3**:

        Input: s = "(]"
        Output: false

**Example4**:

        Input: s = "([)]"
        Output: false 

**Example5**:

        Input: s = "{[]}"
        Output: true
        
-----------

```python
class Solution:
    def isValid(self, s: str) -> bool:
        """
        We use a Stack data structure to valid is a parenthesis is properly formed
        In essence,
        when we meet a left symbol '(' '[' '{' we push it into the stack
        when we mee a right symbol ')' ']' '}' 
            we pop an item from the stack to check if it's the correct left match
        In the end we also need to check if 
            the stack is empty to make sure all left symbol is closed in the end

        denote n := len(s)
        Time Complexity : O(n)
        Space Complexity: O(n)
        """
        Stack = []
        left = ['(', '{', '[']
        map = {')' : '(', '}' : '{', ']' : '['}
        for c in s:
            if c in left:
                Stack.append(c)
            else:
                if not Stack or not Stack[-1] == map[c]: 
                    # fail to match, no prior left or left type doesn't match
                    return False
                Stack.pop()
        return len(Stack) == 0 # all lefts must be closed in the end
```
