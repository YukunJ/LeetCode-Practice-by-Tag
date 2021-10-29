**29. Divide Two Integers**

```Tag : binary search/bit operation```

**Description:**

Given two integers ```dividend``` and ```divisor```, divide two integers without using multiplication, division, and mod operator.

Return the quotient after dividing ```dividend``` by ```divisor```.

The integer division should truncate toward zero, which means losing its fractional part. For example, ```truncate(8.345)``` = ```8``` and ```truncate(-2.7335) = -2```.

**Note:** Assume we are dealing with an environment that could only store integers within the **32-bit** signed integer range: ```[−2^31, 2^31 − 1]```. For this problem, assume that your function **returns** ```2^31 − 1``` when the **division result overflows**.



**Example1**:

		Input: dividend = 10, divisor = 3
		Output: 3
		Explanation: 10/3 = truncate(3.33333..) = 3.
 
**Example2**:
 
		Input: dividend = 7, divisor = -3
		Output: -2
		Explanation: 7/-3 = truncate(-2.33333..) = -2.

**Example3**:

		Input: dividend = 0, divisor = 1
		Output: 0

**Example4**:

		Input: dividend = 1, divisor = 1
		Output: 1

-----------

**Solution1: Bit Expansion**

```python
class Solution:
    def divide(self, dividend: int, divisor: int) -> int:
        """
        This is a bit-operation math problem
        when we do x / y integer division, 
        we want to write it as x = a * y + remainder, where we ignore remainder < y
        we could express the a in its binary expansion
        
        denote n := dividend, the largest answer is such that |divisor|= 1, we are essentially bit-expansion n
        for a digit at i-th bit, we need i-th step to reach it, in worst case every bit is set up, we need
        1 + 2 + 3 + ... + log(n) ~= log(n!) ~= nlog(n)
        Time Complexity : O(nlogn)
        Space Complexity : O(1)
        """
        if dividend == 0: # boundary case
            return 0
        
        INT_MIN, INT_MAX = - 2 ** 31, 2 ** 31 - 1
        sign = 1
        if (dividend > 0 and divisor < 0) or (dividend < 0 and divisor > 0):
            sign = -1
        dividend, divisor = abs(dividend), abs(divisor) # take the absolute value
        res = 0
        while dividend >= divisor: # not yet remainder
            curr = divisor
            multiple = 1
            while 2 * curr <= dividend:
                curr *= 2
                multiple *= 2
            res += multiple
            dividend -= curr
        
        return max(INT_MIN, min(INT_MAX, sign * res))
```

-----------

**Solution2: Binary Search + Fast Bit Multiplication**

```python
class Solution:
    def divide(self, dividend: int, divisor: int) -> int:
        """
        relying on the fact that by integer division x / y answer lies in [0, x]
        we could do a binary search on the interval [0, x] for answer
        when we do mid * y, we use a fast bit-operation multiplication to facilitate it

        denote n := dividend 
        we do a binary search on [0, n] takes O(logn)
        each time we do a fast bit multiplication, in worst case also takes O(logn)
        Time Complexity : O(logn * logn)
        Space Complexity : O(1)
        """
        def fast_mul(x: int, k: int) -> int:
            """Helper function: do fast multiplication by bit-operation on k"""
            ans = 0
            while k > 0:
                if (k & 1) == 1:
                    ans += x
                x <<= 1
                k >>= 1
            return ans

        if dividend == 0 or abs(dividend) < abs(divisor): # boundary case
            return 0
        
        INT_MIN, INT_MAX = - 2 ** 31, 2 ** 31 - 1
        sign = 1
        if (dividend > 0 and divisor < 0) or (dividend < 0 and divisor > 0):
            sign = -1
        dividend, divisor = abs(dividend), abs(divisor) # take the absolute value
        res = 0
        left, right = 0, dividend
        while left <= right:
            mid = left + (right-left) // 2
            if fast_mul(divisor, mid) <= dividend:
		# seek bigger answer that's smaller than dividend
                res = mid
                left = mid + 1
            else:
                right = mid - 1
        
        return max(INT_MIN, min(INT_MAX, sign * res))
```

-----------

**Solution3: Basis Representation**

```python
class Solution:
    def divide(self, dividend: int, divisor: int) -> int:
        """
        We could also trade space for time
        we find a power of 2 representation of dividend in base of divisor
        and store that in an array to avoid repeatitive computation
        
        denote n := dividend
        Time Complexity : O(logn)
        Space Complexity : O(logn)
        """
        # Initialization and boundary case consideration
        if dividend == 0 or divisor == 0 or abs(dividend) < abs(divisor):
            return 0
        sign = 1
        if (dividend < 0 and divisor > 0) or (dividend > 0 and divisor < 0):
            sign = -1
        dividend = abs(dividend)
        divisor = abs(divisor)
        MIN = - 2 ** 31
        MAX = 2 ** 31 -1
        
        # construct the power of 2 array in base of divisor
        powersOfTwo = []
        doubles = []
        powerOfTwo = 1
        while dividend >= divisor:
            doubles.append(divisor)
            powersOfTwo.append(powerOfTwo)
            divisor *= 2
            powerOfTwo *= 2
        # search in dividend to find a representation of it in base of divisor
        res = 0
        for i in reversed(range(len(doubles))):
            if dividend >= doubles[i]:
                res += powersOfTwo[i]
                dividend -= doubles[i]
        return max(MIN, min(MAX, sign * res))
```
