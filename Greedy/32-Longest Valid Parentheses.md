**32. Longest Valid Parentheses**

```Tag : dynamic programming/two pointers/stack/greedy```

**Description:**

Given a string containing just the characters ```'('``` and ```')'```, find the length of the longest valid (well-formed) parentheses substring.


**Example1:**

		Input: s = "(()"
		Output: 2
		Explanation: The longest valid parentheses substring is "()".

**Example2:**

		Input: s = ")()())"
		Output: 4
		Explanation: The longest valid parentheses substring is "()()".

**Example3:**

		Input: s = ""
		Output: 0

-----------

**Solution1: Dynamic Programming**

```python
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        """
        we consider using a dynamic approach to solve the problem
        denote dp[i] := the length of longest valid parthenesis ending with s[i]
        we notice two things
        firstly, to be a valid parenthesis ending with s[i], s[i] has to be ')'
        secondly, if s[i-1] == '(', the parenthesis is like "...()", so dp[i] = dp[i-2] + 2
        if s[i-1] == ')', the parenthesis is like "...))", for the last ')' to be valid,
        the char before the longest of dp[i-1] must be a '(',
        i.e. we need s[i-dp[i-1]-1] = '(', then dp[i] = dp[i-1] + dp[i-dp[i-1]-2] + 2
        
        denote n := len(s)
        Time Complexity : O(n) for one-pass dynamic programming process
        Space Complexity : O(n) dp vector storage 
        """
        if len(s) <= 1: # boundary case
            return 0
        ans = 0
        n = len(s)
        dp = [0] * n
        for i in range(1, n):
            if s[i] == ')': # a possible ending character
                if s[i-1] == '(':
                    if i-2 >= 0:
                        dp[i] = 2 + dp[i-2]
                    else:
                        dp[i] = 2
                elif i-dp[i-1]-1 >= 0 and s[i-dp[i-1]-1] == '(':
                    # be careful about string indexing's s[-1] "wrap around"
                    if i-dp[i-1]-2 >= 0:
                        dp[i] = dp[i-1] + dp[i-dp[i-1]-2] + 2
                    else:
                        dp[i] = dp[i-1] + 2
                ans = max(ans, dp[i])
                    
        return ans
```
-----------

**Solution2: Stack**

```python
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        """
        we could also adopt a stack solution
        we store the index of "all finished matching" index
        along with all "(" index
        when encounter a new ")", we first pop out a '(' from the stack
        if there is nothing in the stack, then it means there is no matching '(', 
        and all the preceding substring is not going to be a valid partheses
        therefore we store the index of this ')' as a header in the stack
        when initialization, we store a -1 in the stack as header
        
        denote n := len(s)
        Time Complexity : O(n) for one-pass 
        Space Complexity : O(n) stack storage
        """
        if len(s) <= 1: # boundary case
            return 0
        n = len(s)
        stack = [-1] # pre-store a header
        ans = 0
        for i in range(n):
            if s[i] == '(':
                stack.append(i)
            elif s[i] == ')':
                stack.pop() # pop out the possible matching '('
                if len(stack) == 0: # fail matching
                    stack.append(i)
                else:
                    ans = max(ans, i - stack[-1])
        return ans
```
-----------

**Solution3:Greedy**

```python
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        """
        The optimal solution is greedy method
        we use two pointer left and right to store 
        the number of '(' and ')' we seen so far respectively
        when the right > left, we discard all the before string and
        reset left and right to 0
        when left = right, we could solve for a valid parthenesis
        However, in this method we might miss '((((())' because 
        right is always smaller than left, but there is indeed valid parthenesis
        Luckily, we can travel one more time from right to left to solve this issue
        
        denote n := len(s)
        Time Complexity : O(n) for left and right two-pass 
        Space Complexity : O(1) 
        """
        if len(s) <= 1: # boundary case
            return 0
        n = len(s)
        ans = 0
        left, right = 0, 0
        for i in range(n):
            # update counting
            if s[i] == '(':
                left += 1
            elif s[i] == ')':
                right += 1
                
            if left == right:
                # a valid parenthesis found
                ans = max(ans, left+right)
            if right > left:
                # reset and discard previous recording
                left, right = 0, 0
        
        left, right = 0, 0
        for i in range(n-1, -1, -1):
            # update counting
            if s[i] == '(':
                left += 1
            elif s[i] == ')':
                right += 1
            
            if left == right:
                # a valid parenthesis found
                ans = max(ans, left+right)
            if left > right:
                # reset and discard previous recording
                left, right = 0, 0
        
        return ans
```
