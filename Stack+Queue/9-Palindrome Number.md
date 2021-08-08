**9. Palindrome Number**

```Tag: Math/Doubly Queue```

**Description:**

Given an integer ```x```, return ```true``` if ```x``` is palindrome integer.

An integer is a **palindrome** when it reads the same backward as forward. For example, ```121``` is **palindrome** while ```123``` is not.

**Example1**:

        Input: x = 121
        Output: true

**Example2**:

        Input: x = -121
        Output: false
        Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
        
**Example3**:

        Input: x = 10
        Output: false
        Explanation: Reads 01 from right to left. Therefore it is not a palindrome.

**Example4**:

        Input: x = -101
        Output: false
        
**Follow up**: Could you solve it without converting the integer to a string?

-----------

**Solution1: Doubly Queue**

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        """
        We use a sliding window approach, recursively compare the first and last digit of this number 'x'
        if they agree, we rip of the first and last digit and the process continue
        until there are less than (or equal to) one digits left
        we use a doubly linked list to first store every digits of the number 'x', and do sliding window comparison 
        
        denote n := length of x := log(x)
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        from collections import deque
        if x < 0: # boundary case , if there is a minus sign
            return False
        double_q = deque()
        while x > 0:
            x, remain = divmod(x, 10)
            double_q.append(remain)
        while len(double_q) > 1:
            first, last = double_q.popleft(), double_q.pop()
            if first != last:
                # fail to match
                return False
        return True
```

-----------

**Solution2: Math**

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        """
        We could use a math solution to retrieve the reversed of later half of the number
        and compare it with the first half of the number
        
        denote n := length of x := log(x)
        Time Complexity: O(n)
        Space Complexity : O(1)        
        """
        if x < 0 or (x % 10 == 0 and x != 0): # boundary case
            return False
        
        revertHalf = 0
        while x > revertHalf:
            revertHalf = 10 * revertHalf + x % 10
            x //= 10
            
        # depending on even number of digits or odd number of digits
        return revertHalf == x or revertHalf // 10 == x
```
