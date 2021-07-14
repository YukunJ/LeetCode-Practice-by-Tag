**981. Time Based Key-Value Store**

```Tag: Design/Hashtable/Binary Search```

**Description:**

Design a time-based key-value data structure that can store multiple values for the same key at different time stamps and retrieve the key's value at a certain timestamp.

Implement the ```TimeMap``` class:

+ ```TimeMap()``` Initializes the object of the data structure.

+ ```void set(String key, String value, int timestamp)``` Stores the key ```key``` with the value ```value``` at the given time ```timestamp```.

+ ```String get(String key, int timestamp)``` Returns a value such that ```set``` was called previously, with ```timestamp_prev <= timestamp```. If there are multiple such values, it returns the value associated with the largest ```timestamp_prev```. If there are no values, it returns ```""```.

**Constraint**:

+ All the timestamps ```timestamp``` of ```set()``` are strictly increasing.

**Example1**:

		Input

		["TimeMap", "set", "get", "get", "set", "get", "get"]
		[[], ["foo", "bar", 1], ["foo", 1], ["foo", 3], ["foo", "bar2", 4], ["foo", 4], ["foo", 5]]

		Output
		[null, null, "bar", "bar", null, "bar2", "bar2"]

		Explanation
		TimeMap timeMap = new TimeMap();
		timeMap.set("foo", "bar", 1);  // store the key "foo" and value "bar" along with timestamp = 1.
		timeMap.get("foo", 1);         // return "bar"
		timeMap.get("foo", 3);         // return "bar", since there is no value corresponding to foo at timestamp 3 and timestamp 2, then the only value is at timestamp 1 is "bar".
		timeMap.set("foo", "bar2", 4); // store the key "foo" and value "ba2r" along with timestamp = 4.
		timeMap.get("foo", 4);         // return "bar2"
		timeMap.get("foo", 5);         // return "bar2"

-----------

```python
class TimeTuple:
    """
    Helper class: for (timestamp, value) storage 
    and overload comparison operator '<' and '<=' for binary search 
    """
    def __init__(self, timestamp: int, value: str):
        self.timestamp = timestamp
        self.value = value

    def __lt__(self, other: "TimeTuple") -> bool:
        return self.timestamp < other.timestamp
    
    def __le__(self, other: "TimeTuple") -> bool:
        return self.timestamp <= other.timestamp

    def __str__(self) -> str:
        return 'Time:{} -> Value:{}'.format(self.timestamp, self.value)

class TimeMap:
    """
    As this question's label indicates,
    we will use hashmap mapping key :-> value list
    and to find the value with a specific timestamp, we will use binary search
    so each value list is sorted in order of the timestamp when they are inserted
    denote n := num of elements in the TimeMap
    in worst case, all n elements are in the value list of same key
    Time Complexity : both set() and get() are O(logn) by binary search
    Space Complexity : O(n) whole storage
    """
    def __init__(self):
        """
        Initialize your data structure here.
        """
        from collections import defaultdict
        self.keyMap = defaultdict(list)
        
    def set(self, key: str, value: str, timestamp: int) -> None:
        # Find the largest timestamp's index that's smaller than given timestamp
        # and insert the given to the next position
        # because the constraint that timestamp of set() is increasing
        # we could directly add it to the end of array
        # However, we also provide functionality below in comment that
        # even if the set() is not increasing in timestamp
        # we could also use binary search to find a proper place to insert into
        self.keyMap[key].append(TimeTuple(timestamp, value))
        """
        # binary search for non-increasing version of set() functionality
        timetuple = TimeTuple(timestamp, value) # create tuple class storage
        if len(self.keyMap[key]) == 0:
            self.keyMap[key] = [timetuple]
        else:
            left, right = 0, len(self.keyMap[key])-1
            while left <= right:
                mid = left + (right - left) // 2 
                # we are guaranteed unique timestamp in this question
                if self.keyMap[key][mid] < timetuple:
                    left = mid + 1
                else:
                    right = mid - 1
            self.keyMap[key].insert(right+1, timetuple)
        """
        
    def get(self, key: str, timestamp: int) -> str:
        # Find the largest timestamp's value that's smaller than given timestamp
        # and return its value
        if len(self.keyMap[key]) == 0: # boundary case
            return ""
        timetuple = TimeTuple(timestamp, '') # fake tuple for comparison convenience
        left, right = 0, len(self.keyMap[key])-1
        ans = -1
        while left <= right:
            mid = left + (right - left) // 2
            if self.keyMap[key][mid] <= timetuple:
                ans = mid 
                left = mid + 1
            else:
                right = mid - 1
        # if ans=-1 in the end, means not found any smaller timpestamp tuple
        # return "" empty string
        return self.keyMap[key][ans].value if ans >= 0 else ""
```