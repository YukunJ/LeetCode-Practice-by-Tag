**726. Number of Atoms**

```Tag: Stack/Hashmap```

**Description:**

Given a string ```formula``` representing a chemical formula, return the *count of each atom*.

The atomic element always starts with an uppercase character, then zero or more lowercase letters, representing the name.

One or more digits representing that element's count may follow if the count is greater than ```1```. If the count is ```1```, no digits will follow.

+ For example, "```H2O```" and "```H2O2```" are possible, but "```H1O2```" is impossible.

Two formulas are concatenated together to produce another formula.

+ For example, "```H2O2He3Mg4```" is also a formula.

A formula placed in parentheses, and a count (optionally added) is also a formula.

+ For example, "```(H2O2)```" and "```(H2O2)3```" are formulas.

Return the count of all elements as a string in the following form: the first name (in sorted order), followed by its count (if that count is more than ```1```), followed by the second name (in sorted order), followed by its count (if that count is more than ```1```), and so on.

**Example1**:

        Input: formula = "H2O"
        Output: "H2O"
        Explanation: The count of elements are {'H': 2, 'O': 1}.

**Example2**:

        Input: formula = "Mg(OH)2"
        Output: "H2MgO2"
        Explanation: The count of elements are {'H': 2, 'Mg': 1, 'O': 2}.

        
**Example3**:

        Input: formula = "K4(ON(SO3)2)2"
        Output: "K4N2O14S4"
        Explanation: The count of elements are {'K': 4, 'N': 2, 'O': 14, 'S': 4}.

**Example4**:

        Input: formula = "Be32"
        Output: "Be32"

**Hints**:

+ To parse ```formula[i:]```, when we see a `'('`, we will parse recursively whatever is inside the brackets (up to the correct closing ending bracket ```')'```) and add it to our count, multiplying by the following multiplicity if there is one. Otherwise, we should see an uppercase character: we will parse the rest of the letters to get the name, and add that (plus the multiplicity if there is one.)

-----------

```python
class Solution:
    def countOfAtoms(self, formula: str) -> str:
        """
        This is again a Stack problem consisting of "outer" and "inner" layer
        we want to deal with the atom sequence inside parenthesis '()' and then
        multiply the following multiplicity if exists
        we use a stack to main the structure and a hashmap to store the frequency of current atom sequence
        The different cases we may encounter:
        1) a upper character indicates the start of a new Atom name
        2) a lower character means it is following some upper character to make up a whole atom name
        3) a digit, we continue to scan until get the whole number
        4) a opening '(', we push the current hashmap to the stack and init a new one as counter
        5) a closing ')', we scan for a possible following multiplicity and pop until we get the 
            hashmap we pushed last time when meeting '(', and update the frequency count
        And also, there are some small details that one may take care of
        denote n := len(formula)
        Time Complexity : O(n^2) 
            the hashmap key sorting takes O(n*logn)
            every time we update the hashmap it may take O(n) time and we may update it O(n) times
            consider a follow hashmap stack sequence: [empty, empty, ..., map with n/2 items]
        Space Complexity : O(n) for total hashmap storage
        """
        from collections import defaultdict
        chemical, num = "", 0
        res = defaultdict(int)
        ptr = 0
        stack = [] # the working stack
        while ptr < len(formula):
            if formula[ptr].isupper(): # beginning of an atom
                # clear the previous storage
                res[chemical] += num if num > 0 else 1
                chemical, num = formula[ptr], 0
                ptr += 1
            elif formula[ptr].islower(): # continuation of an atom
                chemical += formula[ptr]
                ptr += 1
            elif formula[ptr].isdigit(): # scan a number
                num = 10 * num + int(formula[ptr])
                ptr += 1
            elif formula[ptr] == '(': # push the hashmap, go deep into nested layer
                res[chemical] += num if num > 0 else 1
                chemical, num = "", 0
                stack.append(res)
                stack.append('(')
                res = defaultdict(int) # init a new freq counter
                ptr += 1
            elif formula[ptr] == ')':
                res[chemical] += num if num > 0 else 1
                # scan a possibly multiplicity if exists
                num = 0
                while ptr+1 < len(formula) and formula[ptr+1].isdigit():
                    num = 10 * num + int(formula[ptr+1])
                    ptr += 1
                multiplicity = num if num > 0 else 1
                while stack[-1] != '(':
                    stack.pop()
                stack.pop()
                # get the old hashmap and update it
                old_res = stack.pop() 
                for c, freq in res.items():
                    old_res[c] += freq * multiplicity
                res = old_res
                chemical, num = "", 0
                ptr += 1
                
        if formula[-1] != ')':
            # clear the remaining uncached result
            res[chemical] += num if num > 0 else 1
        
        # construct the final answer
        result_str = ""
        for key in sorted(res): # sort the atom alphabetically
            if key != "" and res[key] > 0:
                result_str += key + str(res[key]) if res[key] > 1 else key
        return result_str   
```
