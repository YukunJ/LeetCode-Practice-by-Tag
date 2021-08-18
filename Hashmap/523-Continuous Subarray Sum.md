**523. Continuous Subarray Sum**

```Tag : Hashtable/Presum Array```

**Description:**

Given an integer array ```nums``` and an integer ```k```, return ```true``` if ```nums``` has a continuous subarray of size **at least two** whose elements sum up to a multiple of ```k```, or ```false``` otherwise.

An integer ```x``` is a multiple of ```k``` if there exists an integer ```n``` such that ```x = n * k```. ```0``` is **always** a multiple of ```k```.


**Example1:**

		Input: nums = [23,2,4,6,7], k = 6
		Output: true
		Explanation: [2, 4] is a continuous subarray of size 2 whose elements sum up to 6.

**Example2:**
	
		Input: nums = [23,2,6,4,7], k = 6
		Output: true
		Explanation: [23, 2, 6, 4, 7] is an continuous subarray of size 5 whose elements sum up to 42.
		42 is a multiple of 6 because 42 = 7 * 6 and 7 is an integer.

**Example3:**

		Input: nums = [23,2,6,4,7], k = 13
		Output: false

-----------

```python
class Solution:
    def checkSubarraySum(self, nums: List[int], k: int) -> bool:
        """
        Initially we did a presum O(n^2) solution, but it times out
        the optimal solution is to realize that if two presum has the same value after taking module k
        their subtraction result is going to be a multiple of k
        we utilize this point and save module result in a hashtable
        
        denote n := len(nums)
        Time Complexity : O(n)
        Space Complexity : O(n) presum vector
        """
        module_memory = dict()
        module_memory[0] = -1 # fully divisible is OK
        
        n = len(nums)
        curr_sum = 0
        for i in range(n):
            curr_sum += nums[i]
            residue = curr_sum % k
            if residue not in module_memory:
                # record the first appearance of such a residue, the farthest
                module_memory[residue] = i
            else:
                # already see this residue result
                array_length = i - module_memory[residue]
                if array_length >= 2:
                    # a valid result found
                    return True
        
        return False
```
