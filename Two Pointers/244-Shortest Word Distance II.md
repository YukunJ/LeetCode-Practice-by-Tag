**244. Shortest Word Distance II**

```Tag : two pointers/hashmap/string/array/design```

**Description:**

Design a data structure that will be initialized with a string array, and then it should answer queries of the shortest distance between two different strings from the array.

Implement the ```WordDistance``` class:

+ ```WordDistance(String[] wordsDict)``` initializes the object with the strings array ```wordsDict```.

+ ```int shortest(String word1, String word2)``` returns the shortest distance between ```word1``` and ```word2``` in the array ```wordsDict```.

**Example1:**

        Input
        ["WordDistance", "shortest", "shortest"]
        [[["practice", "makes", "perfect", "coding", "makes"]], ["coding", "practice"], ["makes", "coding"]]
        Output
        [null, 3, 1]

        Explanation
        WordDistance wordDistance = new WordDistance(["practice", "makes", "perfect", "coding", "makes"]);
        wordDistance.shortest("coding", "practice"); // return 3
        wordDistance.shortest("makes", "coding");    // return 1

-----------

```python
from collections import defaultdict
class WordDistance:
    """
    Being different from Question 243
    here our class might be called multiple times once initialized
    Therefore, we need to find an efficient way to solve for shortest distance
    we store the index for each word's occurence during initialization
    and we do a two-pointer approach to slide through the two index list
    and everytime increment the smaller one for a possible smaller distance
    
    denote n := len(wordsDict), m1 := occurence of word1, m2 := occurence of word2
    Time Complexity : __init__() O(n), shortest() max(m1, m2), which could be of O(n)
    Space Complexity : O(n) for hashmap storage
    """
    def __init__(self, wordsDict: List[str]):
        self.index_mapping = defaultdict(list)
        for idx, word in enumerate(wordsDict):
            self.index_mapping[word].append(idx)

    def shortest(self, word1: str, word2: str) -> int:
        index_lst1, index_lst2 = self.index_mapping[word1], self.index_mapping[word2]
        ptr1, ptr2 = 0, 0
        shortest = float('inf')
        while ptr1 < len(index_lst1) and ptr2 < len(index_lst2):
            shortest = min(shortest, abs(index_lst1[ptr1]-index_lst2[ptr2]))
            if index_lst1[ptr1] < index_lst2[ptr2]:
                ptr1 += 1
            else:
                ptr2 += 1
        return shortest


# Your WordDistance object will be instantiated and called as such:
# obj = WordDistance(wordsDict)
# param_1 = obj.shortest(word1,word2)
```
