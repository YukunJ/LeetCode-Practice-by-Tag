**202. Happy Number**

```Tag: two pointers/hashtable```

**Description:**

Write an algorithm to determine if a number ```n``` is happy.

A **happy number** is a number defined by the following process:

+ Starting with any positive integer, replace the number by the sum of the squares of its digits.

+ Repeat the process until the number equals ```1``` (where it will stay), or it loops endlessly in a cycle which does not include ```1```.

+ Those numbers for which this process ends in ```1``` are **happy**.

Return ```true``` if n is a happy number, and ```false``` if not.

**Example1**:

        Input: n = 19
        Output: true
        Explanation:
        12 + 92 = 82
        82 + 22 = 68
        62 + 82 = 100
        12 + 02 + 02 = 1

**Example2**:

        Input: n = 2
        Output: false
    
-----------

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        """
        This is an implicit linked list question, we are asked to find a cycle if any
        Every ListNode's val the previous Node's val's sum of square of digits
        Therefore, we recall that we could use two pointers - fast & slow to solve
        
        Time Complexity : O(logn)
        Space Complexity : O(1)
        """
        def get_next(num: int) -> int:
            """Helper function: take the sum of square of digits of a number"""
            digit_sum = 0
            while num > 0:
                num, remain = divmod(num, 10)
                digit_sum += remain ** 2
            return digit_sum
        
        slow = n
        fast = get_next(n)
        while fast != 1 and fast != slow:
            slow = get_next(slow)
            fast = get_next(get_next(fast))
            
        return fast == 1 # if end reached or loop encountered
```
