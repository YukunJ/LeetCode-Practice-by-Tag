**1094. Car Pooling**

```Tag : sorting/array/simulation```

**Description:**

There is a car with ```capacity``` empty seats. The vehicle only drives east (i.e., it cannot turn around and drive west).

You are given the integer ```capacity``` and an array ```trips``` where ```trip[i] = [numPassengersi, fromi, toi]``` indicates that the ```i-th``` trip has ```numPassengersi``` passengers and the locations to pick them up and drop them off are ```fromi``` and ```toi``` respectively. The locations are given as the number of kilometers due east from the car's initial location.

Return ```true``` if it is possible to pick up and drop off all passengers for all the given trips, or ```false``` otherwise.

**Example1:**

		Input: trips = [[2,1,5],[3,3,7]], capacity = 4
		Output: false

**Example2:**

		Input: trips = [[2,1,5],[3,3,7]], capacity = 5
		Output: true

**Example3:**

		Input: trips = [[2,1,5],[3,5,7]], capacity = 3
		Output: true

**Example4:**

		Input: trips = [[3,2,7],[3,7,9],[8,3,9]], capacity = 11
		Output: true

-----------

```python
class Solution:
    def carPooling(self, trips: List[List[int]], capacity: int) -> bool:
        """
        We do a simulation based on the time stamp
        for pickup, we increment the people on bus (+)
        for dropping, we decrease the people on bus (-)
        and we make sure that in the same timestamp, we first drop then pick up
        
        denote n := len(trips)
        Time Complexity : O(nlogn) mostly for sorting
        Space Complexity : O(n)
        """
        timestamp = []
        for people_num, pick, drop in trips:
            timestamp.append((pick, people_num))
            timestamp.append((drop, -people_num))
        timestamp.sort() # sort based on timestamp, then first drop then pick
        curr_num = 0
        for time, num in timestamp:
            curr_num += num
            if curr_num > capacity:
                return False
        return True
```
