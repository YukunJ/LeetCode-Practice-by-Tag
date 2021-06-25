**787. Cheapest Flights Within K Stops**

```Tag : Bellman-Ford/Dijkstra/Graph```

**Description:**
There are ```n``` cities connected by some number of flights. You are given an array ```flights``` where ```flights[i] = [fromi, toi, pricei]``` indicates that there is a flight from city ```fromi``` to city ```toi``` with cost ```pricei```.

You are also given three integers ```src```, ```dst```, and ```k```, return the cheapest price from src to dst with at most ```k``` stops. If there is no such route, return ```-1```.

**Notes**:

+ There will not be any multiple flights between two cities.
+ ```fromi``` != ```toi```
+ 
**Example1**:

![avatar](Fig/787-E1.png)

        Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
        Output: 200
        Explanation: The graph is shown.
        The cheapest price from city 0 to city 2 with at most 1 stop costs 200, as marked red in the picture.

**Example2**:

![avatar](Fig/787-E2.png)

        Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 0
        Output: 500
        Explanation: The graph is shown.
        The cheapest price from city 0 to city 2 with at most 0 stop costs 500, as marked blue in the picture.

        
-----------

```python
class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:
        """
        This is a shortest distance problem
        we use Bellman-Ford algorithm to solve it
        
        denote m := len(flights) = number of edges in the graph
        Time Complexity : O(k * m)
        Space Complexity : O(n)
        ""
        dist = [float('inf')] * n
        dist[src] = 0 # init the self-distance be zero
        
        # Bellman-Ford algorithm
        for relax in range(k+1):
            # at most k stops means can take at most k+1 steps
            # need a copy of previous step distance here
            # otherwise, you might actually take "multiple steps" in "one step"
            dist_last = [d for d in dist]
            for s, d, p in flights:
                dist[d] = min(dist[d], dist_last[s]+p)
                
        return dist[dst] if dist[dst] != float('inf') else -1
```
