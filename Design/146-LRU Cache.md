**146. LRU Cache**

```Tag: Design/Doubly-Linked List```

**Description:**

Design a data structure that follows the constraints of a **Least Recently Used (LRU) cache**.

Implement the ```LRUCache``` class:

+ ```LRUCache(int capacity)``` Initialize the LRU cache with **positive** size capacity.

+ ```int get(int key)``` Return the value of the ```key``` if the key exists, otherwise return ```-1```.

+ ```void put(int key, int value)``` Update the value of the ```key``` if the key exists. Otherwise, add the ```key-value``` pair to the cache. If the number of keys exceeds the ```capacity``` from this operation, **evict** the least recently used key.

The functions ```get``` and ```put``` must each run in ```O(1)``` average time complexity.

**Example1**:

		Input
		["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
		[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]

		Output
		[null, null, null, 1, null, -1, null, -1, 3, 4]

		Explanation
		LRUCache lRUCache = new LRUCache(2);
		lRUCache.put(1, 1); // cache is {1=1}
		lRUCache.put(2, 2); // cache is {1=1, 2=2}
		lRUCache.get(1);    // return 1
		lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
		lRUCache.get(2);    // returns -1 (not found)
		lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
		lRUCache.get(1);    // return -1 (not found)
		lRUCache.get(3);    // return 3
		lRUCache.get(4);    // return 4

-----------

```python
class DLinkedNode:
    """Helper Class: doubly-linked list"""
    
    def __init__(self, key=0, val=0, prev=None, next=None):
        self.key = key
        self.val = val
        self.prev = prev
        self.next = next

class LRUCache:
    """
    We wuill jointly use a hashmap and doubly-linked list to implement the desired LRU cache
    we use hashmap to get the key-value pair quickly in O(1) time
    and the hashmap maps key to its position node in the doubly linked list
    In doubly linked list, given a node position, we can remove it or update it to the head in O(1) time
    
    Time Complexity : __init__(), get(), put() both O(1)
    Space Complexity : O(capacity), both hashmap and doubly linked list store at most O(capacity) elements
    """

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.size = 0
        self.head, self.tail = DLinkedNode(), DLinkedNode() # header and tailer for boundary
        self.head.next, self.tail.prev = self.tail, self.head
        self.cache = dict() # hashmap {key --> Node in Doubly-linked list}
        
    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        node = self.cache[key]
        self.MoveToHead(node) # update its position in LRU
        return node.val

    def put(self, key: int, value: int) -> None:
        if key not in self.cache:
            # make a new node
            node = DLinkedNode(key=key, val=value)
            # record in hashmap
            self.cache[key] = node
            # move to head of Doubly linked list
            self.AddToHead(node)
            self.size += 1
            if self.size > self.capacity:
                # too many nodes, need to remove the tail
                removedNode = self.RemoveTail()
                del self.cache[removedNode.key] # remove the hash mapping
                self.size -= 1
        else:
            node = self.cache[key]
            node.val = value # update the value
            self.MoveToHead(node)   
        
    def RemoveNode(self, node: DLinkedNode) -> DLinkedNode:
        # make the doubly linking between prev <---> next
        node.prev.next = node.next
        node.next.prev = node.prev
        return node
    
    def RemoveTail(self) -> DLinkedNode:
        return self.RemoveNode(self.tail.prev)
    
    def AddToHead(self, node: DLinkedNode) -> None:
        node.prev = self.head
        node.next = self.head.next
        self.head.next = node
        node.next.prev = node
    
    def MoveToHead(self, node: DLinkedNode) -> None:
        self.RemoveNode(node)
        self.AddToHead(node)

# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```