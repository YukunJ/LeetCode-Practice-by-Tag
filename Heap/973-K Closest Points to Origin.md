**973. K Closest Points to Origin**

```Tag : Heap/Sorting```

Given an array of ```points``` where ```points[i] = [xi, yi]``` represents a point on the **X-Y** plane and an integer ```k```, return the ```k``` closest points to the origin ```(0, 0)```.

The distance between two points on the **X-Y** plane is the Euclidean distance (i.e., ```√(x1 - x2)^2 + (y1 - y2)^2)```.

You may return the answer in **any order**. The answer is **guaranteed** to be **unique** (except for the order that it is in).

**Example1**:

![avatar](Fig/973-E1.jpg)

        Input: points = [[1,3],[-2,2]], k = 1
        Output: [[-2,2]]
        Explanation:
        The distance between (1, 3) and the origin is sqrt(10).
        The distance between (-2, 2) and the origin is sqrt(8).
        Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
        We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]].

**Example2**:

        Input: points = [[3,3],[5,-1],[-2,4]], k = 2
        Output: [[3,3],[-2,4]]
        Explanation: The answer [[-2,4],[3,3]] would also be accepted.

        
-----------

```python
class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        """
        This is a typical heap filtering problem
        we only want to keep the k-closest points from origin, where k << len(points)
        so we keep a size=k max-heap, 
        only want current point's distance is smaller the heap top, 
        then add it into the heap and pop out the head
        Notice, python heapq by default is min-heap, 
        so we take negation of distance to turn it into max-heap
        
        denote n := len(points)
        Time Complexity : O(n*logk)
        Space Complexity : O(k)
        """
        def squaredist2origin(point: List[int]) -> float:
            """Helper function: compute the squared distance from origin"""
            # no need to take the square root here
            return point[0] * point[0] + point[1] * point[1] 
        
        import heapq
        heap = []
        for point in points:
            if len(heap) < k:
                heapq.heappush(heap, (-squaredist2origin(point), point))
            else:
                dist = squaredist2origin(point)
                if dist < -heap[0][0]:
                    heapq.heapreplace(heap, (-dist, point))
        
        return [point for _, point in heap]
```
