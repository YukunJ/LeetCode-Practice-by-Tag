**1539. Kth Missing Positive Number**

```Tag : binary search```

**Description:**

Given an array ```arr``` of positive integers sorted in a **strictly increasing order**, and an integer ```k```.

Find the ```k-th``` positive integer that is *missing from this array*.

**Example1:**

		Input: arr = [2,3,4,7,11], k = 5
		Output: 9
		Explanation: The missing positive integers are 		[1,5,6,8,9,10,12,13,...]. The 5th missing positive integer is 9.

**Example2:**

		Input: arr = [1,2,3,4], k = 2
		Output: 6
		Explanation: The missing positive integers are 		[5,6,7,...]. The 2nd missing positive integer is 6.

-----------

```python
class Solution:
    def findKthPositive(self, arr: List[int], k: int) -> int:
        """
        Because of python's 0-indexed, 
        at position idx, a well placed array should be arr[idx] = idx + 1
        Hence, the number "arr[idx] - idx - 1" would be the positive numbers missing before 
        we utilize binary search here
        
        denote n := len(arr)
        Time Complexity : O(logn)
        Space Complexity : O(1)
        """
        n = len(arr)
        left, right = 0, n - 1
        ans = -1
        while left <= right:
            mid = left + (right - left) // 2
            # we are looking for the largest 'mid'
            # such that arr[mid] - mid - 1 < k
            # i.e. we hope the k-th missing positive lie in arr[mid] and arr[mid+1]
            if arr[mid] - mid - 1 < k:
                ans = mid
                left = mid + 1
            else:
                right = mid - 1
        
        # arr[ans] + (k - arr[ans] + ans + 1) =  k + ans + 1
        return k + ans + 1
```