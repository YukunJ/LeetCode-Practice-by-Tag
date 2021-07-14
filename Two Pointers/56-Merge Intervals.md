**56. Merge Intervals**

```Tag: Sorting/Pointer Sliding```

**Description:**

Given an array of ```intervals``` where ```intervals[i] = [start_i, end_i]```, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

**Example1:**

		Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
		Output: [[1,6],[8,10],[15,18]]
		Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].


**Example2:**

		Input: intervals = [[1,4],[4,5]]
		Output: [[1,5]]
		Explanation: Intervals [1,4] and [4,5] are considered overlapping.

-----------

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        """
        We could first sort the intervals in ascending order of starting position
        and then check adjacent interval, say A and B, we already have A.start < B.start
        if B.start < A.end, we might merge to get a larger interval [A.start, max(B.end, A.end)]
        and the process continue until we meet some C s.t. C.start > A.end, we have to make a new interval

        denote n := len(intervals)
        Time Complexity : O(n*logn) for sorting, and the merge takes one pass O(n)
        Space Complexity : O(logn) for stack height in sorting, or if not allowed sort in place, need extra O(n)
        """
        # sorting in ascending starting time
        intervals.sort(key=lambda x: x[0])
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
