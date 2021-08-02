**38. Count and Say**

```Tag : recursion/string```

**Description:**

The **count-and-say** sequence is a sequence of digit strings defined by the recursive formula:

+ ```countAndSay(1) = "1"```

+ ```countAndSay(n)``` is the way you would "say" the digit string from ```countAndSay(n-1)```, which is then converted into a different digit string.

To determine how you "say" a digit string, split it into the **minimal** number of groups so that each group is a contiguous section all of the **same character**. Then for each group, say the number of characters, then say the character. To convert the saying into a digit string, replace the counts with a number and concatenate every saying.

For example, the saying and conversion for digit string ```"3322251"```:

![avatar](Fig/38.jpeg)

Given a positive integer ```n```, return the ```nth``` term of the **count-and-say** sequence.

**Example1:**

		Input: n = 1
		Output: "1"
		Explanation: This is the base case.

**Example2:**

		Input: n = 4
		Output: "1211"
		Explanation:
		countAndSay(1) = "1"
		countAndSay(2) = say "1" = one 1 = "11"
		countAndSay(3) = say "11" = two 1's = "21"
		countAndSay(4) = say "21" = one 2 + one 1 = "12" + "11" = "1211"

**Hints:**

+ To generate the ```n-th``` term, just count and say the ```n-1-th``` term.

-----------

```python
class Solution:
    def countAndSay(self, n: int) -> str:
        """
        This problem is a little bit awkward to understand
        after turly understanding, it's a basic recursion and string problem
        we call recursion on (n-1)-th and count the contiguous character counts
        and "say" it in a digit-way
        
        Time Complexity : O(n^2)
        Space Complexity : O(n) recursion stack height 
        """
        if n == 1:
            return "1"
        else:
            pre = self.countAndSay(n-1) # retrieve the "digit" we are going to say
            res = ""
            curr_count = 0
            for i in range(len(pre)):
                if i == 0:
                    curr_count += 1
                elif i > 0 and pre[i] == pre[i-1]: # contiguous group
                    curr_count += 1
                else:
                    # a different digit encounter, update it
                    res += str(curr_count) + pre[i-1]
                    curr_count = 1
            if curr_count > 0: # clear the last group remaining
                res += str(curr_count) + pre[len(pre)-1]
            return res
```