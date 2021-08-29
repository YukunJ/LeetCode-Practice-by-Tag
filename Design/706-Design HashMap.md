**706. Design HashMap**

```Tag : design/hashtable/array/linked list```

**Description:**

Design a HashMap without using any built-in hash table libraries.

Implement the ```MyHashMap``` class:

+ ```MyHashMap()``` initializes the object with an empty map.

+ ```void put(int key, int value)``` inserts a ```(key, value)``` pair into the HashMap. If the ```key``` already exists in the map, update the corresponding ```value```.

+ ```int get(int key)``` returns the value to which the specified ```key``` is mapped, or ```-1``` if this map contains no mapping for the ```key```.

+ ```void remove(key)``` removes the ```key``` and its corresponding ```value``` if the map contains the mapping for the ```key```.


**Example1:**

		Input
		["MyHashMap", "put", "put", "get", "get", "put", "get", "remove", "get"]
		[[], [1, 1], [2, 2], [1], [3], [2, 1], [2], [2], [2]]
		Output
		[null, null, null, 1, -1, null, 1, null, -1]

		Explanation
		MyHashMap myHashMap = new MyHashMap();
		myHashMap.put(1, 1); // The map is now [[1,1]]
		myHashMap.put(2, 2); // The map is now [[1,1], [2,2]]
		myHashMap.get(1);    // return 1, The map is now [[1,1], [2,2]]
		myHashMap.get(3);    // return -1 (i.e., not found), The map is now [[1,1], [2,2]]
		myHashMap.put(2, 1); // The map is now [[1,1], [2,1]] (i.e., update the existing value)
		myHashMap.get(2);    // return 1, The map is now [[1,1], [2,1]]
		myHashMap.remove(2); // remove the mapping for 2, The map is now [[1,1]]
		myHashMap.get(2);    // return -1 (i.e., not found), The map is now [[1,1]]

-----------

```python
class Bucket:
    """Helper class: container for key with the same modulo results"""
    
    def __init__(self):
        self.bucket = list()
    
    def get(self, key: int) -> int:
        for bucket_key, value in self.bucket:
            if bucket_key == key:
                return value
        return -1
    
    def add(self, key: int, value: int) -> None:
        for idx, (bucket_key, old_value) in enumerate(self.bucket):
            if bucket_key == key:
                # if already exists, update the key <-> value pair
                self.bucket[idx] = (key, value)
                return
        # not exist so far
        self.bucket.append((key, value)) # add a new key <-> value pair
    
    def delete(self, key: int) -> None:
        for idx, (bucket_key, value) in enumerate(self.bucket):
            if bucket_key == key:
                self.bucket.pop(idx)
                return
            
class MyHashMap:
    """
    To design a map, we use modulo hashing + chaining method for collison
    Complexity Analysis:
    Time Complexity : 
    We expect both put(), get(), remove() could give approximate O(1) amortized performance
    denote n is the key <-> value pair stored so far, k is the prime number we pick
    on average, every bucket should have n / k elements, therefore:
    put() : O(n/k) on average
    get() : O(n/k) on average
    delete() : O(n/k) on average
    
    Space Complexity : O(n+k) where n is the key <-> value pair stored so far, k is the prime number we pick
    """
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.bucket_lst = [Bucket() for _ in range(3371)] # pick a lovely prime number
        self.prime = 3371

    def put(self, key: int, value: int) -> None:
        """
        value will always be non-negative.
        """
        self.bucket_lst[key % self.prime].add(key, value)
        

    def get(self, key: int) -> int:
        """
        Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key
        """
        return self.bucket_lst[key % self.prime].get(key)
        

    def remove(self, key: int) -> None:
        """
        Removes the mapping of the specified value key if this map contains a mapping for the key
        """
        self.bucket_lst[key % self.prime].delete(key)


# Your MyHashMap object will be instantiated and called as such:
# obj = MyHashMap()
# obj.put(key,value)
# param_2 = obj.get(key)
# obj.remove(key)
```
