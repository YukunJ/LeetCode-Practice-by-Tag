**1186. Maximum Subarray Sum with One Deletion**

```Tag : dynamic programming```

**Description:**

Given an array of integers, return the maximum sum for a **non-empty** subarray (contiguous elements) with at most one element deletion. In other words, you want to choose a subarray and optionally delete one element from it so that there is still at least one element left and the sum of the remaining elements is maximum possible.

Note that the subarray needs to be **non-empty** after deleting one element.

**Example1:**

        Input: arr = [1,-2,0,3]
        Output: 4
        Explanation: Because we can choose [1, -2, 0, 3] and drop -2, thus the subarray [1, 0, 3] becomes the maximum value.

**Example2:**

        Input: arr = [1,-2,-2,3]
        Output: 3
        Explanation: We just choose [3] and it's the maximum sum.
        
**Example3:**

        Input: arr = [-1,-1,-1,-1]
        Output: -1
        Explanation: The final subarray needs to be non-empty. You can't choose [-1] and delete -1 from it, then get an empty subarray to make the sum equals to 0.

**Hints**:

+ How to solve this problem if no deletions are allowed ?

+ Try deleting each element and find the maximum subarray sum to both sides of that element.

+ To do that efficiently, use the idea of Kadane's algorithm.

-----------

**Solution1: O(n) Space**

```python
class Solution:
    def maximumSum(self, arr: List[int]) -> int:
        """
        The only difference between this problem and the normal max sum subarray problem
        is that we are allowed to delete one element from a subarray
        we can build a dp array allowing 1 deletion based upon a dp array with no deletion
        dp_one_delete[i] = max(dp_no_delete[i], \
                              dp_no_delete[i-1](delete arr[i] here), \
                              dp_one_delete[i-1]+nums[i] (used up all deletion opportunity ))

        denote n := len(arr)
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        n = len(arr)
        curr = arr[0]
        dp_no_delete = [curr]
        for num in arr[1:]:
            curr = max(curr, 0) + num
            dp_no_delete.append(curr)
            
        curr = arr[0]
        dp_one_delete = [curr]
        for i in range(1, n):
            # three choices in each state
            # 1) no deletion at all so far
            no_delete = dp_no_delete[i]
            # 2) delete current num
            delete = dp_no_delete[i-1]
            # 3) already used delete before, no more deletion
            used_up = arr[i] + dp_one_delete[i-1]
            # take the max of these three
            dp_one_delete.append(max(no_delete, delete, used_up))
            
        return max(dp_one_delete)
```

-----------

**Solution2: O(1) Space**

```python
class Solution:
    def maximumSum(self, arr: List[int]) -> int:
        """
        The only difference between this problem and the normal max sum subarray problem
        is that we are allowed to delete one element from a subarray
        we can build a dp array allowing 1 deletion based upon a dp array with no deletion
        dp_one_delete[i] = max(dp_no_delete[i], \
                              dp_no_delete[i-1](delete arr[i] here), \
                              dp_one_delete[i-1]+nums[i] (used up all deletion opportunity ))

        denote n := len(arr)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        n = len(arr)
        ans = last_delete = last_no_delete = arr[0]
        for i in range(1, n):
            # three choices in each state
            # 1) no deletion at all so far
            if_no_delete = max(last_no_delete, 0) + arr[i]
            # 2) delete current num
            if_delete = last_no_delete
            # 3) already used delete before, no more deletion
            if_used_up = last_delete + arr[i]
            # take the max of these three
            delete = max(if_no_delete, if_delete, if_used_up)
            ans = max(delete, ans)
            # update the last recording for next round
            last_delete = delete
            last_no_delete = if_no_delete
            
        return ans
```
