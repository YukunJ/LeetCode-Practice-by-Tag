**621. Task Scheduler**

```Tag: greedy/hashtable```

**Description:**

Given a characters array ```tasks```, representing the tasks a CPU needs to do, where each letter represents a different task. Tasks could be done in any order. Each task is done in one unit of time. For each unit of time, the CPU could complete either one task or just be idle.

However, there is a non-negative integer ```n``` that represents the cooldown period between two **same tasks** (the same letter in the array), that is that there must be at least ```n``` units of time between any two same tasks.

Return the *least number of units of times* that the CPU will take to finish all the given tasks.

**Example1**:

        Input: tasks = ["A","A","A","B","B","B"], n = 2
        Output: 8
        Explanation: 
        A -> B -> idle -> A -> B -> idle -> A -> B
        There is at least 2 units of time between any two same tasks.
        
**Example2**:

        Input: tasks = ["A","A","A","B","B","B"], n = 0
        Output: 6
        Explanation: On this case any permutation of size 6 would work since n = 0.
        ["A","A","A","B","B","B"]
        ["A","B","A","B","A","B"]
        ["B","B","B","A","A","A"]
        ...
        And so on.
        
**Example3**:

        Input: tasks = ["A","A","A","A","A","A","B","C","D","E","F","G"], n = 2
        Output: 16
        Explanation: 
        One possible solution is
        A -> B -> C -> A -> D -> E -> A -> F -> G -> A -> idle -> idle -> A -> idle -> idle -> A


-----------

```python
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        """
        This is a greedy algorithm problem
        Inituitively we want to prioritize the job with highest frequency
        so that when it's "cooling down", we have a better chance to find another job to do instead of being idle
        And this is the greediness here and it's mathematically correct
        If there is a job A and job B, A has higher frequency than B.
        At that time when the greedy algorithm tells us to do A first,
        if there is another optimal solution that do B first,
        we can swap the order of doing following A and B and finish the job at the same time
        Therefore, the greedy solution is also a optimal solution
        
        denote n := len(tasks), since tasks are characterize by 26 characters
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        from collections import Counter
        frequency = Counter(tasks)
        
        m = len(frequency) # number of tasks
        nextValid = [1] * m # next valid time to be done after cooling
        remain = list(frequency.values()) # the remaining frequency of this job
        
        time = 0
        for do_job in range(len(tasks)):
            # we will finish len(tasks) jobs overall
            
            # skip sometime if all are "cooling down" and we have to be idle
            time += 1
            minNextValid = min([nextValid[i] for i in range(m) if remain[i] > 0])
            time = max(time, minNextValid)
            
            # find the doable one with highest frequency
            # linear search here is OK since at most 26 tasks to be checked
            best = -1
            for i in range(m):
                if remain[i] > 0 and nextValid[i] <= time:
                    if best == -1 or remain[i] > remain[best]:
                        best = i
                        
            nextValid[best] = time + n + 1 # update cooling time
            remain[best] -= 1 # reduce frequency by 1
            
        return time
```
