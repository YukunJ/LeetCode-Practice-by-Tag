**301. Remove Invalid Parentheses**

```Tag : dfs/backtracking```

**Description:**

Given a string ```s``` that contains parentheses and letters, remove the minimum number of invalid parentheses to make the input string valid.

Return *all the possible results*. You may return the answer in **any order**.

**Example1:**

		Input: s = "()())()"
		Output: ["(())()","()()()"]

**Example2:**

		Input: s = "(a)())()"
		Output: ["(a())()","(a)()()"]

**Example3:**

		Input: s = ")("
		Output: [""]

**Hints:**

+ We can use recursion to try out all possibilities for the given expression. For each of the brackets, we have 2 options: (1) We keep the bracket and add it to the expression that we are building on the fly during recursion. OR, (2) we can discard the bracket and move on.

+ The one thing all these valid expressions have in common is that they will all be of the same length i.e. as compared to the original expression, all of these expressions will have the same number of characters removed. Can we somehow find the number of misplaced parentheses and use it in our solution?

-----------

```python
class Solution:
    def removeInvalidParentheses(self, s: str) -> List[str]:
        """
        This is a complicated DFS backtracking problem
        In order to optimize, we need to realize that
        for a minimum number of invalid parentheses removal to be an answer,
        the number of '(' and ')' to be removed are fixed number across all answers
        Then we could proceed to check only removals with the correct left and right removal counts
        
        denote n := len(s)
        Time Complexity : O(2^n) in worst case, all parentheses are invalid
        Space Complexity : O(n) for recursion stack and temporary storage
        """
        
        left, right = 0, 0
        for c in s:
            if c == '(':
                left += 1
            elif c == ')':
                if left == 0: # no matching '('
                    right = right + 1
                if left > 0: # one pair successfully matched
                    left = left - 1
        res = {} # use a hashtable to avoid duplicate answer
        temp = []
        
        def recur(s: str, index: int, left_count: int, right_count: int, left_remain: int, right_remain: int):
            """Helper function: recursively attempt different removal strategy"""
            if index == len(s):
                # one search finishes
                if left_remain == 0 and right_remain == 0:
                    ans = "".join(temp)
                    res[ans] = True
            else:
                # if there is still quota for removal
                if s[index] == '(' and left_remain > 0:
                    recur(s, index+1, left_count, right_count, left_remain-1, right_remain)
                    
                if s[index] == ')' and right_remain > 0:
                    recur(s, index+1, left_count, right_count, left_remain, right_remain-1)
            
                temp.append(s[index]) # below not skipping s[index]
                
                # consider retain this one
                if s[index] == '(':
                    recur(s, index+1, left_count+1, right_count, left_remain, right_remain)           
                elif s[index] == ')' and left_count > right_count:
                    recur(s, index+1, left_count, right_count+1, left_remain, right_remain)
                # special case, if not parenthesis
                elif s[index] not in ['(', ')']:
                    recur(s, index+1, left_count, right_count, left_remain, right_remain)
                  
                temp.pop() # reset the backtracking
                    
        recur(s, 0, 0, 0, left, right)
        return list(res.keys())
```