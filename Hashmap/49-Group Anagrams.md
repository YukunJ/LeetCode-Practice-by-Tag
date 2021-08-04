**49. Group Anagrams**

```Tag: Hashmap/Sorting/String```

**Description:**

Given an array of strings ```strs```, group the **anagrams** together. You can return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example1:**

		Input: strs = ["eat","tea","tan","ate","nat","bat"]
		Output: [["bat"],["nat","tan"],["ate","eat","tea"]]


**Example2:**

		Input: strs = [""]
		Output: [[""]]

**Example3:**

		Input: strs = ["a"]
		Output: [["a"]]

-----------

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        """
        Based on the definition of Anagrams, 
        two words should be grouped together if they have the same character frequency count
        However, since python dictionary takes only immutable object as key
        we need to first count the frequency and convert it into a immutable tuple type to use as a key

        denote n := len('strs'), s := max length of string in the list 'strs', 
               t := size of frequency table, here t=26 lowercasr letters
        Time Complexity : O(n*(s+t))
        Space Complexity : O(n*t) each str generate a frequency tuple
        """
        from collections import defaultdict
        def count_freq(word: str) -> Tuple:
            """Helper function: count frequency of char in a word"""
            offset = ord('a') # the ASCII order of first lowercase letter
            freq = [0] * 26
            for c in word:
                freq[ord(c)-offset] += 1
            return tuple(freq)
        
        myhash = defaultdict(list)
        for word in strs:
            freq_tuple = count_freq(word)
            myhash[freq_tuple].append(word)
        return list(myhash.values())
```
