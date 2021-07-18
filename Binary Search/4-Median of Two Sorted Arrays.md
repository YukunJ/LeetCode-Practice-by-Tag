**4. Median of Two Sorted Arrays**

```Tag: Binary Search/Divide and Conquer```

**Description:**

Given two sorted arrays ```nums1``` and ```nums2``` of size ```m``` and ```n``` respectively, return the **median** of the two sorted arrays.

The overall run time complexity should be ```O(log (m+n))```.

**Example1:**

		Input: nums1 = [1,3], nums2 = [2]
		Output: 2.00000
		Explanation: merged array = [1,2,3] and median is 2.


**Example2:**

		Input: nums1 = [1,2], nums2 = [3,4]
		Output: 2.50000
		Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.

**Example3:**

		Input: nums1 = [0,0], nums2 = [0,0]
		Output: 0.00000

**Example4:**

		Input: nums1 = [], nums2 = [1]
		Output: 1.00000

**Example5:**

		Input: nums1 = [2], nums2 = []
		Output: 2.00000

-----------

```python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        """
        when n+m is odd, we are to find (m+n)//2+1 th element
        when n+m is even, we are to find mean of (m+n)//2 th and (m+n)//2+1 th element
        so the question reduces to "Find K-th smallest element from two sorted array"
        we apply a binary search schema
        we fix pivot1 = nums1[k//2-1] and pivot2 = nums2[k//2-1]
        when pivot1 <= pivot2, we know that
        pivot1 is at most larger than k//2-1 + k//2-1 elements, so itself is at most the k-1 smallest elements
        so we can abandon A[0:k//2-1] part and look for (k-(k//2-1)) -th smallest element in the remaining two arrays
        """
        def getKthElement(k: int) -> int:
            """Helper function: get the k-th smallest element from two sorted list"""
            ptr1, ptr2 = 0, 0
            while True:
                # boundary cases
                if ptr1 == n:
                    return nums2[ptr2+k-1]
                if ptr2 == m:
                    return nums1[ptr1+k-1]
                if k == 1: 
                    return min(nums1[ptr1], nums2[ptr2])
                
                # normal cases
                new_ptr1 = min(ptr1 + k//2 -1, n-1)
                new_ptr2 = min(ptr2 + k//2 -1, m-1)
                pivot1, pivot2 = nums1[new_ptr1], nums2[new_ptr2]
                if pivot1 <= pivot2:
                    # decrement the 'k'-th to find, 'how many are abandoned'
                    k -= (new_ptr1-ptr1+1)
                    # adandon nums1[:new_ptr1], including new_ptr1
                    ptr1 = new_ptr1+1
                else:
                    k -= (new_ptr2-ptr2+1)
                    ptr2 = new_ptr2+1
                    
        n, m = len(nums1), len(nums2)
        k = n + m
        if k % 2 == 1: # odd case
            return getKthElement(k//2+1)
        else: # even case
            return (getKthElement(k//2)+getKthElement(k//2+1)) / 2.0  
```
