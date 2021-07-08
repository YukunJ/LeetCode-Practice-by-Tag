**322. Coin Change**

```Tag: dynamic programming```

**Description:**


You are given an integer array ```coins``` representing coins of different denominations and an integer ```amount``` representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return ```-1```.

You may assume that you have an infinite number of each kind of coin.



**Example1**:

        Input: coins = [1,2,5], amount = 11
        Output: 3
        Explanation: 11 = 5 + 5 + 1


**Example2**:

        Input: coins = [2], amount = 3
        Output: -1

**Example3**:

        Input: coins = [1], amount = 0
        Output: 0

**Example4**:

        Input: coins = [1], amount = 1
        Output: 1

**Example5**:

        Input: coins = [1], amount = 2
        Output: 2

-----------

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        """
        This is a dynamic programming scheme
        denote dp[i] the minimal number of coins needed to achieve amount i
        sub-problem is to solve, conditioning on last coin used,
        dp[i] = min(dp[i-coin]+1) for coin in coins
        
        denote S := amount, n := len(coins)
        Time Complexity : O(S*n) nested loop
        Space Complexity : O(S) storage for the dp vector
        """
        dp = [float('inf')] * (amount+1)
        dp[0] = 0
        for coin in coins:
            for x in range(coin, amount+1):
                dp[x] = min(dp[x], dp[x-coin]+1)
                
        return dp[amount] if dp[amount] != float('inf') else -1    
```
