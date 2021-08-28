**642. Design Search Autocomplete System**

```Tag : trie/design/string/hashtable```

**Description:**

Design a search autocomplete system for a search engine. Users may input a sentence (at least one word and end with a special character ```'#'```).

You are given a string array ```sentences``` and an integer array ```times``` both of length ```n``` where ```sentences[i]``` is a previously typed sentence and ```times[i]``` is the corresponding number of times the sentence was typed. For each input character except ```'#'```, return the top ```3``` historical hot sentences that have the same prefix as the part of the sentence already typed.

Here are the specific rules:

+ The hot degree for a sentence is defined as the number of times a user typed the exactly same sentence before.

+ The returned top 3 hot sentences should be sorted by hot degree (The first is the hottest one). If several sentences have the same hot degree, use ASCII-code order (smaller one appears first).

+ If less than ```3``` hot sentences exist, return as many as you can.

+ When the input is a special character, it means the sentence ends, and in this case, you need to return an empty list.

Implement the ```AutocompleteSystem``` class:

+ ```AutocompleteSystem(String[] sentences, int[] times)``` Initializes the object with the ```sentences ```and ```times``` arrays.

+ ```List<String> input(char c)``` This indicates that the user typed the character ```c```.

	+ Returns an empty array ```[]``` if ```c == '#'``` and stores the inputted sentence in the system.

	+ Returns the top ```3``` historical hot sentences that have the same prefix as the part of the sentence already typed. If there are fewer than ```3``` matches, return them all.


**Example1:**

		Input
		["AutocompleteSystem", "input", "input", "input", "input"]
		[[["i love you", "island", "iroman", "i love leetcode"], [5, 3, 2, 2]], ["i"], [" "], ["a"], ["#"]]
		Output
		[null, ["i love you", "island", "i love leetcode"], ["i love you", "i love leetcode"], [], []]

		Explanation
		AutocompleteSystem obj = new AutocompleteSystem(["i love you", "island", "iroman", "i love leetcode"], [5, 3, 2, 2]);
		obj.input("i"); // return ["i love you", "island", "i love leetcode"]. There are four sentences that have prefix "i". Among them, "ironman" and "i love leetcode" have same hot degree. Since ' ' has ASCII code 32 and 'r' has ASCII code 114, "i love leetcode" should be in front of "ironman". Also we only need to output top 3 hot sentences, so "ironman" will be ignored.
		obj.input(" "); // return ["i love you", "i love leetcode"]. There are only two sentences that have prefix "i ".
		obj.input("a"); // return []. There are no sentences that have prefix "i a".
		obj.input("#"); // return []. The user finished the input, the sentence "i a" should be saved as a historical sentence in system. And the following input will be counted as a new search.

-----------

```python
import heapq
from collections import defaultdict

class MyTuple:
    """Helper class: overloading comparison operator"""
    
    def __init__(self, postfix: str, hot: int):
        self.postfix = postfix
        self.hot = hot
    
    def __lt__(self, another: 'MyTuple') -> bool:
        if self.hot < another.hot:
            return True
        elif self.hot == another.hot and self.postfix >= another.postfix:
            return True
        return False
        
class Trie:
    """Helper class: Trie Node"""
    
    def __init__(self, isEnd=False, hot=0):
        self.trie = defaultdict(lambda: Trie())
        self.isEnd = isEnd
        self.hot = hot
    
    def add(self, sentence: str, hot: int):
        # add a whole sentence into our Trie and record its hottness
        curr = self
        for char in sentence:
            curr = curr.trie[char]
        curr.isEnd = True
        curr.hot += hot

class AutocompleteSystem:
    """
    We use a Trie data structure in solving this Autocomplete system
    Notice that to keep the top 3 hottest sentence memory, we use a Heap data structure
    
    Complexity Analysis:
    
    Time Complexity :
    __init__(): O(n) where n is the sum of character numbers in sentences
    input(): we do a dfs search on every node below the current Trie node,
            during that process, we operate on a heap, whose operation time is O(log{heap_size}) = O(log3) = O(1)
            denote k is depth remaining below the current node, p the size of alphabet,
            it would be worst case O(p^k)
    Space Complexity :
    In worst case, denote n is the sum of total character in the system stored so far,
    every one of them doesn't overlap, then it takes O(p^n) where p is the size of alphabet
    """
    def __init__(self, sentences: List[str], times: List[int]):
        self.root = Trie()
        for sentence, hot in zip(sentences, times):
            # initialize cache sentence and hottness
            self.root.add(sentence, hot)
        self.curr_input = ""
        self.curr_node = self.root

    def input(self, c: str) -> List[str]:
        if c == '#':
            # end of a search, clear cache and search
            self.root.add(self.curr_input, 1)
            self.curr_input = ""
            self.curr_node = self.root
            return []
        self.curr_input += c
        self.curr_node = self.curr_node.trie[c]
        self.res, self.temp = [], []
        self.dfs(self.curr_node)
        # these sentences share the same prefix anyway, just sort on different postfix
        auto_res = [self.curr_input+tp.postfix for tp in heapq.nlargest(3, self.res)]
        return auto_res
    
    def dfs(self, node: 'Trie'):
        # do a dfs backtracking search based on current node
        # to retrieve all following sentences string
        if node.isEnd:
            # record a whole complete sentence search
            # only insert into the heap if satisfy conditions
            curr_postfix = "".join(self.temp)
            if len(self.res) < 3:
                heapq.heappush(self.res, MyTuple(curr_postfix, node.hot))
            else:
                heapq.heappushpop(self.res, MyTuple(curr_postfix, node.hot))
        for key in node.trie.keys():
            self.temp.append(key)
            self.dfs(node.trie[key])
            self.temp.pop()
        

# Your AutocompleteSystem object will be instantiated and called as such:
# obj = AutocompleteSystem(sentences, times)
# param_1 = obj.input(c)
```
