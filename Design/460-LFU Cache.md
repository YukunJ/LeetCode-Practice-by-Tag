**460. LFU Cache**

```Tag : design/doubly-linked list/hashtable```

**Description:**

Design and implement a data structure for a *Least Frequently Used (LFU)* cache.

Implement the ```LFUCache``` class:

+ ```LFUCache(int capacity)``` Initializes the object with the ```capacity``` of the data structure.

+ ```int get(int key)``` Gets the value of the ```key``` if the ```key``` exists in the cache. Otherwise, returns ```-1```.

+ ```void put(int key, int value)``` Update the value of the ```key``` if present, or inserts the ```key``` if not already present. When the cache reaches its ```capacity```, it should invalidate and remove the **least frequently used key** before inserting a new item. For this problem, when there is a **tie** (i.e., two or more keys with the same frequency), the **least recently used key** would be invalidated.

To determine the least frequently used key, a **use counter** is maintained for each key in the cache. The key with the smallest **use counter** is the least frequently used key.

When a key is first inserted into the cache, its **use counter** is set to ```1``` (due to the ```put``` operation). The **use counter** for a key in the cache is incremented either a ```get``` or ```put``` operation is called on it.

The functions ```get``` and ```put``` must each run in ```O(1)``` average time complexity.

**Example1:**

		Input
		["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
		[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
		Output
		[null, null, null, 1, null, -1, 3, null, -1, 3, 4]

		Explanation
		// cnt(x) = the use counter for key x
		// cache=[] will show the last used order for tiebreakers (leftmost element is  most recent)
		LFUCache lfu = new LFUCache(2);
		lfu.put(1, 1);   // cache=[1,_], cnt(1)=1
		lfu.put(2, 2);   // cache=[2,1], cnt(2)=1, cnt(1)=1
		lfu.get(1);      // return 1
                 		// cache=[1,2], cnt(2)=1, cnt(1)=2
		lfu.put(3, 3);   // 2 is the LFU key because cnt(2)=1 is the 		smallest, invalidate 2.
                 		// cache=[3,1], cnt(3)=1, cnt(1)=2
		lfu.get(2);      // return -1 (not found)
		lfu.get(3);      // return 3
                 		// cache=[3,1], cnt(3)=2, cnt(1)=2
		lfu.put(4, 4);   // Both 1 and 3 have the same cnt, but 1 is LRU, invalidate 1.
                 		// cache=[4,3], cnt(4)=1, cnt(3)=2
		lfu.get(1);      // return -1 (not found)
		lfu.get(3);      // return 3
                 		// cache=[3,4], cnt(4)=1, cnt(3)=3
		lfu.get(4);      // return 4
                 		// cache=[3,4], cnt(4)=2, cnt(3)=3

-----------

```python
class DNode:
    """Helper class: doubly-linked list Node"""
    
    def __init__(self, key=None, value=None, prev=None, next=None, freq=0):
        self.key = key
        self.value = value
        self.prev = prev
        self.next = next
        self.freq = freq
    
    def insert_after(self, another: "DNode"):
        # insert another node after the current node
        another.prev = self
        another.next = self.next
        self.next.prev = another
        self.next = another

def create_linked_list():
    head, tail = DNode(), DNode()
    head.next, tail.prev = tail, head
    return (head, tail)

class LFUCache:
    """
    We will maintain both a frequency map and key map 
    to achieve O(1) time complextiy for both get() and put()
    The key map will map a key to a single Node
    The frequency map will map to a doubly linked list of nodes of that frequency
    
    denote n := number of key value pair in the cache
    Time Complexity : O(1) for both __init__(), get(), put()
    Space Complexity : O(n)
    """
    from collections import defaultdict
    
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.size = 0
        self.min_freq = 0
        self.keymap = dict()
        self.freqmap = defaultdict(create_linked_list)
    
    def delete(self, node) -> int:
        # delete a node from its freq and return its key
        if node.prev: # a valid true node, not header
            node.prev.next = node.next
            node.next.prev = node.prev
            if node.prev is self.freqmap[node.freq][0] and node.next is self.freqmap[node.freq][-1]:
                # the only node with this frequency
                self.freqmap.pop(node.freq)
        return node.key
    
    def increase(self, node) ->None:
        # increase the frequency of a node, and update minFreq accordingly
        node.freq += 1
        self.delete(node)
        self.freqmap[node.freq][-1].prev.insert_after(node)
        if node.freq == 1:
            self.min_freq = 1
        elif node.freq == self.min_freq + 1:
            # if min_freq is empty, upgrade by 1
            head, tail = self.freqmap[self.min_freq]
            if head.next is tail:
                self.min_freq = node.freq
        
    def get(self, key: int) -> int:
        # if found, need to increase frequency
        if key in self.keymap:
            self.increase(self.keymap[key])
            return self.keymap[key].value
        return -1
        
    def put(self, key: int, value: int) -> None:
        if self.capacity > 0:
            if key in self.keymap:
                node = self.keymap[key]
                node.value = value
            else:
                node = DNode(key=key, value=value)
                self.keymap[key] = node
                self.size += 1
            if self.size > self.capacity:
                self.size -= 1
                deleted_key = self.delete(self.freqmap[self.min_freq][0].next)
                self.keymap.pop(deleted_key)
            self.increase(node)
```
