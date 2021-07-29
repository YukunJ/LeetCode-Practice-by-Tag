**30. Substring with Concatenation of All Words**

```Tag : hashtable/sliding window```

**Description:**

You are given a string ```s``` and an array of strings ```words``` of the **same length**. Return all starting indices of substring(s) in ```s``` that is a concatenation of each word in ```words``` **exactly once**, in **any order**, and **without any intervening characters**.

You can return the answer in **any order**.



**Example1**:

		Input: s = "barfoothefoobarman", words = ["foo","bar"]
		Output: [0,9]
		Explanation: Substrings starting at index 0 and 9 are "barfoo" and "foobar" respectively.
The output order does not matter, returning [9,0] is fine too.
 
**Example2**:
 
		Input: s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]
		Output: []


**Example3**:

		Input: s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]
		Output: [6,9,12]

-----------

**Solution1: Hashtable**

```python
class Solution:
    def findSubstring(self, s: str, words: List[str]) -> List[int]:
        """
        the first thought is that, since we know the word are fixed-length and must each appear exactly once
        we know the length of a successful match substring of be length l' = len(words) * len(words[0])
        we can iterate all possible substring of length l' in s and count if every word in s appears using hashtable

        denote n := len(s), m := len(words), k := len(words[0])
        Time Complexity : O(n*m*k)
                        the 'i' loops O(n-(m*k)) ~= O(n) times
                        inside each 'i' loop, the temp substring sliding takes O(m*k)
                        the total 'j' loop takes O(m*k) to sliding over the whole temp string
                        the hashtable creation and comparison takes O(m) time
                        therefore the total is O(n*m*k)
        Space Complexity : O(m*k) for hashtable storage
        """
        from collections import Counter
        if not s or not words: # boundary case
            return []
        res = []
        n = len(s)
        word_length = len(words[0]) # a single word length
        total_length = word_length * len(words) # total concatenation length
        Counter_words = Counter(words) # hashtable counting word frequency
        for i in range(n+1-total_length):
            temp = s[i:i+total_length]
            temp_lst = []
            for j in range(0, total_length, word_length):
                # add a word
                temp_lst.append(temp[j:j+word_length])
            if Counter(temp_lst) == Counter_words:
                # a successful match
                res.append(i)
        return res
```

-----------

**Solution2: Sliding Window**

```python
class Solution:
    def findSubstring(self, s: str, words: List[str]) -> List[int]:
        """
        we could do a sliding window on string s, with step size = length of one word 
        we need to consider multiple starting point, otherwise might miss some answers

        denote n := len(s), m := len(words), k := len(words[0])
        Time Complexity : O(n*k)
                        there are O(k) starting points we need to search
                        In each start-point's sliding window, left and right pointers goes through the string s at most once(with jump size = k)
                        So in total this is O(n*k), compared with solution 1, we skip the comparison of two hashmaps which takes extra O(m) every time
        Space Complexity : O(m*k) for hashtable storage
        """
        from collections import Counter
        if not s or not words: # boundary case
            return []
        res = []
        n = len(s)
        word_length = len(words[0]) # a single word length
        total_length = word_length * len(words) # total concatenation length
        Counter_words = Counter(words) # hashtable counting word frequency
        # do a multiple start sliding window
        for start in range(0, word_length):
            left, right = start, start
            correct = 0
            Counter_slide = Counter()
            while right + word_length <= n:
                # absorb a new word
                new_word = s[right:right+word_length]
                right += word_length
                Counter_slide[new_word] += 1
                correct += 1
                while left < right and Counter_slide[new_word] > Counter_words[new_word]:
                    # we try to drop some extra words to be "smaller" than word Counter
                    left_word = s[left:left+word_length]
                    left += word_length
                    Counter_slide[left_word] -= 1
                    correct -= 1
                if left == right:
                    # total failure, clear history, start again from current position
                    Counter_slide.clear()
                    correct = 0
                if correct == len(words):
                    # a match found
                    res.append(left)
        return res
```
