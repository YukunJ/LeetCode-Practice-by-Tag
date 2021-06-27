**295. Find Median from Data Stream**

```Tag : Heap```

The **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value and the median is the mean of the two middle values.

+ For example, for arr = ```[2,3,4]```, the median is ```3```.
+ For example, for arr = ```[2,3]```, the median is ```(2 + 3) / 2 = 2.5```.

Implement the MedianFinder class:

+ ```MedianFinder()``` initializes the MedianFinder object.
+ ```void addNum(int num)``` adds the integer ```num``` from the data stream to the data structure.
+ ```double findMedian()``` returns the median of all elements so far. Answers within ```10^-5``` of the actual answer will be accepted.

**Example1**:

        Input
        ["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
        [[], [1], [2], [], [3], []]
        Output
        [null, null, null, 1.5, null, 2.0]

        Explanation
        MedianFinder medianFinder = new MedianFinder();
        medianFinder.addNum(1);    // arr = [1]
        medianFinder.addNum(2);    // arr = [1, 2]
        medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
        medianFinder.addNum(3);    // arr[1, 2, 3]
        medianFinder.findMedian(); // return 2.0


-----------

```python
import heapq
class MedianFinder:
    """
    This is a on-line sorting problem
    while adding new number into the object, we need be ready to pick the medium
    We keep two parts of our data, "left" and "right"
    "left" : the first 50% smaller of data so far
    "right" : the bigger 50% of data so far
    and we keep "left" and "right" balanced, so that
    either "left" contatins elements one more than that in "right" (odd case)
    or "left" contains as many elements as in "right" (even case)
    so we repeated do "try add in left", then "try add in right"
    In odd case, the medium is the largest one in "left"
    In even case, the medium is mean of the largest one in "left" and smallest in "right"
    To fast access to these elements, we make "left" a max-heap, "right" a min-heap
    
    denote the number of elements already in object is n
    Time Complexity : addNum O(logn), findMedian O(1)
    Space Complexity : O(n)
    """
    def __init__(self):
        """
        initialize your data structure here.
        """
        self.left = [] # max-heap
        self.right = [] # min-heap

    def addNum(self, num: int) -> None:
        if len(self.left) == len(self.right):
            # add a new one into left
            to_left = heapq.heappushpop(self.right, num)
            heapq.heappush(self.left, -to_left) # negation in left heap
        
        elif len(self.left) > len(self.right):
            # add a new one into right
            to_right = -heapq.heappushpop(self.left, -num) # negation in left heap
            heapq.heappush(self.right, to_right)

    def findMedian(self) -> float:
        if (len(self.left) + len(self.right)) % 2 == 0: # even case
            return (-self.left[0] + self.right[0]) / 2
        else: # odd case
            return -self.left[0]
        


# Your MedianFinder object will be instantiated and called as such:
# obj = MedianFinder()
# obj.addNum(num)
# param_2 = obj.findMedian()
```
