**238. Product of Array Except Self**

```Tag: array/math/two pointers```

**Description:**

Given an integer array ```nums```, return an array ```answer``` such that ```answer[i]``` is equal to the product of all the elements of ```nums``` except ```nums[i]```.

The product of any prefix or suffix of ```nums``` is **guaranteed** to fit in a 32-bit integer.

You must write an algorithm that runs in ```O(n)``` time and without using the division operation.

**Follow up**: Can you solve the problem in ```O(1)``` extra space complexity? (The output array **does not** count as extra space for space complexity analysis.)

**Example1**:

        Input: nums = [1,2,3,4]
        Output: [24,12,8,6]

**Example2**:

        Input: nums = [-1,1,0,-3,3]
        Output: [0,0,9,0,0]

-----------

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        """
        First of all, for every index i, we know the answer should be
            ans[i] = prod(nums[0:i]) * prod(nums[i+1:])
        however, in this way each index takes O(n), in total this algorithm is O(n^2)
        we could keep two arrays of prefix product Left and suffix product Right
        and ans[i] = Left[i] * Right[i], 
            i.e. all i's left elements product and i's right elements product
        However, this would result in O(n) space complexity
        In order to get the O(1) space complexity optimization,
        we utilize the ans result to first store Left array
            and dynamically create Right array
        
        denote n := len(nums)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        n = len(nums)
        ans = [1] * n
        # create the prefix product array "Left"
        for i in range(1, n):
            ans[i] = ans[i-1] * nums[i-1]
        # dynamically create "Right" and multiply to answer array
        right = nums[n-1]
        for i in range(n-2, -1, -1):
            ans[i] *= right
            right *= nums[i]
        
        return ans
```
