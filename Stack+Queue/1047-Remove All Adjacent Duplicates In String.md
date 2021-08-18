**1047. Remove All Adjacent Duplicates In String**

```Tag : Stack```

**Description:**

You are given a string ```s``` consisting of lowercase English letters. A **duplicate removal** consists of choosing two **adjacent** and **equal** letters and removing them.

We repeatedly make **duplicate removals** on ```s``` until we no longer can.

Return the final string after all such duplicate removals have been made. It can be proven that the answer is **unique**.


**Example1:**

		Input: s = "abbaca"
		Output: "ca"
		Explanation: 
		For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".

**Example2:**
	
		Input: s = "azxxzy"
		Output: "ay"

-----------

```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        """
        We could just consider if there is already a stable string
        and you add one more character to the end
        if repetition happens, it will recursively collide with previous characters
        we will use a stack to simulate the process
        
        denote n := len(s)
        Time Complexity : O(n) every character in and out stack at most once
        Space Complexity : O(n)
        """           
        stack = []
        for char in s:
            if stack and stack[-1] == char:
                # two characters cancel out
                stack.pop()
            else:
                stack.append(char)
        return "".join(stack)
```