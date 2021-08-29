**1010. Pairs of Songs With Total Durations Divisible by 60**

```Tag : hashtable/array/counting```

**Description:**

You are given a list of songs where the ```i-th``` song has a duration of ```time[i]``` seconds.

Return the *number of pairs of songs for which their total duration in seconds is divisible by ```60```*. Formally, we want the number of indices ```i, j``` such that ```i < j``` with ```(time[i] + time[j]) % 60 == 0```.

**Example1:**

		Input: time = [30,20,150,100,40]
		Output: 3
		Explanation: Three pairs have a total duration divisible by 60:
		(time[0] = 30, time[2] = 150): total duration 180
		(time[1] = 20, time[3] = 100): total duration 120
		(time[1] = 20, time[4] = 40): total duration 60

**Example2:**

		Input: time = [60,60,60]
		Output: 3
		Explanation: All three pairs have a total duration of 120, which is divisible by 60.

-----------

```python
class Solution:
    def numPairsDivisibleBy60(self, time: List[int]) -> int:
        """
        A brute force solution would take O(n^2) here
        However we realize that we don't really care how long is a song
        but rather how many minutes it remains after taking module 60
        we use a hashtable to count the modelo remainer and update result accordingly
        
        denote n := len(time)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        from collections import defaultdict
        remainer = defaultdict(int)
        res = 0
        for num in time:
            if num % 60 == 0:
                # special case when both a % 60 = 0 and b % 60 = 0
                res += remainer[0]
            else:
                res += remainer[60-num%60]
                
            remainer[num%60] += 1 # update records
            
        return res
```
