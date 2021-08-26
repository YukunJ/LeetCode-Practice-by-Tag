**252. Meeting Rooms**

```Tag : sorting/array```

**Description:**

Given an array of meeting time ```intervals``` where ```intervals[i] = [starti, endi]```, determine if a person could attend all meetings.

**Example1:**

		Input: intervals = [[0,30],[5,10],[15,20]]
		Output: false

**Example2:**

		Input: intervals = [[7,10],[2,4]]
		Output: true

-----------

```python
class Solution:
    def canAttendMeetings(self, intervals: List[List[int]]) -> bool:
        """
        To check if a person can attend all meeting is equivalent to check
        if any two meetings have overlap time interval
        to do so, we will first sort the intervals based on their starting time
        and then check if interval[i].end <= interval[i+1].start for all meetings
        
        denote n := len(intervals)
        Time Complexity : O(nlogn) for sorting, the rest is one-pass through the list
        Space Complexity : O(1)
        """
        intervals.sort(key=lambda x: x[0])
        for i in range(len(intervals)-1):
            if intervals[i][1] > intervals[i+1][0]:
		# check for overlap	
                return False
        return True
```


