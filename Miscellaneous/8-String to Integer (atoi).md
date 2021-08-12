**8. String to Integer (atoi)**

```Tag : string/finite-state machine```

**Description:**

Implement the ```myAtoi(string s)``` function, which converts a string to a 32-bit signed integer (similar to C/C++'s ```atoi``` function).

The algorithm for ```myAtoi(string s)``` is as follows:

1. Read in and ignore any leading whitespace.

2. Check if the next character (if not already at the end of the string) is ```'-'``` or ```'+'```. Read this character in if it is either. This determines if the final result is negative or positive respectively. Assume the result is positive if neither is present.

3. Read in next the characters until the next non-digit charcter or the end of the input is reached. The rest of the string is ignored.

4. Convert these digits into an integer (i.e. ```"123" -> 123```, ```"0032" -> 32```). If no digits were read, then the integer is ```0```. Change the sign as necessary (from step 2).

5. If the integer is out of the 32-bit signed integer range ```[-2^31, 2^31 - 1]```, then clamp the integer so that it remains in the range. Specifically, integers less than ```-2^31``` should be clamped to ```-2^31```, and integers greater than ```2^31 - 1``` should be clamped to ```2^31 - 1```.

6. Return the integer as the final result.

**Note:**

+ Only the space character ```' '``` is considered a whitespace character.

+ **Do not ignore** any characters other than the leading whitespace or the rest of the string after the digits.

**Example1:**

		Input: s = "42"
		Output: 42

**Example2:**

		Input: s = "   -42"
		Output: -42

**Example3:**

		Input: s = "4193 with words"
		Output: 4193

**Example4:**

		Input: s = "words and 987"
		Output: 0

**Example5:**

		Input: s = "-91283472332"
		Output: -2147483648

**Constraints:**

+ ```s``` consists of English letters (lower-case and upper-case), digits ```(0-9)```, ```' '```, ```'+'```, ```'-'```, and ```'.'```.

-----------

**Solution1: Ordinary**

```python
class Solution:
    def myAtoi(self, s: str) -> int:
        """
        This is a not very difficult question with many traps
        we need to be careful about all kinds of boundary case
        essentially we follow the procedures given
        (and some error prompts from the testing cases)
        
        denote n := len(s)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        INT_MAX, INT_MIN = 2 ** 31 -1, -1 * 2**31 # the clamp boundary
        result = 0
        sign = 1 
        n = len(s)
        pt = 0 # the sliding pointers
        
        # 1. remove leading whitespace
        while pt < n and s[pt] == " ":
            pt += 1
        if pt == n: # a whole empty string
            return result
        
        # 2. read the sign
        if s[pt] == '-' or s[pt] == '+':
            # there is explicit sign symbol
            if s[pt] == '-':
                sign = -1
            pt += 1
            
        # 3. & 4. read the rest until a non-digit character is read
        while pt < n and s[pt].isdigit():
            digit = int(s[pt])
            result = 10 * result + digit
            pt += 1
        
        # 5. clamp the result
        result = min(INT_MAX, max(INT_MIN, sign * result))
        
        # 6. return the answer
        return result
```

-----------

**Solution2: Finite State Machine**

```python
class Automaton:
    """
    Helper class: Finite State Machine
    there are 4 states: 'start', 'signed', 'in_number' and 'end'    
    """
    
    def __init__(self):
        self.sign = 1
        self.result = 0
        self.state = 'start'
        self.map = {
            'start'     : ['start', 'signed', 'in_number', 'end'],
            'signed'    : ['end', 'end', 'in_number', 'end'],
            'in_number' : ['end', 'end', 'in_number', 'end'],
            'end'       : ['end', 'end', 'end', 'end']
        }
    
    def get_label(self, c: str) -> int:
        """return the state-transition label given next character c"""
        if c.isspace():
            return 0
        elif c == '+' or c == '-':
            return 1
        elif c.isdigit():
            return 2
        else: 
            return 3
    
    def process(self, c: str) -> None:
        """state transition and processing"""
        self.state = self.map[self.state][self.get_label(c)]
        if self.state == 'in_number':
            self.result = 10 * self.result + int(c)
        elif self.state == 'signed':
            if c == '-':
                self.sign = -1
                
class Solution:
    def myAtoi(self, s: str) -> int:
        """
        We could also use Finite-State Machine to write code in a very clear and concise way
        for more knowledge on finite-state machine, see reference answer with detailed steps
        https://leetcode-cn.com/problems/string-to-integer-atoi/solution/zi-fu-chuan-zhuan-huan-zheng-shu-atoi-by-leetcode-/
        
        denote n := len(s)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        INT_MIN, INT_MAX = - 2 ** 31, 2 ** 31 -1
        my_machine = Automaton()
        for c in s:
            my_machine.process(c)
        # clip the number within INT32 range
        return max(INT_MIN, min(INT_MAX, my_machine.sign * my_machine.result))
```
