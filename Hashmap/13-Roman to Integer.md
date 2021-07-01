**13. Roman to Integer**

```Tag: hashmap/math```

**Description:**

Roman numerals are represented by seven different symbols: ```I, V, X, L, C, D and M```.

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

+ ```I``` can be placed before ```V``` (```5```) and ```X``` (```10```) to make ```4``` and ```9```. 

+ ```X``` can be placed before ```L``` (```50```) and ```C``` (```100```) to make ```40``` and ```90```.

+ ```C``` can be placed before ```D``` (```500```) and ```M``` (```1000```) to make ```400``` and ```900```.

Given a roman numeral, convert it to an integer.

**Example1**:

        Input: s = "III"
        Output: 3

**Example2**:

        Input: s = "IV"
        Output: 4
        
**Example3**:

        Input: s = "IX"
        Output: 9

**Example4**:

        Input: s = "LVIII"
        Output: 58
        Explanation: L = 50, V= 5, III = 3.

**Example5**:

        Input: s = "MCMXCIV"
        Output: 1994
        Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.

-----------

```python
class Solution:
    def romanToInt(self, s: str) -> int:
        """
        Usually in roman number representation, larger number appears in left of smaller number
        However, sometimes for example 'IV' represent a 4, with I appears left of V,
        In this case we can subtract the value of 'I' instead of adding it
        
        denote n := len(s)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        # hand-code all the possible mappings from Roman to Int
        hashmap = {'I' : 1,
                   'V' : 5,
                   'X' : 10,
                   'L' : 50,
                   'C' : 100,
                   'D' : 500,
                   'M' : 1000}
        ans = 0
        n = len(s)
        for i in range(n):
            if i < n-1 and hashmap[s[i]] < hashmap[s[i+1]]:
                ans -= hashmap[s[i]]
            else:
                ans += hashmap[s[i]]
        return ans
```
