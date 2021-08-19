**986. Interval List Intersections**

```Tag : array/two pointers```

**Description:**

You are given two lists of closed intervals, ```firstList``` and ```secondList```, where ```firstList[i] = [starti, endi]``` and ```secondList[j] = [startj, endj]```. Each list of intervals is pairwise **disjoint** and in **sorted order**.

Return the intersection of these two interval lists.

A **closed interval** ```[a, b]``` (with ```a < b```) denotes the set of real numbers ```x``` with ```a <= x <= b```.

The **intersection** of two closed intervals is a set of real numbers that are either empty or represented as a closed interval. For example, the intersection of ```[1, 3]``` and ```[2, 4]``` is ```[2, 3]```.


**Example1:**

![avatar](Fig/986-E1.png)

		Input: firstList = [[0,2],[5,10],[13,23],[24,25]], secondList = [[1,5],[8,12],[15,24],[25,26]]
		Output: [[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]

**Example2:**

		Input: firstList = [[1,3],[5,9]], secondList = []
		Output: []

**Example3:**

		Input: firstList = [], secondList = [[4,8],[10,12]]
		Output: []

**Example4:**

		Input: firstList = [[1,7]], secondList = [[3,10]]
		Output: [[3,7]]

-----------

```python
class Solution:
    def intervalIntersection(self, firstList: List[List[int]], secondList: List[List[int]]) -> List[List[int]]:
        """
        
        The criteria for two interval [s1, e1] and [s2, e2] to have intersection is that:
            s1 <= e2 and s1 <= e1 (image two window sliding towards each other and pass away)
        and we are given firstList and secondList are disjoint by themselves. 
        Then the only intersection is some interval A in firstList and some interval B in secondList
        notice that for two interval, the one with smaller endpoint cannot further intersect the next one in the other list
        therefore we may remove the interval with smaller endpoint, sliding forward
        
        denote m := len(firstList), n := len(secondList)
        Time Complexity : O(m+n)
        Space Complexity : O(1)
        """
        res = []
        m, n = len(firstList), len(secondList)
        ptr1, ptr2 = 0, 0
        while ptr1 < m and ptr2 < n:
            insec_start = max(firstList[ptr1][0], secondList[ptr2][0])
            insec_end = min(firstList[ptr1][1], secondList[ptr2][1])
            if insec_start <= insec_end:
                # a valid intersection interval found
                res.append([insec_start, insec_end])
            
            if firstList[ptr1][1] < secondList[ptr2][1]:
                ptr1 += 1
            elif firstList[ptr1][1] > secondList[ptr2][1]:
                ptr2 += 1
            else: # if equal, both eliminated
                ptr1 += 1
                ptr2 += 1
       
        return res
```