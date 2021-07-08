**403. Frog Jump**

```Tag: dynamic programming```

**Description:**

A frog is crossing a river. The river is divided into some number of units, and at each unit, there may or may not exist a stone. The frog can jump on a stone, but it must not jump into the water.

Given a list of stones' positions (in units) in **sorted ascending order**, determine if the frog can cross the river by landing on the last stone. Initially, the frog is on the first stone and assumes the first jump must be 1 unit.

If the frog's last jump was k units, its next jump must be either k - 1, k, or k + 1 units. The frog can only jump in the forward direction.

**Example1**:

        Input: stones = [0,1,3,5,6,8,12,17]
        Output: true
        Explanation: The frog can jump to the last stone by jumping 1 unit to the 2nd stone, then 2 units to the 3rd stone, then 2 units to the 4th stone, then 3 units to the 6th stone, 4 units to the 7th stone, and 5 units to the 8th stone.

**Example2**:

        Input: stones = [0,1,2,3,4,8,9,11]
        Output: false
        Explanation: There is no way to jump to the last stone as the gap between the 5th and 6th stone is too large.

-----------

```python
class Solution:
    def canCross(self, stones: List[int]) -> bool:
        """
        This is a hard dynamic programming problem
        In state transition, 
            we not only condition on the last stone's position
            but also need to know the last jump step
        denote dp[i][k] := if can get to stone #i with last step k
        the based on the jump rule, we could get the state transition
        dp[i][k] := dp[j][k] && stone[i]-stone[j] == k
                    | dp[j][k+1] && stone[i]-stone[j] == k
                    | dp[j][k-1] && stone[i]-stone[j] == k
                    for some j < i
        An optimization trick is to observe that, 
        the last step to get to stone # i is at most i,
        i.e. ideally, you jump 1 to #1, 2 to #2, ..., i to #i,
        so if stone[i] - stone[i-1] > i, the frog for sure cannot reach
        so the 2nd dimension, k, for our dp matrix, could be set to len(stones)
        
        denote n:= len(stones)
        Time Complexity : O(n^2) for nested loop
        Space Complexity : O(n^2) for matrix storage
        """
        
        # Optimization: to reach index i stone, last jump at most i
        n = len(stones)
        for i in range(n-1):
            if stones[i+1] - stones[i] > i+1:
                return False
        
        # init the dp matrix
        dp = [[False] * (n+1) for _ in range(n)]
        dp[0][0] = True
        
        for i in range(1, n):
            for j in range(i-1, -1, -1): # look at previous stones
                d = stones[i] - stones[j]
                if d > j + 1:
                    # when on stones j, the next jump is at most j+1
                    break 
                dp[i][d] = dp[j][d-1] | dp[j][d] | dp[j][d+1]
                if i == n-1 and dp[i][d]:
                    return True

        return False
```
