**7. Reverse Integer**

```Tag: Stack/Math```

**Description:**

Given a signed 32-bit integer ```x```, return ```x``` with its digits reversed. If reversing ```x``` causes the value to go outside the signed 32-bit integer range ```[-2^31, 2^31 - 1]```, then return ```0```.

**Assume the environment does not allow you to store 64-bit integers (signed or unsigned)**.

**Example1**:

        Input: x = 123
        Output: 321

**Example2**:

        Input: x = -123
        Output: -321
        
**Example3**:

        Input: x = 120
        Output: 21

**Example4**:

        Input: x = 0
        Output: 0  

-----------

```python
class Solution:
    def reverse(self, x: int) -> int:
        """
        We don't explicitly use a stack
        but the algorithm iteratively take mod and power, 
        it is eseential implicit stack operation
        be careful we need to check the range [-2147483648, 2147483647] before overflow/underflow
        Time Complexity : O(log_x) where log_x is essential the number of digits of x
        Space Complexity : O(1)
        """

        # the sign of this number
        sign = 1
        if x < 0:
            sign = -1
            x = abs(x)
        ans = 0
        # 2 ^ 31 = 2147483648, hard-code this number
        MAX = 2147483647 if sign == 1 else 2147483648
        while x > 0:
            if (ans > MAX // 10) or (ans == MAX // 10 and ((sign == 1 and x % 10 > 7) or (sign == -1 and x % 10 > 8))):
                return 0
            ans = 10 * ans + x % 10
            x = x // 10
            
        return sign*ans
```
