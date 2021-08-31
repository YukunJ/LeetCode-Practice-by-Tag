**128. Longest Consecutive Sequence**

```Tag : disjoint set union/hashtable```

**Description:**

Given an unsorted array of integers ```nums```, return the *length of the longest consecutive elements sequence*.

You must write an algorithm that runs in ```O(n)``` time.

**Example1:**
	
		Input: nums = [100,4,200,1,3,2]
		Output: 4
		Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.

**Example2:**

		Input: nums = [0,3,7,2,5,8,4,6,0,1]
		Output: 9

-----------

**Solution1: Disjoint Set Union**

```python
class DSU:
    """Helper class: Disjoint Set of Union"""
    def __init__(self, nums: List[int]):
        self.n = len(nums)
        self.parent = [i for i in range(self.n)] # the "cluster parent"
        self.rank = [1 for i in range(self.n)] # how many points each cluster contain
    
    def find(self, i: int) -> int:
        if self.parent[i] != i:
            # path compression
            self.parent[i] = self.find(self.parent[i])
        return self.parent[i]
    
    def union(self, p: int, q: int):
        leaderp, leaderq = self.find(p), self.find(q)
        if leaderp != leaderq:
            if self.rank[leaderp] > self.rank[leaderq]:
                # want leaderp always be the smaller one to be absorted
                leaderp, leaderq = leaderq, leaderp
            self.parent[leaderp] = leaderq
            # the bigger cluster eats up all the elements in the other cluster
            self.rank[leaderq] += self.rank[leaderp]
    
    def getLongest(self) -> int:
        return max(self.rank)

class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        """
        We adopt a Disjoint Set Union approach in solving this problem 
        we want to put consecutive number into "clusters" 
        and gradually expand soem clusters as we iterate through the nums array
        Be careful here that duplicate number doesn't count, so we need a bit of pre-processing to remove duplicate
        Disjoint set Union operation is more or less approximate O(1) operation, since Inverse of Tower of 2 could be treated as constant.
        
        denote n := len(nums)
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        if not nums: # boundary case
            return 0
        nums = list(set(nums))
        my_DSU = DSU(nums)
        memory = {}
        for idx, num in enumerate(nums):
            memory[num] = idx
            for neighbor in [num-1, num+1]:
                if neighbor in memory:
                    my_DSU.union(idx, memory[neighbor])
        return my_DSU.getLongest()
```

-----------

**Solution2: One-Pass Trick**

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        """
        Another trick to achieve O(n) time complexity is as follows:
        we only start search a number 'num' when 'num-1' is not in the array
        otherwise it would be searched sooner or later
        In this way, we achieve the result that every number in array would be visited only once
        1) either it's the begining of a consecutive subarray
        2) or it's part of another consecutive subarray initiated by a smaller number in the array
        
        denote n := len(nums)
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        if not nums: # boundary case
            return 0
        
        longest = 0
        nums_set = set(nums)
        for num in nums_set:
            # each number will be visited only once
            if num - 1 not in nums_set:
                # a new consecutive subarray encountered
                curr_length = 1
                while num + 1 in nums_set:
                    num += 1
                    curr_length += 1
                longest = max(longest, curr_length)
        return longest
```