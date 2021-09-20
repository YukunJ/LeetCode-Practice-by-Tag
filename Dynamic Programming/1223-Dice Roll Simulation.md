**1223. Dice Roll Simulation**

```Tag : dynamic programming/array```

**Description:**

A die simulator generates a random number from ```1``` to ```6``` for each roll. You introduced a constraint to the generator such that it cannot roll the number i more than ```rollMax[i]``` (**1-indexed**) consecutive times.

Given an array of integers ```rollMax``` and an integer ```n```, return the number of distinct sequences that can be obtained with exact ```n``` rolls. Since the answer may be too large, return it **modulo** ```10^9 + 7```.

Two sequences are considered different if at least one element differs from each other.


**Example1:**

        Input: n = 2, rollMax = [1,1,2,2,2,3]
        Output: 34
        Explanation: There will be 2 rolls of die, if there are no constraints on the die, there are 6 * 6 = 36 possible combinations. In this case, looking at rollMax array, the numbers 1 and 2 appear at most once consecutively, therefore sequences (1,1) and (2,2) cannot occur, so the final answer is 36-2 = 34.
        
**Example2:**

        Input: n = 2, rollMax = [1,1,1,1,1,1]
        Output: 30
        
**Example3:**

        Input: n = 3, rollMax = [1,1,1,2,2,3]
        Output: 181

**Hints**:

+ Think on Dynamic Programming.

+ ```DP(pos, last)``` which means we are at the position ```pos``` having as ```last``` the last character seen.

-----------

**Solution1: 3D dynamic programming**

```python
class Solution:
    def dieSimulator(self, n: int, rollMax: List[int]) -> int:
        """
        We are tracking different ways to roll a dice so that
        number i could not be rolled more than rollMax[i] consecutively times
        (meaning if you take a rest, you could roll that number again)
        
        Following the hint, we will use dynamic programming approach to solve this problem
        denote dp[n][j][k] a 3-d dynamic programming vector
        it means, roll n times with ending j k consecutive times, how many ways we have
        then our answer is the sum of dp[n][i][j], keeping n fixed and summing over 1 <= i <=6, 1<=j<=rollMax[j]
        
        denote S := sum(rollMax)
        Time Complexity : O(n*S), in 3-d expansion it's sum of n * i * s_i, since dice size is constant, it simplifies to  O(n* sum(s_i))
        Space Complexity : O(n*S)
        """
        memory = dict() # memorization storage
        
        def recur(n: int, j: int, k: int) -> int:
            """Helper function: remaining n times rolling with ending j repeated k times"""
            nonlocal memory
            if n == 0:
                # a finished rolling, get a full sequence
                return 1
            elif (n, j, k) in memory:
                return memory[(n, j, k)]
            else:
                seq_num = 0
                for next_roll in range(6):
                    if next_roll == j:
                        if k+1 <= rollMax[j]:
                            # repeat rolling same number under limit
                            seq_num += recur(n-1, j, k+1)
                    else:
                        seq_num += recur(n-1, next_roll, 1)
                memory[(n, j, k)] = seq_num
                return seq_num
        
        return sum(recur(n-1, roll, 1) for roll in range(6)) % (10**9 + 7)
```

-----------

**Solution2: 2D dynamic programming**

```python
class Solution:
    def dieSimulator(self, n: int, rollMax: List[int]) -> int:
        """
        Built upon the previous solution, another way to program this quesion is
        we are going to throw repeative number of a single number at all once,
        and switch to another number 
        this could save the dimension of 'k' because we will only throw valid consecutive ones
        
        denote S := sum of 'rollMax' array
        Time Complexity : O(n * S) 
        Space Complexity : O(n)
        """
        from functools import lru_cache
        
        @lru_cache(maxsize=None)
        def recur(n: int, last: int) -> int:
            if n <= 0:
                # might over-shoot, be careful here
                return int(n == 0)
            else:
                seq_num = 0
                for next in range(6):
                    if next != last:
                        # change to another number to roll the dice
                        for consecutive in range(1, rollMax[next]+1):
                            seq_num += recur(n-consecutive, next)
                return seq_num
        
        # init with -1 to allow any 1-6 to be rolled at first place
        return recur(n, -1) % (10 ** 9 + 7)
```
