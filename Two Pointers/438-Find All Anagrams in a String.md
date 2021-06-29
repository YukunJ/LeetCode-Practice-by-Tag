**438. Find All Anagrams in a String**

```Tag: sliding window/hashtable```

**Description:**

Given two strings ```s``` and ```p```, return an array of all the start indices of ```p```'s anagrams in ```s```. You may return the answer in any order.



**Example1**:

		Input: s = "cbaebabacd", p = "abc"
		Output: [0,6]
		Explanation:
		The substring with start index = 0 is "cba", which is an anagram of "abc".
		The substring with start index = 6 is "bac", which is an anagram of "abc".

**Example2**:

    	Input: s = "abab", p = "ab"
		Output: [0,1,2]
		Explanation:
		The substring with start index = 0 is "ab", which is an anagram of "ab".
		The substring with start index = 1 is "ba", which is an anagram of "ab".
		The substring with start index = 2 is "ab", which is an anagram of "ab".


-----------

**Solution1: Free Sliding Window**

```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        """
        This is a sliding window question, we maintain two pointers left & right
        we first use a hashtable hash_fix to record the frequency of elements in p,
            and also maintain another hashtable hash_slide for current sliding window
        while we fix left pointer, we try to expand the window by moving right
        we define a way to compare two hashtables: there are 3 cases
        1) for all elements e, hash_fix[e] = hash_slide[e], we find an anagram, record the left and move to right by 1 position
        2) any element e cause hash_slide[e] > hash_fix[e] (or e not in hash_fix at all), we fail in this left, move to right by 1 position
        3) for some element hash_slide[e] < hash_fix[e] and other element not exceeding, we can expand the window by moving the right pointer to right 

        denote n := len(s), m := len(p)
        Time Complexity : O(n+m) construct the hash_fix and pointers iterate through string s
        Space Complexity : O(1) two hashtables are finite ASCII characters
        """
        from collections import defaultdict
        def compare(hash_slide: defaultdict(int), hash_fix: defaultdict(int)) -> int:
            """Helper function: compare two hashtable, 3 types of return code"""
            """Since ASCII characters are finite, this compare() operation is O(1)"""
            # first, key range check
            for e, occu in hash_slide.items():
                if occu > 0 and e not in hash_fix:
                    return 2 # case (2) bigger
            smaller = False
            for e, occu in hash_fix.items():
                if hash_slide[e] > occu: # case (2) bigger
                    return 2
                elif hash_slide[e] < occu:
                    smaller = True
            if not smaller: # case (1) exact equal
                return 1
            else:
                return 3 # case (3) smaller

        hash_slide, hash_fix = defaultdict(int), defaultdict(int)
        res = []
        # record the frequency in p, the base Anagrams
        for c in p:
            hash_fix[c] += 1
        right = -1
        for left in range(len(s)):
            if left != 0: 
                # move right, abandon s[left-1]
                hash_slide[s[left-1]] -= 1
            while right < len(s)-1:
                right += 1
                hash_slide[s[right]] += 1
                compare_code = compare(hash_slide, hash_fix)
                if compare_code == 1:
                    # exact match, record the ans and ready to move left pointer
                    res.append(left)
                    break
                elif compare_code == 2:
                    # bigger, fail, move the left pointer
                    break 
                # else: smaller, expand the window
                    
        return res
```
-----------

**Solution2: Fix Size Sliding Window**

```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        """
        Can also fix 26-characters counter
        and slide with a fix-size window of len(p)
        
        denote n := len(s), m := len(p)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        if len(p) > len(s): # boundary checking
            return []
        
        count_s, count_p, offset = [0] * 26, [0] * 26, ord('a')
        # the first size=len(p) window
        for i in range(len(p)):
            count_s[ord(s[i])-offset] += 1
            count_p[ord(p[i])-offset] += 1
            
        res = []
        left = 0
        for right in range(len(p), len(s)):
            if count_s == count_p:
                res.append(left)
            # abandon the left pointer
            count_s[ord(s[left])-offset] -= 1
            # increment the left pointer
            left += 1
            # slide to right by 1 position
            count_s[ord(s[right])-offset] += 1
            
        # the last window not checked yet
        if count_s == count_p:
            res.append(left)
        
        return res
```
