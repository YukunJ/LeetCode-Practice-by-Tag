**122. Best Time to Buy and Sell Stock II**

```Tag: dynamic programming```

**Description:**

You are given an array ```prices``` where ```prices[i]``` is the price of a given stock on the ```i-th``` day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

**Note**: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example1**:

        Input: prices = [7,1,5,3,6,4]
        Output: 7
        Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
        Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
        
**Example2**:

        Input: prices = [1,2,3,4,5]
        Output: 4
        Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
        Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.
        
**Example3**:

        Input: prices = [7,6,4,3,1]
        Output: 0
        Explanation: In this case, no transaction is done, i.e., max profit = 0.

-----------

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        """
        use dynmaic programming
        In theory, we want to find the longest "increasing" stock price sequence
        and buy at lowest and sell at highest
        
        However, we may adopt a greedy approach that,
        as long as next stock price is increasing, 
            we buy current one and sell at next time
        for example, if there is a price sequence 1 -> 3 -> 5 -> 7
        instead of try to get a (7-1),
        we do a (3-1) + (5-3) + (7-5), which yields the same profits
        
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        profit = 0
        last_price = float('inf') # init null step to infinity
        for price in prices:
            if price > last_price:
                # profitable, execute immediately
                profit += (price - last_price)
            last_price = price
        return profit
```
