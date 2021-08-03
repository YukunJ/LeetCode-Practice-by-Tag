**43. Multiply Strings**

```Tag : math/string```

**Description:**

Given two non-negative integers ```num1``` and ```num2``` represented as strings, return the product of ```num1``` and ```num2```, also represented as a string.

**Note**: You must not use any built-in BigInteger library or convert the inputs to integer directly.

**Example1:**

		Input: num1 = "2", num2 = "3"
		Output: "6"

**Example2:**

		Input: num1 = "123", num2 = "456"
		Output: "56088"

**Constraints:**

+ ```num1``` and ```num2``` consist of digits only.

+ Both ```num1``` and ```num2``` do not contain any leading zero, except the number ```0``` itself.

-----------

```python
class Solution:
    def multiply(self, num1: str, num2: str) -> str:
        """
        We simulate this problem in the way how we are taught to do multiplication in primary school
        for example, to calculate 123X456=56088, we consider the following:
                 123
         X       456
         -----------
                 738
                615
               512
         -----------
               56088
        
        denote n := len(num1), m := len(num2)
        Time Complexity : O(n*m) double loop to iterate each digit
        Space Complexity : O(1)
        """
        result = 0
        multiple_2 = 1 
        for digit2 in num2[::-1]:
            # fix a digit in num2
            # multiply it in total with num1
            digit2 = int(digit2)
            digit_result = 0
            multiple_1 = 1 # the trailing zero for digit in num1
            for digit1 in num1[::-1]:
                digit1 = int(digit1)
                digit_result += digit2 * digit1 * multiple_1
                multiple_1 *= 10
            result += digit_result * multiple_2
            multiple_2 *= 10 # move to next digit in num2, add trailing zero
        
        return str(result)
```