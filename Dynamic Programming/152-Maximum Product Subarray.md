**152. Maximum Product Subarray**

```Tag: dynamic programming```

**Description:**

Given an integer array ```nums```, find a contiguous non-empty subarray within the array that has the largest product, and return the product.

It is **guaranteed** that the answer will fit in a **32-bit** integer.

A **subarray** is a contiguous subsequence of the array.

**Example1**:

        Input: nums = [2,3,-2,4]
        Output: 6
        Explanation: [2,3] has the largest product 6.

**Example2**:

        Input: nums = [-2,0,-1]
        Output: 0
        Explanation: The result cannot be 2, because [-2,-1] is not a subarray.

-----------

```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        """
        This is a dynamic programming problem.
        The key observation is that "negative * negative = positive"
        so the smallest prod might turn into the largest by "flipping the sign"
        In this sense, we will track both the "largest prod" so far, but also "smallest prod"
        denote M[i] := the largest sub-array product ending with element nums[i]
        denote m[i] := the smallest sub-array product ending with element nums[i]
        then, the transition state equation write like:
        M[i+1] := max(nums[i+1], nums[i+1] * M[i](if positive), nums[i+1] * m[i] (if negative))
        m[i+1] := min(nums[i+1], nums[i+1] * M[i](if negative), nums[i+1] * m[i] (if positive))
        And we may update along the way to save the space storage
        
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        n = len(nums)
        globalM = M = m = nums[0]
        for i in range(1, n):
            M, m = max(nums[i], nums[i] * M, nums[i] * m), min(nums[i], nums[i] * M, nums[i] * m)
            globalM = max(globalM, M)
            
        return globalM
```
