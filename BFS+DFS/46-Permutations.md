**46. Permutations**

```Tag: DFS```

**Description:**

Given an array ```nums``` of distinct integers, return all the possible permutations. You can return the answer in **any order**.

**Example1**:

        Input: nums = [1,2,3]
        Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

**Example2**:

        Input: nums = [0,1]
        Output: [[0,1],[1,0]]

**Example3**:

        Input: nums = [1]
        Output: [[1]]

-----------

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        """
        This is a DFS-backtracking problem
        we are filling an array of length n with n integers
        we want to dynamically maintain the 'nums' array so that,
        when we want to fill "index" position, nums[:index] is those already picked
        so we need to switch positions back and forward in each recursion stack
        
        Time Complexity : O(n * n!), O(n) for coping the curr permutation to ans vector
                                    O(n!) is possible number of permutations we may generate
        Space Complexity : O(n) recursion stack height
        """
        if not nums: # boundary checking
            return []
        res = []
        n = len(nums)
        
        def backtrack(index: int):
            if index == n:
                res.append(nums[:])
            else:
                for switch in range(index, n):
                    # want to pick "switch", make it to the left
                    nums[index], nums[switch] = nums[switch], nums[index]
                    backtrack(index + 1)
                    # cancel the previous switching operation
                    nums[index], nums[switch] = nums[switch], nums[index]
        backtrack(0)
        return res
```
