**415. Add Strings**

```Tag : two pointer/math/string```

**Description:**

Given two non-negative integers, ```num1``` and ```num2``` represented as string, return the **sum** of ```num1``` and ```num2``` as a string.

You must solve the problem without using any built-in library for handling large integers (such as ```BigInteger```). You must also not convert the inputs to integers directly.

**Example1:**

		Input: num1 = "11", num2 = "123"
		Output: "134"

**Example2:**

		Input: num1 = "456", num2 = "77"
		Output: "533"

**Example3:**

		Input: num1 = "0", num2 = "0"
		Output: "0"

-----------

```python
class Solution:
    def addStrings(self, num1: str, num2: str) -> str:
        """
        We start adding from the backend of two strings
        and keep the carrier all the way
        
        denote m := len(num1), n := len(num2)
        Time Complexity : O(max(m, n))
        Space Complexity : O(max(m, n))
        """
        res = []
        m, n = len(num1), len(num2)
        ptr1, ptr2 = m-1, n-1
        carrier = 0
        
        while carrier > 0 or ptr1 >= 0 or ptr2 >= 0:
            # as long as one of the three conditions is valid
            value = carrier
            if ptr1 >= 0:
                value += int(num1[ptr1])
                ptr1 -= 1
            if ptr2 >= 0:
                value += int(num2[ptr2])
                ptr2 -= 1
            carrier, value = divmod(value, 10) # module 10
            res.append(str(value)) # add current digit
        
        res.reverse() # reverse the stack storage
        return "".join(res)
```