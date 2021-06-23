**224. Basic Calculator**

```Tag: Stack/Math```

**Description:**

Given a string ```s``` representing a valid expression, implement a basic calculator to evaluate it, and return the result of the evaluation.

Note: You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as ```eval()```.


**Example1**:

        Input: s = "1 + 1"
        Output: 2

**Example2**:

        Input: s = " 2-1 + 2 "
        Output: 3
        
**Example3**:

        Input: s = "(1+(4+5+2)-3)+(6+8)"
        Output: 23
        
**Example4**:

        Input: s = "+48 + -48"
        Output: 0
        Explanation: Numbers can have multiple digits and start with +/-.
        
-----------

```python
class Solution:
    def calculate(self, s: str) -> int:
        """
        We use a stack to simulate the calculation process
        since we only have '+' or '-', a sign signal would suffice to store the operator
        when we encounter a '('
            we need to priorize the calculation in the parenthesis,
        so we will store the current result and operator in the stack
        when we later encounter a closing ')' 
            we pop the operator and left-operator out to perform the calculation

        denote n := len(s)
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        Stack = []
        num, res, sign = 0, 0, 1
        for c in s:
            if c.isdigit():
                # operand might be two digits, three digits, etc.
                num = 10 * num + int(c)
            elif c == '+' or c == '-':
                # a number ends, store its value and reset
                res += sign * num
                num = 0
                sign = 1 if c == '+' else -1
            elif c == '(':
                # priortize the calculation in parenthesis, store curr result
                Stack.append(res)
                Stack.append(sign)
                num, res, sign = 0, 0, 1
            elif c == ')':
                # pop out previous result and calculate
                res += sign * num
                num = 0
                res *= Stack.pop() # consider the sign before the parenthesis
                res += Stack.pop()
        res += sign * num
        return res

```
