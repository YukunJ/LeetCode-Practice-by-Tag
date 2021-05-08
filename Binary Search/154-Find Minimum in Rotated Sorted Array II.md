**154. Find Minimum in Rotated Sorted Array II**

```Tag: binary search```

**Description:**

Suppose an array of length ```n``` sorted in ascending order is **rotated** between ```1``` and ```n``` times. For example, the array ```nums = [0,1,2,4,5,6,7]``` might become:

    [4,5,6,7,0,1,2] if it was rotated 4 times.
    [0,1,2,4,5,6,7] if it was rotated 7 times.
    
Notice that rotating an array ```[a[0], a[1], a[2], ..., a[n-1]]``` 1 time results in the array ```[a[n-1], a[0], a[1], a[2], ..., a[n-2]]```.

Given the sorted rotated array ```nums``` that may contain **duplicates**, return the minimum element of this array.

**Follow up**: This is the same as 153. Find Minimum in Rotated Sorted Array but with duplicates. Would allow duplicates affect the run-time complexity? How and why?

**Example1**:

        Input: nums = [1,3,5]
        Output: 1

**Example2**:

        Input: nums = [2,2,2,0,1]
        Output: 0

-----------

```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        """
        This question is similar to Question.153
        Except duplicates allowed
        
        Time Complexity : O(n) in worst case, but we expect O(logn) by binary search
        Space Complexity: O(1)
        """
        l, r = 0, len(nums)-1
        while l < r:
            mid = l + (r-l)//2
            if nums[mid] == nums[r]:
                # This is the new complicated case
                # The Min might be on left-hand or right-hand side
                # we cannot make a cut-half decision in this case
                # we make r -= 1 and we will not lose answer, because
                # even if nums[r] is the min, nums[mid] equals it
                r -= 1
            elif nums[mid] > nums[r]: # right-hand is cliff, Min exists there
                l = mid + 1
            else: # nums[mid] < nums[r] # left-hand is Min
                r = mid
        return nums[l]
```
