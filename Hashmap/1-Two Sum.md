**1. Two Sum**

```Tag: hashmap```

**Description:**

Given an array of integers ```nums``` and an integer ```target```, return indices of the two numbers such that they add up to ```target```.

You may assume that each input would have **exactly one solution**, and you may not use the same element twice.

You can return the answer in any order.

**Follow-up**: Can you come up with an algorithm that is less than O(n2) time complexity?

**Example1**:

        Input: nums = [2,7,11,15], target = 9
        Output: [0,1]
        Output: Because nums[0] + nums[1] == 9, we return [0, 1].

**Example2**:

        Input: nums = [3,2,4], target = 6
        Output: [1,2]
        
**Example3**:

        Input: nums = [3,3], target = 6
        Output: [0,1]

-----------

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        """
        We use a hashmap to store the integer we've encountered
        and do 1-time iteration through the list
        
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        hashmap = dict()
        for i, num in enumerate(nums):
            to_search = target - num
            if to_search in hashmap:
                return [hashmap[to_search], i]
            hashmap[num] = i
        return [] # indicate failure for search
```
