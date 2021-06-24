**239. Sliding Window Maximum**

```Tag : Heap/Monotonic Queue/Sliding Window```

**Description:**

You are given an array of integers ```nums```, there is a sliding window of size ```k``` which is moving from the very left of the array to the very right. You can only see the ```k``` numbers in the window. Each time the sliding window moves right by one position.

Return the *max sliding window*.

+ hint1: How about using a data structure such as deque (double-ended queue)?
+ hint2: The queue size need not be the same as the window’s size.
+ hint3: Remove redundant elements and the queue should store only elements that need to be considered.

**Example1**:

        Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
        Output: [3,3,5,5,6,7]
        Explanation: 
        Window position                Max
        ---------------               -----
        [1  3  -1] -3  5  3  6  7       3
         1 [3  -1  -3] 5  3  6  7       3
         1  3 [-1  -3  5] 3  6  7       5
         1  3  -1 [-3  5  3] 6  7       5
         1  3  -1  -3 [5  3  6] 7       6
         1  3  -1  -3  5 [3  6  7]      7

**Example2**:

        Input: nums = [1], k = 1
        Output: [1]

**Example3**:

        Input: nums = [1,-1], k = 1
        Output: [1,-1]

**Example4**:

        Input: nums = [9,11], k = 2
        Output: [11]

**Example5**:

        Input: nums = [4,-2], k = 2
        Output: [4]
        
-----------

**Solution1: Monotonic Queue**

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        """
        We will maintain a Monotonic Queue
        so that the queue is decreasing from Head to Tail
        when we move one position next,
        the sliding window essentially lose one element and add one element
        if the losing one equal the head of the queue, we pop it out
        if the adding one is larger than the tail of queue, we continuously pop out the tail 
            until empty or the previous element larger than the adding element
        And the head of the Queue every snapshot will be the max of sliding window

        denote n := len(nums)
        Time Complexity : O(n)
        Space Complexity : O(k) the queue is at most len k, might be smaller during the process
        """
        from collections import deque
        Queue = deque()
        maxSlidingWindow = []

        # construct the first k-window
        for i in range(k):
            while Queue and Queue[-1] < nums[i]:
                Queue.pop()
            Queue.append(nums[i])

        maxSlidingWindow.append(Queue[0])

        for i in range(len(nums)-k):
            # pop head
            if Queue and nums[i] == Queue[0]:
                # the max is the one we are losing
                Queue.popleft()
            # pop tail
            while Queue and Queue[-1] < nums[i+k]:
                # no longer need those "smaller" yet "former entered" ones
                Queue.pop()
            # add this element
            Queue.append(nums[i+k])
            # add to the max sliding window
            maxSlidingWindow.append(Queue[0])
        
        return maxSlidingWindow
```

-----------

**Solution2: Heap**

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        """
        Using a Heap to achieve the same procedure as the Monotonic Queue
        
        denote n := nums
        worst case: consider an increasing sequence nums, 
            the heap never pop out elements
            the heap top is always the biggest and newest element
        Time Complexity : O(n*logn)
        Space Complexity : O(n)
        """
        # notice heapq supports a min-heap
        import heapq
        maxSlidingWindow = []
        # deal with the first k-window, needs to store index as well
        heap = [(-nums[i], i) for i in range(k)]
        heapq.heapify(heap)
        maxSlidingWindow.append(-heap[0][0])

        # deal with the rest window
        for i in range(len(nums)-k):
            while heap and heap[0][1] <= i:
                heapq.heappop(heap)
            heapq.heappush(heap,(-nums[i+k], i+k))
            maxSlidingWindow.append(-heap[0][0])
        
        return maxSlidingWindow
```
