**249. Group Shifted Strings**

```Tag : hashtable```

**Description:**

We can shift a string by shifting each of its letters to its successive letter.

+ For example, ```"abc"``` can be shifted to be ```"bcd"```.

We can keep shifting the string to form a sequence.

+ For example, we can keep shifting ```"abc"``` to form the sequence: ```"abc" -> "bcd" -> ... -> "xyz"```.

Given an array of strings ```strings```, group all ```strings[i]``` that belong to the same shifting sequence. You may return the answer in **any order**.

**Example1:**

		Input: strings = ["abc","bcd","acef","xyz","az","ba","a","z"]
		Output: [["acef"],["a","z"],["abc","bcd","xyz"],["az","ba"]]

**Example2:**

		Input: strings = ["a"]
		Output: [["a"]]

-----------

```python
class Solution:
    def groupStrings(self, strings: List[str]) -> List[List[str]]:
        """
        By observations, two strings belong to the same shifting sequence if:
        1) they are of same length
        2) the character order difference between successive characters are the same
        for example, 'abc' we can find 'b'-'a' = 1, 'c'-'a' = 1,
        so essentially we can represent 'abc' as (length=3, difference=(1, 1))
        we group together strings that share the same above nested Tuples
        
        denote n := total sum of characters in str in List 'strings'
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        from collections import defaultdict
        
        def generate_tuple(string: str) -> Tuple[int, Tuple]:
            """Helper function: generate the representing tuple for a string"""
            length = len(string)
            difference = []
            for i in range(length-1):
                # order difference between successive characters
                # be careful about 'warp around' character, i.e 'z' to 'a'
                differ = ord(string[i+1]) - ord(string[i])
                if ord(string[i+1]) - ord(string[i]) < 0:
                    differ += 26
                difference.append(differ)
            return (length, tuple(difference))
        
        my_hashtable = defaultdict(list)
        for string in strings:
            my_hashtable[generate_tuple(string)].append(string)
        return my_hashtable.values()
```