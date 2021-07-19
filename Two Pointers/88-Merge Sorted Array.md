**88. Merge Sorted Array**

```Tag : two pointers/sorting```

**Description:**

You are given two integer arrays ```nums1``` and ```nums2```, sorted in **non-decreasing** order, and two integers ```m``` and ```n```, representing the number of elements in ```nums1``` and ```nums2``` respectively.

**Merge** ```nums1``` and ```nums2``` into a single array sorted in **non-decreasing order**.

The final sorted array should not be returned by the function, but instead be stored inside the array ```nums1```. To accommodate this, nums1 has a length of ```m + n```, where the first ```m``` elements denote the elements that should be merged, and the last ```n``` elements are set to ```0``` and should be ignored. ```nums2``` has a length of ```n```.


**Example1**:

		Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
		Output: [1,2,2,3,5,6]
		Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
		The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.

**Example2**:

		Input: nums1 = [1], m = 1, nums2 = [], n = 0
		Output: [1]
		Explanation: The arrays we are merging are [1] and [].
		The result of the merge is [1].

**Example3**:

		Input: nums1 = [0], m = 0, nums2 = [1], n = 1
		Output: [1]
		Explanation: The arrays we are merging are [] and [1].
		The result of the merge is [1].
		Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.

**Hints**:

+ You can easily solve this problem if you simply think about two elements at a time rather than two arrays. We know that each of the individual arrays is sorted. What we don't know is how they will intertwine. Can we take a local decision and arrive at an optimal solution?

+ If you simply consider one element each at a time from the two arrays and make a decision and proceed accordingly, you will arrive at the optimal solution.
        
-----------


```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        We will use two pointers approach in solving this problem
        notice that since we don't want to overwrite num1's first m elements,
        we will first from "largest" element from the two arrays and put it at the end of num1

        Time Complexity : O(m+n)
        Space Complexity : O(1)
        """
        ptr, ptr1, ptr2 = m+n-1, m-1, n-1
        while 0 <= ptr1 and 0 <= ptr2:
            # sliding from right to left
            if nums1[ptr1] >= nums2[ptr2]:
                nums1[ptr] = nums1[ptr1]
                ptr1 -= 1
                ptr -= 1
            else:
                nums1[ptr] = nums2[ptr2]
                ptr2 -= 1
                ptr -= 1
        
        # if nums2 are fully exploited, we can just leave the rest nums1 be there already sorted
        # if nums1 is first fully exploited, need to move the rest of nums2 to first part of nums1
        if ptr1 < 0:
            while ptr >= 0:
                nums1[ptr] = nums2[ptr2]
                ptr -= 1
                ptr2 -= 1
        
        return 
```
