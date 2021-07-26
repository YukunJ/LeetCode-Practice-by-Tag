**6. ZigZag Conversion**

```Tag : string```

**Description:**

The string ```"PAYPALISHIRING"``` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

        P   A   H   N
        A P L S I I G
        Y   I   R
        
And then read line by line: ```"PAHNAPLSIIGYIR"```

Write the code that will take a string and make this conversion given a number of rows:

        ```string convert(string s, int numRows);```

**Example1**:

        Input: s = "PAYPALISHIRING", numRows = 3
        Output: "PAHNAPLSIIGYIR"
        
**Example2**:      

        Input: s = "PAYPALISHIRING", numRows = 4
        Output: "PINALSIGYAHRPI"
        Explanation:
        P     I    N
        A   L S  I G
        Y A   H R
        P     I
 
 **Example3**:      
 
         Input: s = "A", numRows = 1
         Output: "A"
 
-----------

```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        """
        It takes a while to understand what the question is about
        essentially it's placing the characters in the string in a 'up-and-down' way
        if you compress all the columns in the piture, you see the row-index is down and up and so on
        There are we just need to simulate the process 
        
        denote n := len(s)
        Time Complexity : O(n) one pass-through the string s
        Space Complexity : O(n) extra matrix storage
        """
        if numRows < 2: # boundary case
            return s
        zigzag = [""] * numRows
        row, flag = 0, -1
        
        for char in s:
            zigzag[row] += char
            if row == 0 or row == numRows-1: # revert direction
                flag = -flag
            row += flag
        
        return "".join(zigzag)
```
