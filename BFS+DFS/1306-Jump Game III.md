**1306. Jump Game III**

```Tag : bfs/dfs```

**Description:**

Given an array of non-negative integers ```arr```, you are initially positioned at ```start``` index of the array. When you are at index ```i```, you can jump to ```i + arr[i]``` or ```i - arr[i]```, check if you can reach to **any** index with value ```0```.

Notice that you can not jump outside of the array at any time.

**Example1:**

		Input: arr = [4,2,3,0,3,1,2], start = 5
		Output: true
		Explanation: 
		All possible ways to reach at index 3 with value 0 are: 
		index 5 -> index 4 -> index 1 -> index 3 
		index 5 -> index 6 -> index 4 -> index 1 -> index 3 

**Example2:**

		Input: arr = [4,2,3,0,3,1,2], start = 0
		Output: true 
		Explanation: 
		One possible way to reach at index 3 with value 0 is: 
		index 0 -> index 4 -> index 1 -> index 3

**Example3:**
		
		Input: arr = [3,0,2,1,2], start = 2
		Output: false
		Explanation: There is no way to reach at index 1 with value 0.

-----------

```python
class Solution:
    def canReach(self, arr: List[int], start: int) -> bool:
        """
        We consider a BFS approach to avoid visiting an index multiple times
        denote n := len(arr)
        Time Complexity : O(n) every index visited only once
        Space Complexity : O(n) for deque
        """
        from collections import deque
        n = len(arr)
        visited = set()
        queue = deque([start])
        while queue:
            idx = queue.popleft()
            visited.add(idx)
            if arr[idx] == 0: # solution found
                return True
            for jump_idx in [idx-arr[idx], idx+arr[idx]]:
                # careful about boundary checking
                if 0 <= jump_idx < n and jump_idx not in visited:
                    queue.append(jump_idx)
        return False
```

