**253. Meeting Rooms II**

```Tag : heap/sorting/greedy```

**Description:**

Given an array of meeting time ```intervals``` where ```intervals[i] = [starti, endi]```, determine if a person could attend all meetings.

**Example1:**

		Input: intervals = [[0,30],[5,10],[15,20]]
		Output: false

**Example2:**

		Input: intervals = [[7,10],[2,4]]
		Output: true

-----------

Solution1: Heap

```python
class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        """
        we want to save the number of rooms, i.e. fit a room with as compacy scheudle as possible
        we will first sort the intervals in ascending order of starting time, 
        then we initialize a min-heap which stores the tuple of form
            (room index, the ending meeting time so far)
        if the new meeting can fit in, we update the ending time and re-insert back this tuple
        otherwise we would have to initialize a new room for this meeting
        
        notice the greediness here, when we pop from the min-heap,
        we don't destory "possible in-between small intervals" because the intervals are sorted
        so that next one is going to start later, doesn't matter
        
        denote n := len(intervals)
        Time Complexity : O(nlogn) sorting takes O(nlogn), each heap operation is O(logn)
        Space Complexity : O(n) need at most n room for meetings, heap at most n elements
        
        """
        import heapq
        room_num = 0
        
        heap = []
        intervals.sort(key=lambda x: x[0])
        for start, end in intervals:
            if heap and heap[0][0] <= start:
                # can update and insert this meeting to an existing room
                latest_end, room_idx = heapq.heappop(heap)
                heapq.heappush(heap, (end, room_idx))
            else:
                room_num += 1
                heapq.heappush(heap, (end, room_num))
        return room_num
```

-----------

Solution2: chronological ordering

```python
class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        """
        We can do this problem in chronological ordering way
        we are going to sort starting time and ending time separately.
        This might sounds counter-intuitive, we use two pointers for start and end
        when the curr end is smaller than curr start, we know one room is freed up
        
        denote n := len(intervals)
        Time Complexity : O(nlogn) sorting takes O(nlogn), the rest is O(n) one pass-through
        Space Complexity : O(n) need at most n room for meetings, heap at most n elements
        
        """
        start_times = sorted([start for start, end in intervals])
        end_times = sorted([end for start, end in intervals])
        room_num = 0
        start_ptr, end_ptr = 0, 0
        while start_ptr < len(intervals):
            if end_times[end_ptr] <= start_times[start_ptr]:
                # a free room available
                room_num -= 1
                end_ptr += 1
            room_num += 1
            start_ptr += 1
        return room_num
```
