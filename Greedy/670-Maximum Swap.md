**670. Maximum Swap**

```Tag : greedy/math```

**Description:**

You are given an integer ```num```. You can swap two digits at most once to get the maximum valued number.

Return the **maximum valued** number you can get.


**Example1:**

		Input: num = 2736
		Output: 7236
		Explanation: Swap the number 2 and the number 7.

**Example2:**

		Input: num = 9973
		Output: 9973
		Explanation: No swap.

-----------

```python
class Solution:
    def maximumSwap(self, num: int) -> int:
        """
        we adopt a greedy approach in this question.
        essentially, we want to swap the biggest number to the highest digit
        In doing so, we will record the last appearance index of every number in 0-9
        and we traverl from high digits to low digits to check if a swap is possible
        why we need the last appearance? see the following example
        1 9_1 9_2
        if we swap 1 <-> 9_2: 9_2 9_1 1 = 991
        if we swap 1 <-> 9_1: 9_1 1 9_2 = 919
        and 991 > 919 
        This example should be enough to exemplify the greediness
        
        denote n := len(num)
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        num_lst = list(str(num))
        last_appear = [-1 for _ in range(10)]
        for idx, num_str in enumerate(num_lst):
            # update the last appearance index
            last_appear[int(num_str)] = idx
        for idx, num_str in enumerate(num_lst):
            # try swap, starting from the higher digits
            for small in range(9, int(num_str), -1):
                # greedy, search possible swap number from high to low
                if last_appear[small] > idx:
                    num_lst[idx], num_lst[last_appear[small]] = \
                        num_lst[last_appear[small]], num_lst[idx]
                    return int("".join(num_lst))
        # reach here means no need to swap
        return num
```
