**953. Verifying an Alien Dictionary**

```Tag : hashtable```

**Description:**

In an alien language, surprisingly they also use english lowercase letters, but possibly in a different ```order```. The ```order``` of the alphabet is some permutation of lowercase letters.

Given a sequence of ```words``` written in the alien language, and the ```order``` of the alphabet, return ```true``` if and only if the given ```words``` are sorted lexicographicaly in this alien language.

**Example1:**

		Input: words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"
		Output: true
		Explanation: As 'h' comes before 'l' in this language, then the sequence is sorted.

**Example2:**

		Input: words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"
		Output: false
		Explanation: As 'd' comes after 'l' in this language, then words[0] > words[1], hence the sequence is unsorted.

**Example3:**

		Input: words = ["apple","app"], order = "abcdefghijklmnopqrstuvwxyz"
		Output: false
		Explanation: The first three characters "app" match, and the second string is shorter (in size.) According to lexicographical rules "apple" > "app", because 'l' > '∅', where '∅' is defined as the blank character which is less than any other character (More info).

-----------

```python
class Solution:
    def isAlienSorted(self, words: List[str], order: str) -> bool:
        """
        This is a hashtable question.
        We use a hashmap to store the order of Alien alphabet
        and implement a customizer alien comparison function for alien strings
        
        denote n := len(words), m := max length of word in words
        Time Complexity : O(n * m) in worst case to scan every character in words
        Space Complexity : O(1) 
        """
        # the alien alphabet ordering
        alienOrder = {char: idx for idx, char in enumerate(order)}
        
        # define a custom alien string comparison function
        def compare(s1: str, s2: str, alienOrder: dict) -> bool:
            """Helper function: return true of s1 < s2 in alien order"""
            ptr1, ptr2 = 0, 0
            while ptr1 < len(s1) and ptr2 < len(s2):
                if alienOrder[s1[ptr1]] < alienOrder[s2[ptr2]]:
                    return True
                elif alienOrder[s1[ptr1]] > alienOrder[s2[ptr2]]:
                    return False
                else:
                    # equal at this character level, move to next one
                    ptr1 += 1
                    ptr2 += 1
            return len(s1) <= len(s2) # finish comparison
        
        for i in range(len(words)-1):
            if not compare(words[i], words[i+1], alienOrder):
                return False
        return True
```