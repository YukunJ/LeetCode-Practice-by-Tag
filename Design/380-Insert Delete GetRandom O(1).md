**380. Insert Delete GetRandom O(1)**

```Tag : design/hashtable/array```

**Description:**

Implement the ```RandomizedSet``` class:

+ ```RandomizedSet()``` Initializes the ```RandomizedSet``` object.

+ ```bool insert(int val)``` Inserts an item ```val``` into the set if not present. Returns ```true``` if the item was not present, ```false``` otherwise.

+ ```bool remove(int val)``` Removes an item ```val``` from the set if present. Returns ```true``` if the item was present, ```false``` otherwise.

+ ```int getRandom()``` Returns a random element from the current set of elements (it's guaranteed that at least one element exists when this method is called). Each element must have the **same probability** of being returned.

You must implement the functions of the class such that each function works in **average** ```O(1)``` time complexity.

**Example1:**

        Input
        ["RandomizedSet", "insert", "remove", "insert", "getRandom", "remove", "insert", "getRandom"]
        [[], [1], [2], [2], [], [1], [2], []]
        Output
        [null, true, false, true, 2, true, false, 2]

        Explanation
        RandomizedSet randomizedSet = new RandomizedSet();
        randomizedSet.insert(1); // Inserts 1 to the set. Returns true as 1 was inserted successfully.
        randomizedSet.remove(2); // Returns false as 2 does not exist in the set.
        randomizedSet.insert(2); // Inserts 2 to the set, returns true. Set now contains [1,2].
        randomizedSet.getRandom(); // getRandom() should return either 1 or 2 randomly.
        randomizedSet.remove(1); // Removes 1 from the set, returns true. Set now contains [2].
        randomizedSet.insert(2); // 2 was already in the set, so return false.
        randomizedSet.getRandom(); // Since 2 is the only number in the set, getRandom() will always return 2.  

-----------

```python
class RandomizedSet:
    """
    It's natural to think using a hashtable for insert() and remove()
    But the problem is how to in average O(1) time get a random item
    we will use random number generator and also static storage for each existing item
    
    denote n := current number of items in the RandomizedSet
    Time Complexity : O(1) for every function
    Space Complexity : O(n)
    """
    import random
    from collections import defaultdict
    
    def __init__(self):
        self.size = 0
        self.storage = []
        self.map = dict()

    def insert(self, val: int) -> bool:
        if val not in self.map:
            self.map[val] = self.size
            self.storage.append(val)
            self.size += 1
            return True
        return False

    def remove(self, val: int) -> bool:
        if val in self.map:
            # swap the element to-delete to the end, and pop it using O(1)
            # and update the mapping for index
            index = self.map[val]
            self.map[self.storage[self.size-1]] = index
            self.storage[index], self.storage[self.size-1] = self.storage[self.size-1], self.storage[index]
            self.storage.pop()
            del self.map[val]
            self.size -= 1
            return True
        return False

    def getRandom(self) -> int:
        random_index = random.randint(0, self.size-1)
        return self.storage[random_index]
```

