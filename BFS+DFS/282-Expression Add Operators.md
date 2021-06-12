**282. Expression Add Operators**

```Tag: DFS```

**Description:**

Given a string num that contains only digits and an integer target, return all possibilities to add the binary operators '```+```', '```-```', or '```*```' between the digits of num so that the resultant expression evaluates to the ```target``` value.


**Example1**:

        Input: num = "123", target = 6
        Output: ["1*2*3","1+2+3"]


**Example2**:

        Input: num = "232", target = 8
        Output: ["2*3+2","2+3*2"]

**Example3**:

        Input: num = "105", target = 5
        Output: ["1*0+5","10-5"]
        
**Example4**:

        Input: num = "00", target = 0
        Output: ["0*0","0+0","0-0"]
        
**Example5**:

        Input: num = "3456237490", target = 9191
        Output: []

-----------

```python
class Solution:
    def addOperators(self, num: str, target: int) -> List[str]:
        """
        This is DFS backtracking problem
        In each step, we can do 4 things:
        1) extend the current position, for example, '1' merge with '0' become '10'
        2) apply '+'
        3) apply '-'
        4) apply '*'
        
        notice we need to keep track of last operand, 
            because '*' has higher operational precedence
        for example, we want 10 + 2 * 4, but when we are 10 + 2 we have 12, 
            we don't want 12 *4, we want to remember last operand is 2
        
        Time Complexity : O(4^n) in each step 4 possibities, exponential growth
        Space Complexity : O(4^n) answer string and recursion stack height
        """
        
        N = len(num)
        answer = []
        
        def recurse(index: int, prev_op: int, curr_op: int, value: int, string: list[str]):
            if index == N:
                if value == target and curr_op == 0:
                    # we have a fake '0+' at the beginning of string
                    answer.append("".join(string[1:]))
                return # return anyway, end reached
            
            curr_op = curr_op * 10 + int(num[index])
            if curr_op > 0:
                # extend the operand
                recurse(index+1, prev_op, curr_op, value, string)
            
            # Addition
            string.append('+'); string.append(str(curr_op))
            recurse(index+1, curr_op, 0, value+curr_op, string)
            string.pop(); string.pop()
            
            if string: 
                # can do subtraction and division if previous operand is not None
                
                # Subtraction
                string.append('-'); string.append(str(curr_op))
                recurse(index+1, -curr_op, 0, value-curr_op, string)
                string.pop(); string.pop()
                
                # Multiplication
                string.append('*'); string.append(str(curr_op))
                recurse(index+1, prev_op * curr_op, 0, value-prev_op+prev_op*curr_op, string)
                string.pop(); string.pop()
                
        string = []
        recurse(0, 0, 0, 0, string)
        return answer
```
