**224. Basic Calculator**

```Tag : stack/string/math```

**Description:**

Given a string ```s``` representing a valid expression, implement a basic calculator to evaluate it, and return the result of the evaluation.

**Note**: You are **not** allowed to use any built-in function which evaluates strings as mathematical expressions, such as ```eval()```.

**Example1:**

		Input: s = "1 + 1"
		Output: 2

**Example2:**

		Input: s = " 2-1 + 2 "
		Output: 3

**Example3:**

		Input: s = "(1+(4+5+2)-3)+(6+8)"
		Output: 23

-----------

```python
class Solution:
    def calculate(self, s: str) -> int:
        """
        This is a troublesome parsing problem using stack
        It's not that hard, but just easily to get it wrong
        since we only have '+' and '-' operation in this question
        whenever we see an opening '(', we will store the previous operand and sign to stack
        when we see a closing ')', we will add current result to the previous one in the stack
        
        denote n := len(s)
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        stack = []
        curr, sign = 0, 1
        ptr = 0
        while ptr < len(s):
            if s[ptr].isdigit():
                new_number = 0
                while ptr < len(s) and s[ptr].isdigit():
                    # retrieve a whole number
                    new_number = 10 * new_number + int(s[ptr])
                    ptr += 1
                curr += sign * new_number
            elif s[ptr] == '(':
                # a new predence scope, record current number
                stack.append(curr)
                stack.append(sign)
                curr, sign = 0, 1 # reset current number and sign 
                ptr += 1
            elif s[ptr] == ')':
                # the predence scope ends
                last_sign = stack.pop()
                last_curr = stack.pop()
                curr = last_curr + last_sign * curr
                ptr += 1
            elif s[ptr] in ['+', '-']:
                sign = -1 if s[ptr] == '-' else 1
                ptr += 1
            else:
                # space 
                ptr += 1
                
        stack.append(curr)
        return sum(stack)
```
