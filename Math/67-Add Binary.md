**67. Add Binary**

```Tag : Math/Bit Simulation/Two Pointers```

**Description:**

Given two binary strings ```a``` and ```b```, return their sum as a *binary string*.

**Example1:**

		Input: a = "11", b = "1"
		Output: "100"

**Example2:**

		Input: a = "1010", b = "1011"
		Output: "10101"

-----------

```python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        """
        We proceed as normal binary addition
        starting with the lowest bit and add upward
        until carrier is 0 and two string has all been enumerated
        
        denote n := len(a), m := len(b)
        Time Complexity : O(n+m)
        Space Complexity : O(n+m)
        """
        carrier = 0
        res = []
        ptr1, ptr2 = len(a)-1, len(b)-1 # two pointers sliding from end
        while ptr1 >= 0 or ptr2 >= 0 or carrier > 0:
            curr = carrier
            if ptr1 >= 0:
                if a[ptr1] == '1':
                    curr += 1
                ptr1 -= 1
            if ptr2 >= 0:
                if b[ptr2] == '1':
                    curr += 1
                ptr2 -= 1
            carrier, curr = divmod(curr, 2)
            res.append(str(curr))
        return "".join(res[::-1])
```