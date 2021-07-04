**334. Increasing Triplet Subsequence**

```Tag: greedy```

**Description:**

Given an integer array ```nums```, return ```true``` if there exists a triple of indices ```(i, j, k)``` such that ```i < j < k``` and ```nums[i] < nums[j] < nums[k]```. If no such indices exists, return ```false```.

**Follow up**: Could you implement a solution that runs in ```O(n)``` time complexity and ```O(1)``` space complexity?

**Example1**:

		Input: nums = [1,2,3,4,5]
		Output: true
		Explanation: Any triplet where i < j < k is valid.
        
**Example2**:

		Input: nums = [5,4,3,2,1]
		Output: false
		Explanation: No triplet exists.
        
**Example3**:

		Input: nums = [2,1,5,0,4,6]
		Output: true
		Explanation: The triplet (3, 4, 5) is valid because nums[3] == 0 < nums[4] == 4 < nums[5] == 6.

-----------


```python
class Solution:
    def increasingTriplet(self, nums: List[int]) -> bool:
        """
        This is a greedy algorithm problem:
        the greediness lies in that to find (i, j, k) s.t nums[i] < nums[j] < nums[k]
        we want nums[i] as small as possible, to leave the most "possibility" for future j, k
        and condition of nums[i] < nums[j], we also want nums[j] as small as possible for future k
        we iterate through index of the nums, and try to decide
        if there are already i, j s.t. nums[i] < nums[j] and here nums[index] > nums[j] then we are done
        if not, we try to update i and j 
        if nums[index] smaller than nums[i], we updates i
        if nums[index] > nums[i], we try to update j if nums[index] < nums[j]
        
        In a word, when at an index, we look at all elements preceding it, and want to main a pair of
        (i, j) such that nums[j] is as small as possible as long as there is a i < j s.t. nums[i] < nums[j]
        
        denote n := len(nums)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        first, second = float('inf'), float('inf')
        for num in nums:
            if num <= first:
                first = num
            elif num <= second:
                second = num
            else:
                return True
        return False
```
