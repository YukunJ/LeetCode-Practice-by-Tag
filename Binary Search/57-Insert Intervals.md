**57. Insert Intervals**

```Tag: Array/Sorting```

**Description:**

Given a set of *non-overlapping* intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.

**Example1:**

		Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
		Output: [[1,5],[6,9]]


**Example2:**

		Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
		Output: [[1,2],[3,10],[12,16]]
		Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].

**Example3:**

		Input: intervals = [], newInterval = [5,7]
		Output: [[5,7]]

**Example4:**

		Input: intervals = [[1,5]], newInterval = [2,3]
		Output: [[1,5]]

**Example5:**

		Input: intervals = [[1,5]], newInterval = [2,7]
		Output: [[1,7]]

**Constraint**:

+ ```intervals``` is sorted by ```intervals[i][0]``` in **ascending** order.

-----------

**Solution1: Inherit from Q56**

```python
class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        """
        Given the merge functionality from Q56,
        all we need to do is to insert the newInterval at a proper index
        to maintain the ascending "sorted-ness" of the intervals
        we use a binary search to find the insertion position and insert it.
        the binary search takes O(logn), insertion may take O(n) depending on the inserting position, merge takes O(n)
        therefore:

        denote n := len(intervals)
        Time Complexity : O(n) dominated by merge process
        Space Complexity : O(1) we don't use auxiliary space except the answer array
        """
        # find the largest index of starting time that's smaller than newInterval.start
        newStart = newInterval[0]
        left, right = 0, len(intervals)-1
        insert_idx = -1
        while left <= right:
            mid = left + (right - left) // 2
            if intervals[mid][0] < newStart:
                insert_idx = mid
                left = mid + 1
            else:
                right = mid - 1
        intervals.insert(insert_idx+1, newInterval)
        return self.merge(intervals)


    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        """
        The merge functionality from Q56 <Merge Intervals>
        This time there is no need to sort first, which saves time and space
        denote n := len(intervals)
        Time Complexity : O(n) merge takes one pass O(n)
        Space Complexity : O(1)
        """
        # sorting in ascending starting time
        n = len(intervals)
        ptr = 0
        res = []
        while ptr < n:
            curr_start, curr_end = intervals[ptr]
            next_ptr = ptr + 1
            while next_ptr < n and intervals[next_ptr][0] <= curr_end:
                # try expand the window's end
                curr_end = max(curr_end, intervals[next_ptr][1])
                next_ptr += 1
            # reach here means the next interval's start is strict larger than curr's end
            # have to start a new individual interval
            res.append([curr_start, curr_end])
            ptr = next_ptr
        return res
```

-----------

**Solution2: Better Solution**

```python
class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        """
        A close analysis reveals the fact that, for two intervals [s1, f1] and [s2, f2]
        there are only two cases:
        1) they don't overlap, either f1 < s2 or s1 > f2
        2) they overlap, the merged inveral of them is [min(s1, s2), max(f1, f2)]
        using the fact that the intervals are already sorted in ascending order
        we could one-pass through the interval lists,
        when the interval[i] doesn't overlap with newInterval, add it to result lsit
        when the interval[i] does overlap with the newInterval, newInterval "absort" it to expand its window

        denote n := len(intervals)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        newStart, newEnd = newInterval
        res = []
        newAdded = False
        for Start, End in intervals:
            if Start > newEnd:
                # at right side of new interval
                if not newAdded:
                    newAdded = True
                    res.append([newStart, newEnd])
                res.append([Start, End])
            elif End < newStart:
                # at left side of new interval
                res.append([Start, End])
            else:
                # have intersection, absort!
                newStart, newEnd = min(newStart, Start), max(newEnd, End)
        
        if not newAdded:
            # the new interval may win till the end and not added yet
            res.append([newStart, newEnd])
        return res
```