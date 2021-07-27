**12. Integer to Roman**

```Tag : math/string```

**Description:**

Roman numerals are represented by seven different symbols: ```I, V, X, L, C, D``` and ```M```.

	Symbol       Value
	I             1
	V             5
	X             10
	L             50
	C             100
	D             500
	M             1000

For example, ```2``` is written as ```II``` in Roman numeral, just two one's added together. ```12``` is written as ```XII```, which is simply ```X + II```. The number ```27``` is written as ```XXVII```, which is ```XX + V + II```.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not ```IIII```. Instead, the number four is written as ```IV```. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as ```IX```. There are six instances where subtraction is used:

+ ```I``` can be placed before ```V``` (5) and ```X``` (10) to make 4 and 9. 

+ ```X``` can be placed before ```L``` (50) and ```C``` (100) to make 40 and 90. 

+ ```C``` can be placed before ```D``` (500) and ```M``` (1000) to make 400 and 900.

Given an integer, convert it to a roman numeral.

**Example1:**

		Input: num = 3
		Output: "III"

**Example2:**

		Input: num = 4
		Output: "IV"

**Example3:**

		Input: num = 9
		Output: "IX"

**Example4:**

		Input: num = 58
		Output: "LVIII"
		Explanation: L = 50, V = 5, III = 3.

**Example5:**

		Input: num = 1994
		Output: "MCMXCIV"
		Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.


-----------

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        """
        Since there is a unique way conversion between Integer and Roman representation
        we could use a kind of 'greedy' approach
        try to subtract a large number and add a Roman representation
        and continue with the remaining number
        
        due to the range limitation of Roman number, 
        we iterate through a finite mapping, and each representation might occur at most 3 times
        Time Complexity : O(1)
        Space Complexity : O(1)
        """
        mapping = [
            (1000, 'M'),
            (900, 'CM'),
            (500, 'D'),
            (400, 'CD'),
            (100, 'C'),
            (90, 'XC'),
            (50, 'L'),
            (40, 'XL'),
            (10, 'X'),
            (9, 'IX'),
            (5, 'V'),
            (4, 'IV'),
            (1, 'I')
        ]
        
        res = ""
        for value, represent in mapping:
            # greedy, go from higher value to lower value
            while num >= value:
                num -= value
                res += represent
        return res
```
