## Trie (Prefix Tree)

**Source**: Leetcode Question **208**. [Implement Trie(Prefix Tree)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/solution/shi-xian-trie-qian-zhui-shu-by-leetcode-ti500/)

```python
class Trie:
    """
    Time Complexity : O(|S|) for operation, where S is the length of input string for search or insert
    
    Space Complexity : the worst case is that, all inserted words have different prefix. Given we have 26 characters in alphabet, 
    if the Trie has height n, then the space storage is O(26^n). And to interpret the height "n" in term of the sum of total inserted words |sigma|, 
    we image, each character in |sigma| occupy an individual place in the Trie, contributing to 26 children. 
    Therefore, the space Complexity could also be written as O(|sigma|*26), 
    where |sigma| is the sum of character length of all inserted words.
    """
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.children = [None] * 26 # alphabet
        self.isEnd = False # indicator of the end of a word
    
    def searchPrefix(self, prefix: str) -> "Trie":
        """
        Helper function to return the Node of a prefix search if success
        """
        node = self
        for c in prefix:
            ch = ord(c) - ord('a')
            if not node.children[ch]: # not found, return None
                return None
            node = node.children[ch] # move to next character
        return node
    
    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.
        """
        node = self
        for c in word:
            ch = ord(c) - ord('a')
            if not node.children[ch]: # create a new linking
                node.children[ch] = Trie()
            node = node.children[ch]
        node.isEnd = True
        
    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        """
        node = self.searchPrefix(word)
        return node is not None and node.isEnd

    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        node = self.searchPrefix(prefix)
        return node is not None

# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```
