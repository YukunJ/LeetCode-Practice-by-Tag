**50. Pow(x,n)**

```Tag : math/recursion```

**Description:**

Implement ```pow(x, n)```, which calculates ```x``` raised to the power ```n``` (i.e., ```x^n```).



**Example1**:

		Input: x = 2.00000, n = 10
		Output: 1024.00000

 
**Example2**:
 
		Input: x = 2.10000, n = 3
		Output: 9.26100


**Example3**:

		Input: x = 2.00000, n = -2
		Output: 0.25000
		Explanation: 2-2 = 1/22 = 1/4 = 0.25


-----------

**Solution1: Recursion**

```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        """
        This is a recursion problem, with one optimization tips
        we could use the "fast-powering" method to compute x^n
        using O(logn) time complexity instead of O(n)
        The trick is that, 
        if n is even, x^n = x^{n//2} * x^{n//2}
        if n is odd, x^n = x * x^{n//2} * x^{n//2}
        
        Time Complexity : O(logn)
        Space Complexity : O(logn) recursion stack height
        """
        
        def fast_power(x: float, n: int) -> float:
            """Helper function, O(logn) time compute x^n"""
            if n == 0: # boundary case
                return 1
            elif n % 2 == 0:
                x_half = fast_power(x, n // 2) # compute it only once
                return x_half * x_half
            else:
                x_half = fast_power(x, n // 2)
                return x * x_half * x_half
            
        if x == 0: # boundary case
            return 0
        
        if n < 0: # revert power to be positive
            x, n = 1/x, -n
        return fast_power(x, n)
```

-----------

**Solution2: Iterative**

```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        """
        A better iterative way to do this problem is to
        expanding the n into its binary expression
        for example, x^5, 5 in binary expression is 0b00000101
        we may write it as x^5 = x^4 * x^1
        
        Time Complexity : O(logn)
        Space Complexity : O(1) 
        """
        
        def fast_power(x: float, n: int) -> float:
            """Helper function, O(logn) time compute x^n and only O(1) space complexity"""
            if n == 0: # boundary case
                return 1.0
            res = 1.0
            multiple = x
            while n > 0:
                if n % 2 == 1:
                    res *= multiple
                multiple *= multiple # make x into x^2, x^2 into x^4, etc.
                n //= 2 # shrink the binary expansion of n
            return res
            
            
        if x == 0: # boundary case
            return 0.0
        
        if n < 0: # revert power to be positive
            x, n = 1/x, -n
        return fast_power(x, n)
```
