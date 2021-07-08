**123. Best Time to Buy and Sell Stock III**

```Tag: dynamic programming```

**Description:**

You are given an array ```prices``` where ```prices[i]``` is the price of a given stock on the ```i-th``` day.

Find the maximum profit you can achieve. You may complete **at most two transactions**.

**Note**: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example1**:

        Input: prices = [3,3,5,0,0,3,1,4]
        Output: 6
        Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
        Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
        
**Example2**:

        Input: prices = [1,2,3,4,5]
        Output: 4
        Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
        Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.
        
**Example3**:

        Input: prices = [7,6,4,3,1]
        Output: 0
        Explanation: In this case, no transaction is done, i.e. max profit = 0.

**Example4**:

        Input: prices = [1]
        Output: 0
        
-----------

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        """
        We use dynamic programming to consider 5 states:
        1) No action at all
        2) Buy a stock
        3) Buy already and sell a stock
        4) Buy and sell a stock areadly, buy second stock
        5) Buy and sell a stack and buy second stock already, sell the second stock
        
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        buy1, buy2 = -prices[0], -prices[0]
        sell1, sell2 = 0, 0
        
        for i in range(1, len(prices)):
            p = prices[i]
            buy1 = max(buy1, -p) # buy the current stock
            sell1 = max(sell1, buy1 + p) # sell at current price
            buy2 = max(buy2, sell1 - p) # buy the current stock
            sell2 = max(sell2, buy2 + p) # sell at current price
        
        return max(0, sell1, sell2)
```
