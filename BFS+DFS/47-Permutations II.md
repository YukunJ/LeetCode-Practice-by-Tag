**47. Permutations II**

```Tag: DFS```

**Description:**

Given a collection of numbers, ```nums```, that might contain duplicates, return all possible **unique** permutations in any order.

**Example1**:

        Input: nums = [1,1,2]
        Output:
        [[1,1,2],
         [1,2,1],
         [2,1,1]]

**Example2**:

        Input: nums = [1,2,3]
        Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

-----------

```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        """
        This Question is similar to Question46, except we might have duplicates in the "nums"
        So in order to achieve unique solution, 
            we need to record if the current number is already pick "before" in DFS.
        We sort the array first, so as to make duplicate numbers sit together with each other
        And we need another array of "checking" if a number is already picked, 
            and we skip duplicate numbers
        Both complexity remain the same as before

        Time Complexity : O(n * n!)
        Space Complexity : O(n)
        """
        if not nums:
            return []
        nums.sort()
        check = [False] * len(nums)
        res = []
        def backtrack(perm: List[int]):
            if len(perm) == len(nums): # finish a permutation, record it
                res.append(perm)
            else:
                for i in range(len(nums)):
                    if check[i]:
                        continue
                    if i > 0 and nums[i] == nums[i-1] and not check[i-1]:
                        continue
                    check[i] = True
                    backtrack(perm+[nums[i]])
                    check[i] = False
        
        backtrack([])
        return res
```
