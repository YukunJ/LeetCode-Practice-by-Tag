**630. Course Schedule III**

```Tag : greedy/heap/array```

**Description:**

There are ```n``` different online courses numbered from ```1``` to ```n```. You are given an array ```courses``` where ```courses[i] = [duration_i, lastDay_i]``` indicate that the ```i-th``` course should be taken **continuously** for ```duration_i``` days and must be finished before or on ```lastDay_i```.

You will start on the ```1st``` day and you cannot take two or more courses simultaneously.

Return the *maximum number of courses* that you can take.


**Example1:**

        Input: courses = [[100,200],[200,1300],[1000,1250],[2000,3200]]
        Output: 3
        Explanation: 
        There are totally 4 courses, but you can take 3 courses at most:
        First, take the 1st course, it costs 100 days so you will finish it on the 100th day, and ready to take the next course on the 101st day.
        Second, take the 3rd course, it costs 1000 days so you will finish it on the 1100th day, and ready to take the next course on the 1101st day. 
        Third, take the 2nd course, it costs 200 days so you will finish it on the 1300th day. 
        The 4th course cannot be taken now, since you will finish it on the 3300th day, which exceeds the closed date.
        
**Example2:**

        Input: courses = [[1,2]]
        Output: 1
              
**Example3:**

        Input: courses = [[3,2],[4,3]]
        Output: 0

**Hints**:

+ During iteration, say I want to add the current course, currentTotalTime being total time of all courses taken till now, but adding the current course might exceed my deadline or it doesn’t.

+ 1. If it doesn’t, then I have added one new course. Increment the currentTotalTime with duration of current course.

+ 2. If it exceeds deadline, I can swap current course with current courses that has biggest duration.

    + No harm done and I might have just reduced the currentTotalTime, right? 
    
    + What preprocessing do I need to do on my course processing order so that this swap is always legal?

-----------

**Solution1: Greedy + Recursion with Memorization (TLE 68/97)**

```python
class Solution:
    def scheduleCourse(self, courses: List[List[int]]) -> int:
        """
        First of all, we present a recursion with memorization solution
        By making an example of two courses, we might observe that
        it's always more beneficial take the course with closer deadline
        we will emphasize the "Greediness" underlying all of our approaches
        take an example of course A duration a ending x, course B duration b ending y
        say x < y, course A has a closer deadline than course B
        1) if a + b < x, it doesn't matter the order you take
        2) if x < a + b < y, a < x
           #1 if take course A first, a < x and a+b < y ensures we take both courses
           #2 if take course B first, since a+b > x, we would not be able to take course A
        This briefly shows that we greedily take the 'deadline-closer' course if possible
        Therefore the first thing we do is to sort the coruses based on deadline
        Then we initiate a procedure just like dynamic programming
        where recur(idx, time) means the maximum number of courses can take
        starting from courses["idx"] (take it or not) when the timestamp is "time" already
        we can 2 choices at this step
        1) not take this course, go to recur(idx+1, time)
        2) if time+duration <= deadline, go to 1 + recur(idx+1, time+duration)
        and we take the max[1), 2)] to get the solution
        To avoid redunancy computation, we will use memorization
        
        denote n := len(courses), m := the farthest deadline
        Time Complexity: O(n*max(logn, m)) initial sorting takes O(n*logn)
        Space Complexity : O(n*m)
        """
        from functools import lru_cache
        
        @lru_cache(maxsize=None)
        def recur(idx: int, time: int) -> int:
            if idx == len(courses):
                return 0
            
            not_take = recur(idx+1, time)
            take = 0
            if time + courses[idx][0] <= courses[idx][1]:
                # a valid duration, could be taken
                take = 1 + recur(idx+1, time+courses[idx][0])
            return max(take, not_take)
        
        courses.sort(key=lambda x : x[1])
        return recur(0, 0)
```

-----------

**Solution2: Greedy + Heap**

```python
class Solution:
    def scheduleCourse(self, courses: List[List[int]]) -> int:
        """
        another approach is to similar to Longest increasing subsequence question
        we will keep track of the max length of courses taken so far and the timestamp
        when encounter a new course, we have 2 cases:
        1) if duration + timestamp <= deadline, add this course and update the timestamp
        2) if not, try to replace this course with the max duration courses already taken
        to faciliate the process of "finding the max duration courses taken so far", we use heap data structure
        
        denote n := len(courses)
        Time Complexity: O(n*logn) initial sorting takes O(n*logn), at most n heap operation each takes O(logn)
        Space Complexity : O(n) for heap storage
        """
        import heapq
        heap = []
        count = 0
        timestamp = 0
        courses.sort(key=lambda x : x[1])
        
        for course in courses:
            duration, deadline = course
            if timestamp + duration <= deadline:
                # could successfully fit in current course plan
                timestamp += duration
                count += 1
                heapq.heappush(heap, -duration)
            else:
                # try to replace a long duration course already taken
                if not heap or -heap[0] <= duration:
                    continue
                old_duration = -heapq.heappushpop(heap, -duration)
                timestamp -= (old_duration - duration) # we earn some extra time in course planning
                
        return count
```
