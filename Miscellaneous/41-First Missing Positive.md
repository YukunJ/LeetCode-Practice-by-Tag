**41. First Missing Positive**

```Tag : swap```

**Description:**

Given an unsorted integer array ```nums```, find the smallest missing positive integer.

You must implement an algorithm that runs in ```O(n)``` time and uses **constant extra space**.

**Example1**:

        Input: nums = [1,2,0]
        Output: 3
        
**Example2**:      

        Input: nums = [3,4,-1,1]
        Output: 2
 
 **Example3**:      

        Input: nums = [7,8,9,11,12]
        Output: 1
         
 **Hints:**
 
 + Think about how you would solve the problem in non-constant space. Can you apply that logic to the existing space?
 
 + We don't care about duplicates or non-positive integers
 
 + Remember that ```O(2n)``` = ```O(n)```
 
-----------

```python
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        """
        In order to achieve the required O(n) time complexity and O(1) space complexity
        We need to modify the array nums in place
        for an array of length n, in best case all positive integer [1, n] appears
        then we can just return n+1, otherwise some number in [1, n] must be missing
        we can place a number x in [1, n] to its proper index x-1
        then we iterate the array, the first index i not satisfying nums[i-1]=i is answer
        
        denote n := len(nums)
        Time Complexity : O(n)     
        Space Complexity : O(1)
        """
        def _swap(i: int, j: int) -> None:
            """Helper function: swap two index's elements"""
            nums[i], nums[j] = nums[j], nums[i]
        
        n = len(nums)
        for i in range(n):
            while 1 <= nums[i] <= n and nums[i] != nums[nums[i]-1]:
                # place x = nums[i] to index x-1
                _swap(i, nums[i]-1)
                
        for i in range(n):
            # find the first not satisfying nums[i] = i+1
            if nums[i] != i+1:
                return i+1
            
        # all [1, n] inclued, return n+1
        return n+1
```
