**371. Sum of Two Integers**

```Tag : math/bitwise manipulation```

**Description:**

Given two integers ```a``` and ```b```, return the **sum of the two integers** without using the operators ```+``` and ```-```.

**Example1:**

		Input: a = 1, b = 2
		Output: 3

**Example2:**

		Input: a = 2, b = 3
		Output: 5

**Constraints**:

+ ```-1000 <= a, b <= 1000```

-----------

```python
class Solution:
    def getSum(self, a: int, b: int) -> int:
        """
        we mostly use "XOR" operation to solve this problem,
        this is hard bitwise operation,
        for a detailed insturction, see the website below:
        https://leetcode.com/problems/sum-of-two-integers/solution/
        
        Time Complexity : O(1) 32-bit integers
        Space Complexity : O(1)
        """
        x, y = abs(a), abs(b)
        if x < y:
            # make sure that |a| >= |b|
            return self.getSum(b, a)
        # a determines the sign of overall result
        sign = 1 if a > 0 else -1
        
        if a * b >= 0:
            # sum of two positive integers x + y
            while y:
                answer = x ^ y # sum without carrier
                carry = (x & y) << 1
                x, y = answer, carry
        else:
            # difference between two integers x - y, x > y in specific
            while y:
                answer = x ^ y # difference without borrower
                borrow = (~x & y) << 1
                x, y = answer, borrow
        
        return x * sign
```