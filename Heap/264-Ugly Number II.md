**264. Ugly Number II**

```Tag : heap/math/recursion```

**Description:**

An **ugly number** is a positive integer whose prime factors are limited to ```2```, ```3```, and ```5```.

Given an integer ```n```, return the ```n-th``` **ugly number**.

**Example1:**

        Input: n = 10
        Output: 12
        Explanation: [1, 2, 3, 4, 5, 6, 8, 9, 10, 12] is the sequence of the first 10 ugly numbers.

**Example2:**

        Input: n = 1
        Output: 1
        Explanation: 1 has no prime factors, therefore all of its prime factors are limited to 2, 3, and 5.

-----------

```python
class Solution:
    def nthUglyNumber(self, n: int) -> int:
        """
        Based on the hint, the most important observation is
        given the first k-th ugly number U_i, the next k+1 -th ugly number
        must be among min(min(U_i * 2, U_i * 3, U_i * 5)) for 1 <= i <= k
        We use a priority queue to fast retrieve the current smallest unprocessed ugly number
        
        Time Complexity : O(n * logn)
        Space Complexity : O(n)
        """
        import heapq
        myheap = [1]
        ordered_ugly = [1]
        visited = set()
        for i in range(n-1):
            curr_ugly = heapq.heappop(myheap)
            for factor in [2, 3, 5]:
                if curr_ugly*factor not in visited:
                    visited.add(curr_ugly*factor)
                    heapq.heappush(myheap, curr_ugly*factor)
        return heapq.heappop(myheap)
```
