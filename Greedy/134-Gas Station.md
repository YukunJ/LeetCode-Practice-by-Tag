**134. Gas Station**

```Tag: greedy/math```

**Description:**

There are ```n``` gas stations along a circular route, where the amount of gas at the ```i-th``` station is ```gas[i]```.

You have a car with an unlimited gas tank and it costs ```cost[i]``` of gas to travel from the ```i-th``` station to its next ```(i + 1)-th``` station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays ```gas``` and ```cost```, return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return ```-1```. If there exists a solution, it is **guaranteed** to be **unique**

**Example1**:

        Input: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
        Output: 3
        Explanation:
        Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
        Travel to station 4. Your tank = 4 - 1 + 5 = 8
        Travel to station 0. Your tank = 8 - 2 + 1 = 7
        Travel to station 1. Your tank = 7 - 3 + 2 = 6
        Travel to station 2. Your tank = 6 - 4 + 3 = 5
        Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
        Therefore, return 3 as the starting index.
        
**Example2**:

        Input: gas = [2,3,4], cost = [3,4,3]
        Output: -1
        Explanation:
        You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
        Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
        Travel to station 0. Your tank = 4 - 3 + 2 = 3
        Travel to station 1. Your tank = 3 - 3 + 3 = 3
        You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
        Therefore, you can't travel around the circuit once no matter where you start.
        

-----------

```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        """
        A brute force solution would be to try every starting index to see
            if you could traverl a whole circle, which takes O(n^2)
        However, this is a greedy algorithm question actually
            if there is a failure from x station to y station, it means
                sum of gas[x:y+1] < sum of cost[x:y+1] (*1)
            and any middle station j is s.t. x station could go to j
                sum of gas[x:j+1] > sum of cost[x:j+1] (*2)
            from here we deduce that using (*1) - (*2)
                sum of gas[j:y+1] < sum cost [j:y+1], so j station could also not reach y
            so we can skip these station and start next search at y
        
        denote n := len(gas)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        if sum(gas) < sum(cost): # safeguard checking
            return -1
        
        start_idx, n = 0, len(gas)
        while start_idx < n:
            # try start with start_idx
            if gas[start_idx] < cost[start_idx]:
                # failure at first step
                start_idx += 1
            else:
                curr_gas = gas[start_idx] - cost[start_idx]
                ptr = start_idx + 1
                while (ptr % n != start_idx):
                    # try to go a whole circle 
                    curr_gas += (gas[ptr % n] - cost[ptr % n])
                    if curr_gas < 0:
                        break
                    ptr += 1
                if ptr % n == start_idx:
                    return start_idx
                else:
                    # if idx > n already, this is indicator of overall failure already
                    start_idx = ptr
        return -1
```
