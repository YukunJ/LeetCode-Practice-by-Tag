**40. Combination Sum II**

```Tag : dfs/backtracking```

**Description:**

Given a collection of candidate numbers (```candidates```) and a target number (```target```), find all **unique** combinations in ```candidates``` where the candidate numbers sum to ```target```.

Each number in ```candidates``` may only be used **once** in the combination.

**Note**: The solution set must not contain duplicate combinations.

**Example1:**

		Input: candidates = [10,1,2,7,6,1,5], target = 8
		Output: 
		[
		[1,1,6],
		[1,2,5],
		[1,7],
		[2,6]
		]

**Example2:**

		Input: candidates = [2,5,2,1,2], target = 5
		Output: 
		[
		[1,2,2],
		[5]
		]


-----------

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        """
        The only difference between this question and Q39 <Combination Sum>
        is that 1) each number in candidates may only be used once and 2) we need to ensure answer generated is unique
	1) is easy, just to change idx to idx+1 in the search process, 
	i.e. after checking on this number candidate[idx], no longer re-consider it
	2) needs sorting and skipping
        again we use dfs + backtracking in a tree-expanding way
        to avoid duplicate answer, we could sort the array first and jump skip repeated elements
        and in the dfs part, if two adjacent numbers x1 and x2 are equal,
        then we don't need to recur on x2, because when we recur on x1,
        x2 is still a subset of the remaining recursion dfs of x1 part
        
        denote n := len(candidates)
        Time Complexity : O(n * 2^n) in worst case, 
                        there are 2^n possibilities and each takes O(n) time to assess
        Space Complexity : O(n) recursion stack and the temporary 'path' variable storage
        """
        def dfs(candidates: List[int], n: int, idx: int, path: List[int], remainder: int) -> None:
            if remainder <= 0:
                if remainder == 0:
                    # a solution found
                    res.append(path[:])
                # terminate the search process anyway
                return 
            else:
                for search_idx in range(idx, n):
                    if search_idx > idx and candidates[search_idx] == candidates[search_idx-1]:
                        # a repeated number found, skip it
                        continue
                    path.append(candidates[search_idx])
                    dfs(candidates, n, search_idx+1, path, remainder-candidates[search_idx])
                    path.pop() # reset the path from dfs
            
        res = []
        path = []
        candidates.sort() # sort in ascending to deal with duplicates
        dfs(candidates, len(candidates), 0, path, target)
        return res
```
