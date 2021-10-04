**359. Logger Rate Limiter**

```Tag : design/hashtable```

**Description:**

Design a logger system that receives a stream of messages along with their timestamps. Each **unique** message should only be printed **at most every 10 seconds** (i.e. a message printed at timestamp ```t``` will prevent other identical messages from being printed until timestamp ```t + 10```).

All messages will come in chronological order. Several messages may arrive at the same timestamp.

Implement the ```Logger``` class:

+ ```Logger()``` Initializes the logger object.

+ ```bool shouldPrintMessage(int timestamp, string message)``` Returns ```true``` if the message should be printed in the given ```timestamp```, otherwise returns ```false```.

**Example1:**

        Input
        ["Logger", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage"]
        [[], [1, "foo"], [2, "bar"], [3, "foo"], [8, "bar"], [10, "foo"], [11, "foo"]]
        Output
        [null, true, true, false, false, false, true]

        Explanation
        Logger logger = new Logger();
        logger.shouldPrintMessage(1, "foo");  // return true, next allowed timestamp for "foo" is 1 + 10 = 11
        logger.shouldPrintMessage(2, "bar");  // return true, next allowed timestamp for "bar" is 2 + 10 = 12
        logger.shouldPrintMessage(3, "foo");  // 3 < 11, return false
        logger.shouldPrintMessage(8, "bar");  // 8 < 12, return false
        logger.shouldPrintMessage(10, "foo"); // 10 < 11, return false
        logger.shouldPrintMessage(11, "foo"); // 11 >= 11, return true, next allowed timestamp for "foo" is 11 + 10 = 21


-----------

```python
from collections import defaultdict

class Logger:

    """
    We use a hashmap to store when does a message come last time
    and will return False if the gap is less than 10 timestamps
    if the gap is longer than 10 timestamps, we will update the timestamp and allow to print the message
    
    denote n := the number of unique messages ever appear in the stream
    Time Complexity : O(1) for both __init__() and shouldPrintMessage()
    Space Complexity : O(n) for hashtable storage
    """
    def __init__(self):
        self.ratelimiter = defaultdict(lambda : float('-inf'))

    def shouldPrintMessage(self, timestamp: int, message: str) -> bool:
        if timestamp - self.ratelimiter[message] < 10:
            return False
        self.ratelimiter[message] = timestamp
        return True


# Your Logger object will be instantiated and called as such:
# obj = Logger()
# param_1 = obj.shouldPrintMessage(timestamp,message)
```

