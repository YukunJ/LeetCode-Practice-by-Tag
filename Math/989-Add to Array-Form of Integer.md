**989. Add to Array-Form of Integer**

```Tag : math/array```

**Description:**

The **array-form** of an integer ```num``` is an array representing its digits in left to right order.

+ For example, for ```num = 1321```, the array form is ```[1,3,2,1]```.

Given ```num```, the **array-form** of an integer, and an integer ```k```, return the **array-form** of the integer ```num + k```.

**Example1:**

		Input: num = [1,2,0,0], k = 34
		Output: [1,2,3,4]
		Explanation: 1200 + 34 = 1234

**Example2:**

		Input: num = [2,7,4], k = 181
		Output: [4,5,5]
		Explanation: 274 + 181 = 455

**Example3:**

		Input: num = [2,1,5], k = 806
		Output: [1,0,2,1]
		Explanation: 215 + 806 = 1021

**Example4:**

		Input: num = [9,9,9,9,9,9,9,9,9,9], k = 1
		Output: [1,0,0,0,0,0,0,0,0,0,0]
		Explanation: 9999999999 + 1 = 10000000000

-----------

```python
class Solution:
    def addToArrayForm(self, num: List[int], k: int) -> List[int]:
        """
        This is textbook style two pointers addition problem
        we do that from backend of the two given objects
        
        denote n := len(num), m := digits of k = log(k)
        Time Complexity : O(max(n, m))
        Space Complexity : O(max(n, m))
        """
        res = []
        n = len(num)
        ptr = n - 1
        carry = 0
        
        while carry > 0 or ptr >= 0 or k > 0:
            # continue as long as any of the three conditions hold
            value = carry
            if ptr >= 0:
                value += num[ptr]
                ptr -= 1
            if k > 0:
                # extract the last digit from k
                k, digit = divmod(k, 10)
                value += digit
            carry, value = divmod(value, 10)
            res.append(value)
        
        res.reverse() # reverse the stack order result
        return res
```