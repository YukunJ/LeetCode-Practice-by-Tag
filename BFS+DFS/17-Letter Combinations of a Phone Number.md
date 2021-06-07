**17. Letter Combinations of a Phone Number**

```Tag: DFS```

**Description:**

Given a string containing digits from ```2-9``` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![avatar](Fig/17.png)

**Example1**:

        Input: digits = "23"
        Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]

**Example2**:

        Input: digits = ""
        Output: []
        
**Example3**:

        Input: digits = "2"
        Output: ["a","b","c"]

-----------

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        """
        This is a DFS problem
        We construct a mapping to map from digit to the letter it represents

        denote n:=the number of digit that respond to 3 letters
               m:=the number of digit that respond to 4 letters
        Time Complexity : O(3^n * 4^m) enumerate all possibilities
        Space Complexity: O(n+m)
        """
        digit2letter = {
            '2' : ['a', 'b', 'c'],
            '3' : ['d', 'e', 'f'],
            '4' : ['g', 'h', 'i'],
            '5' : ['j', 'k', 'l'],
            '6' : ['m', 'n', 'o'],
            '7' : ['p', 'q', 'r', 's'],
            '8' : ['t', 'u', 'v'],
            '9' : ['w', 'x', 'y', 'z']
        }
        if not digits:
            return []
        def dfs(index: int):
            if index == len(digits):
                combinations.append("".join(combination))
            else:
                curr_digit = digits[index]
                for mapping in digit2letter[curr_digit]:
                    combination.append(mapping)
                    dfs(index+1)
                    combination.pop()
        combination = []
        combinations = []
        dfs(0)
        return combinations
```
