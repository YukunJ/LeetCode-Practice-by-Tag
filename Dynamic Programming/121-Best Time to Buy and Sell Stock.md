**121. Best Time to Buy and Sell Stock**

```Tag: dynamic programming```

**Description:**

You are given an array ```prices``` where ```prices[i]``` is the price of a given stock on the ```i-th``` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return ```0```.

**Example1**:

        Input: prices = [7,1,5,3,6,4]
        Output: 5
        Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
        Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
        
**Example2**:

        Input: prices = [7,6,4,3,1]
        Output: 0
        Explanation: In this case, no transactions are done and the max profit = 0.

-----------

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        """
        We use dynamic programming to solve the problem
        we keep memory of the past "lowest" price,
        when see a new price, 
        1) if it's higher, try to update the max profit
        2) if it's lower, update the past "lowest" price
        
        Time Complexity : O(n)
        Space Complexity : (1)
        """
        profit = 0
        lowest = float('inf') # init to infinity
        for price in prices:
            if price < lowest:
                lowest = price
            else: # price >= lowest
                profit = max(profit, price - lowest)
        return profit                    
```
