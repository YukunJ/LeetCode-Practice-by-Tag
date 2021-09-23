**243. Shortest Word Distance**

```Tag : two pointer/string/array```

**Description:**

Given an array of strings ```wordsDict``` and two different strings that already exist in the array ```word1``` and ```word2```, return the *shortest distance between these two words in the list*.

**Example1:**

        Input: wordsDict = ["practice", "makes", "perfect", "coding", "makes"], word1 = "coding", word2 = "practice"
        Output: 3
        
**Example2:**

        Input: wordsDict = ["practice", "makes", "perfect", "coding", "makes"], word1 = "makes", word2 = "coding"
        Output: 1

-----------

```python
class Solution:
    def shortestDistance(self, wordsDict: List[str], word1: str, word2: str) -> int:
        """
        A brute force would take O(n^2) to double nest loop
        one clever one-pass solution is to observe that,
        at each position, a word could at most equal to one of word1 and word2
        if it's word1, we try to match it with last occurrence of word2
        if it's word2, we try to match it with last occurrence of word1
        
        denote n := len(wordsDict), M := len(word1) + len(word2)
        Time Complexity : O(n * M) scan for equalness takes O(M) time
        Space Complexity : O(1)
        """
        last_occur1, last_occur2 = -1, -1
        shortestDist = float('inf')
        
        for i, word in enumerate(wordsDict):
            if word == word1:
                last_occur1 = i
            elif word == word2:
                last_occur2 = i
            
            if last_occur1 != -1 and last_occur2 != -1:
                # both have already occurred
                shortestDist = min(shortestDist, abs(last_occur1-last_occur2))
        
        return shortestDist
```
