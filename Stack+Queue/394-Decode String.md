**394. Decode String**

```Tag: Stack/Recursion```

**Description:**

Given an encoded string, return its decoded string.

The encoding rule is: ```k[encoded_string]```, where the ```encoded_string``` inside the square brackets is being repeated exactly ```k``` times. Note that ```k``` is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, ```k```. For example, there won't be input like ```3a``` or ```2[4]```.

**Example1**:

        Input: s = "3[a]2[bc]"
        Output: "aaabcbc"

**Example2**:

        Input: s = "3[a2[c]]"
        Output: "accaccacc"
        
**Example3**:

        Input: s = "2[abc]3[cd]ef"
        Output: "abcabccdcdcdef"

**Example4**:

        Input: s = "abc3[cd]xyz"
        Output: "abccdcdcdxyz"
        
-----------

**Solution1: Stack**

```python
class Solution:
    def decodeString(self, s: str) -> str:
        """
        This is a Stack problem
        In consideration of nested decoding structure, 
        we want to decode the string inner to outer, layer by layer
        In specific, we main a stack and traversal the string s:
        1) when we see a digit, we scan and store a number(might be multiple-digits) into stack
        2) when we see a left '[' or character, push into stack
        3) when we see a right closing ']', we pop stuff out of stack until '[' 
            and pop one more time to get the corresponding multiplicative number
            and concantate the string with multiplicity and push it back to the stack
        In the end we just concantate all stuff in the stack and get the answer
        
        denote S := length of string after decoding, S > len(s) usually
        Time Complexity : O(S)
        Space Complexity : O(S)
        """
        stack = []
        ptr = 0
        while ptr < len(s):
            if s[ptr].isdigit():
                num = 0
                while s[ptr].isdigit():
                    # scan a possible multi-digit number
                    num = 10 * num + int(s[ptr])
                    ptr += 1
                stack.append(num)
            elif s[ptr] == ']':
                curr_s = ''
                while stack[-1] != '[':
                    # reversed order poping here
                    curr_s = stack.pop() + curr_s
                stack.pop() # pop out the '['
                multi = stack.pop() # before the '[' must be a number
                stack.append(curr_s * multi)
                ptr += 1
            else:
                stack.append(s[ptr])
                ptr += 1
        
        return "".join(stack)
```

-----------

**Solution2: Recursion**

```python
class Solution:
    def decodeString(self, s: str) -> str:
        """
        We could also use recursion to solve the problem level by level
        Time Complexity : O(S)
        Space Complexity : O(S)
        """
        def dfs(s: str, i: int) -> Tuple[int, str]:
            """ Helper function: solve the problem starting with index i
            We call recursion every bracket '[]' level encountered """
            num, res = 0, ''
            while i < len(s):
                if s[i].isdigit(): # scan a number
                    num = 10 * num + int(s[i])
                elif s[i] == '[': 
                    # call recursion to deal with next bracket
                    # update pointer, the multiply the string in bracket with num
                    i, next_res = dfs(s, i+1)
                    res += num * next_res
                    num = 0
                elif s[i] == ']':
                    # end of recursion
                    return i, res
                else:
                    res += s[i]
                i += 1
                
            return i, res
        
        _, res = dfs(s, 0)
        return res
```
