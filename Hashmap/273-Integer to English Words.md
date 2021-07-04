**273. Integer to English Words**

```Tag: math/recursion/hashmap```

**Description:**

Convert a non-negative integer ```num``` to its English words representation. 0 <= ```num``` <= 2^31 - 1

**Example1**:

        Input: num = 123
        Output: "One Hundred Twenty Three"

**Example2**:

        Input: num = 12345
        Output: "Twelve Thousand Three Hundred Forty Five"
        
**Example3**:

        Input: num = 1234567
        Output: "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"

**Example4**:

        Input: num = 1234567891
        Output: "One Billion Two Hundred Thirty Four Million Five Hundred Sixty Seven Thousand Eight Hundred Ninety One"

**Hints**:

+ Did you see a pattern in dividing the number into chunk of words? For example, 123 and 123000.

+ Group the number by thousands (3 digits). You can write a helper function that takes a number less than 1000 and convert just that chunk to words.

+ There are many edge cases. What are some good test cases? Does your code work with input such as 0? Or 1000010? (middle chunk is zero and should not be printed out)
-----------

```python
class Solution:
    def numberToWords(self, num):
        """
        This is a labor-intensive recursive problem
        we find we could group the number into every three digits, 
            in unit of "one", "thousand", "million", "billion"
        and to represent the 3 digit number, it splits into 0~9, 10~20, 20~99
            where extra work needs to be taken care of
        
        denote n := digit len of input num := log(num)
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        
        def one_digit(num: int) -> int:
            """Helper function: deal with single digit number"""
            map = {
                    1: 'One',
                    2: 'Two',
                    3: 'Three',
                    4: 'Four',
                    5: 'Five',
                    6: 'Six',
                    7: 'Seven',
                    8: 'Eight',
                    9: 'Nine'
                    }
            return map.get(num)
        
        def ten_to_twenty(num: int) -> int:
            """Helper function: deal with num from 10 ~ 20"""
            map = {
                    10: 'Ten',
                    11: 'Eleven',
                    12: 'Twelve',
                    13: 'Thirteen',
                    14: 'Fourteen',
                    15: 'Fifteen',
                    16: 'Sixteen',
                    17: 'Seventeen',
                    18: 'Eighteen',
                    19: 'Nineteen'
                }
            return map.get(num)
        
        def ten(num):
            """Helper function: deal with multiple of 10 num"""
            map = {
                    2: 'Twenty',
                    3: 'Thirty',
                    4: 'Forty',
                    5: 'Fifty',
                    6: 'Sixty',
                    7: 'Seventy',
                    8: 'Eighty',
                    9: 'Ninety'
                }
            return map.get(num)
        
        def two_digit(num):
            """Helper function: deal with two digits number"""
            if not num:
                return ''
            elif num < 10:
                return one_digit(num)
            elif num < 20:
                return ten_to_twenty(num)
            else:
                ten_count, remain = divmod(num, 10)
                return ten(ten_count) + ' ' + one_digit(remain) if remain else ten(ten_count)
            
        def three_digit(num):
            """Helper function: deal with three digits number"""
            hundred, remain = divmod(num, 100)
            if hundred and remain:
                return one_digit(hundred) + ' Hundred ' + two_digit(remain)
            elif not hundred and remain:
                return two_digit(remain)
            elif hundred and not remain:
                return one_digit(hundred) + ' Hundred'
            
        billion, remain = divmod(num, 10 ** 9)
        million, remain = divmod(remain, 10 ** 6)
        thousand, remain = divmod(remain, 10 ** 3)
        result = ''
        
        if not num: # extra attention for boundary case
            return 'Zero'
        # recursively add 3 digits and its units
        if billion:
            result += three_digit(billion)  + ' Billion'
        if million:
            result += ' ' if result else ''
            result += three_digit(million) + ' Million'
        if thousand:
            result += ' ' if result else ''
            result += three_digit(thousand) + ' Thousand'
        if remain:
            result += ' ' if result else ''
            result += three_digit(remain)
            
        return result
```
