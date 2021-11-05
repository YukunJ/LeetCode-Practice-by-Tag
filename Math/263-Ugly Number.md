**263. Ugly Number**

```Tag : math```

**Description:**

An **ugly number** is a positive integer whose prime factors are limited to ```2```, ```3```, and ```5```.

Given an integer ```n```, return ```true``` if ```n``` is an **ugly number**.

**Example1:**

        Input: n = 6
        Output: true
        Explanation: 6 = 2 × 3

**Example2:**

        Input: n = 8
        Output: true
        Explanation: 8 = 2 × 2 × 2
        
**Example3:**

        Input: n = 14
        Output: false
        Explanation: 14 is not ugly since it includes the prime factor 7.
        
**Example4:**

        Input: n = 1
        Output: true
        Explanation: 1 has no prime factors, therefore all of its prime factors are limited to 2, 3, and 5.

-----------

```python
class Solution:
    def isUgly(self, n: int) -> bool:
        """
        Normal Math division problem
        Time Complexity : O(logn)
        Space Complexity : O(1)
        """
        if n <= 0:
            return False
        if n in [1, 2, 3, 5]:
            return True
        while n > 1 and n % 2 == 0:
            n = n // 2
        while n > 1 and n % 3 == 0:
            n = n // 3
        while n > 1 and n % 5 == 0:
            n = n // 5
        return n == 1
```
