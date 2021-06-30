**560. Subarray Sum Equals K**

```Tag: hashmap```

**Description:**

Given an array of integers ```nums``` and an integer ```k```, return the **total number of continuous subarrays** whose sum equals to ```k```.

**Hints**:

+ Will Brute force work here? Try to optimize it.

+ Can we optimize it by using some extra space?

+ What about storing sum frequencies in a hash table? Will it be useful?

+ ```sum(i,j)=sum(0,j)-sum(0,i)```, where ```sum(i,j)``` represents the sum of all the elements from index ```i``` to ```j-1```. Can we use this property to optimize it?

**Example1**:

        Input: nums = [1,1,1], k = 2
        Output: 2

**Example2**:

        Input: nums = [1,2,3], k = 3
        Output: 2

-----------

```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        """
        We start to think this problem by realizing that
        sum(i,j) = sum(0,j)-sum(0,i) where sum(i,j) include nums[i] but not including nums[j]
        suppose we are at i, we are looking for j <= i such that
        sum(0,j) = sum(0,i) - target
        such a "j" might appear multiple times for a given fixed i,
            since the number in array might be negative
        we use a hashmap to record all the appearance frequency of "sum(0,j)" to quickly find
        if a "sum(0,j) = sum(0,i) - target" could be found
        
        denote n := len(nums)
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        
        from collections import defaultdict
        hashmap = defaultdict(int)
        hashmap[0] = 1 # for exact match nums[0:j] equals k
        presum = 0
        count = 0
        for num in nums:
            presum += num
            count += hashmap[presum-k] # might be 0, if such a presum not appeared before
            hashmap[presum] += 1
        return count
```
