**518. Coin Change 2**

```Tag : dynamic programming/array```

**Description:**

You are given an integer array ```coins``` representing coins of different denominations and an integer ```amount``` representing a total amount of money.

Return the *number of combinations* that make up that amount. If that amount of money cannot be made up by any combination of the coins, return ```0```.

You may assume that you have an infinite number of each kind of coin.

The answer is **guaranteed** to fit into a signed **32-bit** integer.

**Example1:**

        Input: amount = 5, coins = [1,2,5]
        Output: 4
        Explanation: there are four ways to make up the amount:
        5=5
        5=2+2+1
        5=2+1+1+1
        5=1+1+1+1+1

**Example2:**

        Input: amount = 3, coins = [2]
        Output: 0
        Explanation: the amount of 3 cannot be made up just with coins of 2.

**Example3:**

        Input: amount = 10, coins = [10]
        Output: 1

-----------

```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        """
        We adopt a dynamic programming approach to this problem
        To initialize, zero amount means there is only 1 approach : no coin picked
        then, we define dp[i] := the possible way to make up i amount of money using coins
        Then we discover that for each coin, dp[i] contributes to dp[i+coin] by dp[i] amount
        
        denote n := len(coins)
        Time Complexity : O(n * amount)
        Space Complexity : O(amount)
        """
        dp = [0] * (1 + amount)
        dp[0] = 1 # zero amount only needs to pick 0 coin
        # order in the for loop here is important
        # don't switch the 2 for loops, otherwise it's permutation
        for coin in coins:
            for i in range(coin, amount+1):
                dp[i] += dp[i-coin]
        return dp[-1]
```

