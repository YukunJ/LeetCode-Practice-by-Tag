**350. Intersection of Two Arrays II**

```Tag : hashtable/array/two pointers/sorting```

**Description:**

Given two integer arrays ```nums1``` and ```nums2```, return an array of their intersection. Each element in the result must appear as many times as it shows in both arrays and you may return the result in **any order**.

**Example1:**

        Input: nums1 = [1,2,2,1], nums2 = [2,2]
        Output: [2,2]

**Example2:**

        Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
        Output: [4,9]
        Explanation: [9,4] is also accepted.

**Follow up**: 

+ What if the given array is already sorted? How would you optimize your algorithm?

+ What if ```nums1```'s size is small compared to ```nums2```'s size? Which algorithm is better?

+ What if elements of ```nums2``` are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once? 

-----------

```python
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        """
        Similar to its previous question, but only that here the number might appear multiple times
        We could use a hashtable to store the counting of an array 
        and while we loop through the other array, if we find a match, we decrease the count by 1
        
        denote n, m = len(nums1), len(nums2)
        Time Complexity : O(n+m)
        Space Complexity : O(min(n, m))
        """
        from collections import defaultdict
        n, m = len(nums1), len(nums2)
        if n > m:
            # make sure nums1 is the smaller one
            nums1, nums2 = nums2, nums1
            
        # construct the counting
        counting = defaultdict(int)
        for e in nums1:
            counting[e] += 1
            
        result = []
        for e in nums2:
            if counting[e] > 0:
                result.append(e)
                counting[e] -= 1
        return result
        
```

