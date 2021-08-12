**1249. Minimum Remove to Make Valid Parentheses**

```Tag : stack/string```

**Description:**

Given a string ```s``` of ```'('``` , ```')'``` and lowercase English characters. 

Your task is to remove the minimum number of parentheses  ```'('``` or ```')'```, in any positions so that the resulting parentheses string is valid and return **any** valid string.

Formally, *a parentheses string* is valid if and only if:

+ It is the empty string, contains only lowercase characters, or

+ It can be written as ```AB``` (```A``` concatenated with ```B```), where ```A``` and ```B``` are valid strings, or

+ It can be written as (```A```), where ```A``` is a valid string.

**Example1:**

		Input: s = "lee(t(c)o)de)"
		Output: "lee(t(c)o)de"
		Explanation: "lee(t(co)de)" , "lee(t(c)ode)" would also be accepted.

**Example2:**

		Input: s = "a)b(c)d"
		Output: "ab(c)d"

**Example3:**

		Input: s = "))(("
		Output: ""
		Explanation: An empty string is also valid.

**Example4:**

		Input: s = "(a(b(c)d)"
		Output: "a(b(c)d)"

**Hints:**

+ Each prefix of a balanced parentheses has a number of open parentheses greater or equal than closed parentheses, similar idea with each suffix.

+ Check the array from left to right, remove characters that do not meet the property mentioned above, same idea in backward way.

-----------

```python
class Solution:
    def minRemoveToMakeValid(self, s: str) -> str:
        """
        Following the hints, the key observation is that:
        when going left to right, at any index for a valid parentheses:
            number of '(' >= number of ')' always
        when going right to left, at any index for a valid parentheses: 
            number of ')' >= number of '(' always number of '(' >= number of ')'
        Therefore, we do left and right two-pass through the string to filter out invalid parentheses. This is approach1
        
        Another alternative is to do one pass, 
            we use a counter to count the number of '(' - number of ')', keep it >= 0
        
        And in the end remember to remove extra '(' at the end of the string if unclosed finally
        
        denote n := len(s)
        Time Complexity : O(n)
        Space Complexity : O(n) stack and string builder
        """
        if not s: # boundary case
            return s
        
        n = len(s)
        res = []
        stack = []
        index_to_remove = set()
        counter = 0
        for idx, char in enumerate(s):
            if char in '()':
                # only care about parentheses:
                if char == ')' and counter <= 0: 
                    # a closing ')' to be removed
                    index_to_remove.add(idx)
                elif char == ')':
                    counter -= 1
                    stack.pop() # pop a matching '('
                else: # the index of a '('
                    counter += 1
                    stack.append(idx)
                    
        # the remaining unclosed '(' needs also be removed
        index_to_remove = index_to_remove.union(set(stack))
        for idx, char in enumerate(s):
            if idx not in index_to_remove:
                res.append(char)
        # string builder
        return "".join(res)
```