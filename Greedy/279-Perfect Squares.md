**279. Perfect Squares**

```Tag : greedy/dynamic programming/math```

**Description:**

Given an integer ```n```, return the *least number of perfect square numbers* that sum to ```n```.

A **perfect square** is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, ```1```, ```4```, ```9```, and ```16``` are perfect squares while ```3``` and ```11``` are not.

**Example1:**

        Input: n = 12
        Output: 3
        Explanation: 12 = 4 + 4 + 4.

**Example2:**

        Input: n = 13
        Output: 2
        Explanation: 13 = 4 + 9.

-----------

**Solution1: Dynamic Programming**

```python
class Solution:
    def numSquares(self, n: int) -> int:
        """
        denote dp[i] := least number of perfect square number that sum up to n
        then the state transition is such that:
        dp[i+1] = min(1+ dp[i+1-j^2] for integer j such that j^2 <= i)
        
        Time Complexity : O(n^1.5) in every loop, we are traversa all square number 
                          that's smaller than current number 
                          which is of magnitude O(current^0.5)
        Space Complexity : O(n) for the dp vector
        """
        square_nums = [i**2 for i in range(int(math.sqrt(n))+1)]
        
        # bottom up dynamic programming approach
        dp = [float('inf')] * (n+1)
        dp[0] = 0
        for i in range(1, n+1):
            for square in square_nums:
                if square > i: # out of bound
                    break
                dp[i] = min(dp[i], dp[i-square] + 1)
                
        return dp[n]
```

-----------

**Solution2: Greedy**

```python
class Solution:
    def numSquares(self, n: int) -> int:
        """
        We could also adopt a greedy approach
        we use to build the soluton to test starting from 1, 2, ..., big number
        The first testing returning True is going to be the solution
        
        
        Time Complexity : O(n^1.5) in the worst case, we essentially fill up the memory hashtable 
                          just like the 2-D dp vector as in solution1
                          However, from the runtime we could tell this solution is much faster
                          Because by greedy approach, once we find the optimal solution we can just stop and return
        Space Complexity : O(n^0.5) for square number hashset
        """
        from functools import lru_cache
        from collections import defaultdict
        
        @lru_cache(maxsize=None)
        def is_divided_by(n, count):
            """Helper function: test if n could be decomposed into "count" number of perfect squares"""
            nonlocal square_nums
            
            if count == 1:
                return n in square_nums
            else:
                for k in square_nums:
                    if k <= n and is_divided_by(n-k, count-1):
                        return True
            return False

        square_nums = set([i**2 for i in range(int(math.sqrt(n))+1)])
        for greedy_count in range(1, n+1):
            if is_divided_by(n, greedy_count):
                return greedy_count
```
