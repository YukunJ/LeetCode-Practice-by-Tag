**342. Power of Four**

```Tag : math/bit manipulation```

**Description:**

Given an integer ```n```, return ```true``` if it is **a power of four**. Otherwise, return ```false```.

An integer ```n``` is a power of four, if there exists an integer ```x``` such that ```n == 4^x```.

**Example1:**

        Input: n = 16
        Output: true

**Example2:**

        Input: n = 5
        Output: false

**Example3:**

        Input: n = 1
        Output: true

**Follow up**: Could you solve it without loops/recursion?

-----------

**Solution1: O(logn)**

```python
class Solution:
    def isPowerOfFour(self, n: int) -> bool:
        """
        In thinking about this question, 
        we can factor it into thinking about binary representation
        if a number is power of Two, then only 1 digit of its binary representation is 1, all others are 0
        Here, we do a similar computation to expand the number n into its 4-inary representation
        
        Time Complexity : O(logn)
        Space Complexity : O(1)
        """
        digit_up = 0
        power_of_four = 1
        powers_of_four = []
        while power_of_four <= n:
            powers_of_four.append(power_of_four)
            power_of_four *= 4
        
        for i in range(len(powers_of_four)-1, -1, -1):
            p = powers_of_four[i]
            factor, n = divmod(n, p)
            digit_up += factor
            
        return digit_up == 1
```

-----------

**Solution2: O(1)**

```python
class Solution:
    def isPowerOfFour(self, n: int) -> bool:
        """
        A more clever trick is
        1) by n > 0 and n & (n-1) == 0 to check if n is a power of 2
        2) bit-and with 0b10101010 to check if this is an even power of 2
        
        Time Complexity : O(1)
        Space Complexity : O(1)
        """
        return n > 0 and n & (n-1) == 0 and n & 0xaaaaaaaa == 0
```
