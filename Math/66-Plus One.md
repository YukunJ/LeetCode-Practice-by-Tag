**66. Plus One**

```Tag : math/array```

**Description:**

Given a **non-empty** array of decimal digits representing a non-negative integer, increment one to the integer.

The digits are stored such that the most significant digit is at the head of the list, and each element in the array contains a single digit.

You may assume the integer does not contain any leading zero, except the number 0 itself.

**Example1:**

		Input: digits = [1,2,3]
		Output: [1,2,4]
		Explanation: The array represents the integer 123.

**Example2:**

		Input: digits = [4,3,2,1]
		Output: [4,3,2,2]
		Explanation: The array represents the integer 4321.

**Example3:**

		Input: digits = [0]
		Output: [1]

-----------

```python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        """
        textbook standard sliding pointer addition
        
        denote n := len(digits)
        Time Complexity : O(n)
        Space Complexity : O(n) in worst case allocate new space storage
        """
        ptr = len(digits) - 1
        carrier = 1
        while ptr >= 0:
            digits[ptr] += carrier
            carrier, digits[ptr] = divmod(digits[ptr], 10)
            if carrier == 0:
                # early stopping, no more carrier forward
                return digits
            ptr -= 1
        
        if carrier > 0: # the boundary case: carrier all the way up 
            digits = [carrier] + digits
        
        return digits
```
