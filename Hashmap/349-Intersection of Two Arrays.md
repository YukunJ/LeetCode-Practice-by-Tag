**349. Intersection of Two Arrays**

```Tag : hashtable/two pointers/array```

**Description:**

Given two integer arrays ```nums1``` and ```nums2```, return an array of *their intersection*. Each element in the result must be **unique** and you may return the result in any order.

**Example1:**

        Input: nums1 = [1,2,2,1], nums2 = [2,2]
        Output: [2]
        
**Example2:**

        Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
        Output: [9,4]
        Explanation: [4,9] is also accepted.

-----------

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        """
        a naive approach would take O(n*m) time complexity
        we uilitize the O(1) access time for hashset to solve this problem
        we solve by converting two arrays into two hashsets
        and iterate over the smaller one to check for membership in another array
        
        denote n := len(nums1), m := len(nums2)
        Time Complexity : O(n+m) pass-through each array
        Space Complexity : O(n+m) set storage
        """
        if len(nums1) > len(nums2):
            # make sure nums1 is the smaller array
            nums1, nums2 = nums2, nums1
            
        result = []
        reference_set = set(nums2)
        for element in set(nums1):
            if element in reference_set:
                result.append(element)
        return result
```
