**90. Subsets II**

```Tag: DFS```

**Description:**

Given an integer array ```nums``` that may contain **duplicates**, return all possible subsets (the power set).

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example1**:

        Input: nums = [1,2,2]
        Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]

**Example2**:

        Input: nums = [0]
        Output: [[],[0]]

-----------

```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        """
        This is a DFS backtracking problem
        denote n := len(nums), we know the power sets is of size 2^n
        at each index, you either take the number or not, this is a binary branching
        The difference between this problem and problem <78. Subsets> is that we may have duplicate here
        We first sort the array, let duplicate numbers sit together.
        In order to avoid duplicate, 
        we let the binary expansion of picking or not a seris of duplicate number be decreasing
        for example, we allow 111, 110, 100,
        but not allow 011, 101, 001
        Once the first "0(not pick)" appears, all the following duplicates should not be picked
        
        Time Complexity: O(2^n * n) coping to result list costs O(n), there are 2^n answers
        Space Complexity: O(n) for both the temporary "curr" var and recursion stack height
        """
        nums.sort() # sort, duplicates become adjacent
        res = []
        curr = []
        check = [0] * len(nums) # record keeping
        
        def backtrack(index: int):
            """ 
                at each index, either take the number nums[index] there or not
                i.e. binary branching 
            """
            if index == len(nums): # the end of dfs search reached
                res.append(curr[:]) # shallow copy
                return
            
            # not pick this number
            backtrack(index+1)
            
            if (index > 0 and nums[index] == nums[index-1] and not check[index-1]):
                # we find a duplicate, and the previous duplicate is not picked
                # to make it decreasing binary expansion, we will not pick this one neither
                return
                
            # pick this number
            check[index] = 1
            curr.append(nums[index])
            backtrack(index+1)
            curr.pop() # when you done, clear it up
            check[index] = 0

        backtrack(0)
        return res
```
