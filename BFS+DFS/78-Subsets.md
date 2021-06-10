**78. Subsets**

```Tag: DFS```

**Description:**

Given an integer array ```nums``` of unique elements, return all possible subsets (the power set).

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example1**:

        Input: nums = [1,2,3]
        Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

**Example2**:

        Input: nums = [0]
        Output: [[],[0]]

-----------

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        """
        This is a DFS backtracking problem
        denote n := len(nums), we know the power sets is of size 2^n
        at each index, you either take the number or not, this is a binary branching

        Time Complexity: O(2^n * n) coping to result list costs O(n), there are 2^n answers
        Space Complexity: O(n) for both the temporary "curr" var and recursion stack height
        """
        res = []
        curr = []

        def backtrack(index: int):
            """ 
                at each index, either take the number nums[index] there or not
                i.e. binary branching 
            """
            if index == len(nums): # the end of dfs search reached
                res.append(curr[:]) # shallow copy
                return
            # pick this number
            curr.append(nums[index])
            backtrack(index+1)
            curr.pop() # when you done, clear it up

            # not pick this number
            backtrack(index+1)
        backtrack(0)
        return res
```
