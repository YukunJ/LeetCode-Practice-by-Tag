**166. Fraction to Recurring Decimal**

```Tag: hashtable/math```

**Description:**

Given two integers representing the ```numerator``` and ```denominator``` of a fraction, return the *fraction in string format*.

If the fractional part is repeating, enclose the repeating part in parentheses.

If multiple answers are possible, return **any of them**.

It is **guaranteed** that the length of the answer string is less than ```10^4``` for all the given inputs.

**Hints**:

+ No scary math, just apply elementary math knowledge. Still remember how to perform a long division?

+ Try a long division on 4/9, the repeating part is obvious. Now try 4/333. Do you see a pattern?

+ Notice that once the remainder starts repeating, so does the divided result.

+ Be wary of edge cases! List out as many test cases as you can think of and test your code thoroughly.


**Example1**:

		Input: numerator = 1, denominator = 2
		Output: "0.5"
        
**Example2**:

		Input: numerator = 2, denominator = 1
		Output: "2"
        
**Example3**:

		Input: numerator = 2, denominator = 3
		Output: "0.(6)"

**Example4**:

		Input: numerator = 4, denominator = 333
		Output: "0.(012)"

**Example5**:

		Input: numerator = 1, denominator = 5
		Output: "0.2"

-----------


```python
class Solution:
    def fractionToDecimal(self, numerator: int, denominator: int) -> str: 
        """
        This is a long division problem,
        we spot inifinite repeatition by finding a recursive reminder
        There are some details to be taken care of,
        for example, negative sign, the digit point, the index of parenthesis

        denote n := the answer length of such a division result
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        if numerator == 0:
            return '0'
        res = []
        if (numerator < 0) ^ (denominator < 0):
            # XOR operation, if one of them is negative
            res.append('-')
        numerator, denominator = abs(numerator), abs(denominator)
        div, remain = divmod(numerator, denominator)
        res.append(str(div))
        if remain == 0: # integer division, no fraction, no need for digit decimal point
            return "".join(res)
        else:
            res.append(".") # append the decimal point
        hashmap = dict()
        while remain != 0:
            if remain in hashmap:
                # the next division result would be repeatitve
                # stop here and add parenthesis ()
                res.insert(hashmap[remain], '(')
                res.append(')')
                break
            else:
                hashmap[remain] = len(res)
                remain *= 10
                div, remain = divmod(remain, denominator)
                res.append(str(div))
                
        return "".join(res)
```
