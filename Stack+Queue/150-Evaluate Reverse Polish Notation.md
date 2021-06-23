**150. Evaluate Reverse Polish Notation**

```Tag: Stack```

**Description:**

Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are +, -, *, and /. Each operand may be an integer or another expression.

**Note** that division between two integers should truncate toward zero.

It is guaranteed that the given RPN expression is always valid. That means the expression would always evaluate to a result, and there will not be any division by zero operation.

**Example1**:

        Input: tokens = ["2","1","+","3","*"]
        Output: 9
        Explanation: ((2 + 1) * 3) = 9

**Example2**:

        Input: tokens = ["4","13","5","/","+"]
        Output: 6
        Explanation: (4 + (13 / 5)) = 6
        
**Example3**:

        Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
        Output: 22
        Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
        = ((10 * (6 / (12 * -11))) + 17) + 5
        = ((10 * (6 / -132)) + 17) + 5
        = ((10 * 0) + 17) + 5
        = (0 + 17) + 5
        = 17 + 5
        = 22

-----------

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        """
        The reverse Polish notation is such that, when encounter an operator, 
        the previous two numbers seen are the operands for this operation.
        For example [1, 2, '+'], when see the '+', we take out 1 and 2 and calculate 1 + 2 = 3
        However the result should bd remember that [1, 2, '+', 3, '-'] would be (1 + 2 ) - (3) = 0
        We use a Stack to simulate the process that when see an operand, we push into the stack
        when see an operator, we pop out two operands and do the calculation and then push the result back to stack

        denote n := len(tokens)
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        
        def calc(op1: int, op2: int, operator: str) -> int:
            """Helper function: deal with detailed calculations"""
            if operator == '+':
                return op1 + op2
            elif operator == '-':
                return op1 - op2
            elif operator == '*':
                return op1 * op2
            elif operator == '/':
                if (op1 > 0 and op2 > 0) or (op1 < 0 and op2 < 0):
                    return op1 // op2
                else:
                    # negative division, truncate toward zero
                    return -1 * (abs(op1) // abs(op2))
        
        Stack = []
        for token in tokens:
            if token in ['+', '-', '*', '/']:
                op2 = Stack.pop()
                op1 = Stack.pop()
                Stack.append(calc(op1, op2, token))
            else:
                Stack.append(int(token))
        
        return Stack.pop()
```
