**456. 132 Pattern**

```Tag: monotone stack/sorting```

**Description:**

Given an array of ```n``` integers ```nums```, a **132 pattern** is a subsequence of three integers ```nums[i], nums[j]``` and ```nums[k]``` such that ```i < j < k``` and ```nums[i] < nums[k] < nums[j]```.

Return ```true``` if there is a **132 pattern** in ```nums```, otherwise, return ```false```.

**Example1**:

        Input: nums = [1,2,3,4]
        Output: false
        Explanation: There is no 132 pattern in the sequence.

**Example2**:

        Input: nums = [3,1,4,2]
        Output: true
        Explanation: There is a 132 pattern in the sequence: [1, 4, 2].
        
**Example3**:

        Input: nums = [-1,3,2,0]
        Output: true
        Explanation: There are three 132 patterns in the sequence: [-1, 3, 2], [-1, 3, 0] and [-1, 2, 0].

-----------

```python
class Solution:
    def find132pattern(self, nums: List[int]) -> bool:
        """
        We use monotone stack approach
        while we iterate through the list, we main a stack that's decreasing from bottom to top to maintain "2"
        when we encounter a new element nums[index], we consider the following scerario:
        1) could this element helps to update the "2", we want the "2" as large as possible as long as there is a "3" bigger than it,
        here if nums[index] > heap[-1], we found the "3" here, we can update the "2_max" to stack[-1] and pop it from the stack
        2) is there a solution found? if nums[index] < "2_max", means we found a "132" already, 
            because the presence of "2_max" implicitly indicates there is a "3"
        3) we add the nums[index] to the stack as a candidate for "2_max"
        
        denote n := len(nums)
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        n = len(nums)
        if n < 3: # boundary case
            return False
        mono_stack = [nums[n-1]]
        two_max = float('-inf')
        for i in range(n-2, -1, -1): # traversal the list backward
            if nums[i] < two_max: # answer found
                return True
            while mono_stack and nums[i] > mono_stack[-1]:
                two_max = mono_stack[-1]
                mono_stack.pop()
            if nums[i] > two_max:
                # optimization
                # append the number only if it might possibly update the two_max to a lagre number
                mono_stack.append(nums[i])
        
        return False
```
