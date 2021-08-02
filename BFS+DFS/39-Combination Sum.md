**39. Combination Sum**

```Tag : DFS```

**Description:**

Given an array of **distinct** integers ```candidates``` and a target integer ```target```, return a list of all **unique combinations** of ```candidates``` where the chosen numbers sum to ```target```. You may return the combinations in **any order**.

The **same** number may be chosen from ```candidates``` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

It is **guaranteed** that the number of unique combinations that sum up to ```target``` is less than ```150``` combinations for the given input.

**Example1**:

        Input: candidates = [2,3,6,7], target = 7
        Output: [[2,2,3],[7]]
        Explanation:
        2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
        7 is a candidate, and 7 = 7.
        These are the only two combinations.

**Example2**:

        Input: candidates = [2,3,5], target = 8
        Output: [[2,2,2,2],[2,3,3],[3,5]]
        
**Example3**:

        Input: candidates = [2], target = 1
        Output: []

**Example4**:

        Input: candidates = [1], target = 1
        Output: [[1]]

**Example5**:

        Input: candidates = [1], target = 2
        Output: [[1,1]]
        
-----------

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        """
        DFS using backtracking
        since an element is allowed to be chosen multiple times
        it's like a tree expanding, with each weight meaning target - node_value
        The tree reaches leaf node when weight <= 0
        
        Time Complexity: O(S) where S is the length of all possible solutions. 
                        Another very loose upper bound is O(n * 2^n) where n := len(candidates). 
                        Consider whether or not to pick a position yielding 2^n possibility
                        and we need O(n) time to assess if it's valid or not
        Space Complexity: O(target) the stack for recursion is at most target high, 
                          since each recursion reduce the target at least by 1
        """
        def dfs(candidates: List[int], target: int, idx: int, path: List[int], res: List[List[int]]):
            """
            idx is the index you start to look. You don't look backward(may re-consider current idx),
            because any combination involves previous item should already be explored
            so it would be redunant
            """
            if target <= 0:
                # leaf reached
                if target == 0: # a combination found
                    res.append(path[:])
                return
            for index in range(idx, len(candidates)):
                path.append(candidates[index])
                dfs(candidates, target-candidates[index], index, path, res)
                path.pop()
                
        # main driver
        if len(candidates) == 0:
            return []
        res = []
        path = []
        dfs(candidates, target, 0, path, res)
        return res
```
