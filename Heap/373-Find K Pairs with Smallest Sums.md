**373. Find K Pairs with Smallest Sums**

```Tag : Heap/Priorty Queue/Array```

**Description:**

You are given two integer arrays ```nums1``` and ```nums2``` sorted in **ascending order** and an integer ```k```.

Define a pair ```(u, v)``` which consists of one element from the first array and one element from the second array.

Return the ```k``` pairs ```(u1, v1), (u2, v2), ..., (uk, vk)``` with the smallest sums.

**Example1:**

        Input: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
        Output: [[1,2],[1,4],[1,6]]
        Explanation: The first 3 pairs are returned from the sequence: [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]

**Example2:**

        Input: nums1 = [1,1,2], nums2 = [1,2,3], k = 2
        Output: [[1,1],[1,1]]
        Explanation: The first 2 pairs are returned from the sequence: [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]

**Example3:**

        Input: nums1 = [1,2], nums2 = [3], k = 3
        Output: [[1,3],[2,3]]
        Explanation: All possible pairs are returned from the sequence: [1,3],[2,3]

-----------

```python
class Solution:
    def kSmallestPairs(self, nums1: List[int], nums2: List[int], k: int) -> List[List[int]]:
        """
        We adopt a priority queue approach to solve this problem
        At each step, we either increment the num1 array by 1 position or num2
        However, we keep to keep track of both result
        
        denote n := len(nums1), m := len(nums2)
        Time Complexity : O(k*logk) each time when you pop one item from priority queue, you insert at most 2 others into it
                          therefore, there are at most O(k) elements in the queue, and each queue operation takes O(logk)
        Space Complexity : O(k) for the priority queue storage
        """
        from queue import PriorityQueue
        
        if not k: # boundary case
            return []
        res = []
        count = 0
        pq = PriorityQueue()
        visited = set()
        
        n, m = len(nums1), len(nums2)
        pq.put((nums1[0]+nums2[0], 0, 0))
        visited.add((0, 0))
        
        while not pq.empty():
            pair_sum, ptr1, ptr2 = pq.get()
            res.append([nums1[ptr1], nums2[ptr2]])
            count += 1
            
            if count == k:
                # mission accomplished
                break
            
            if ptr1+1 < n and (ptr1+1, ptr2) not in visited:
                # try move first pointer
                visited.add((ptr1+1, ptr2))
                pq.put((nums1[ptr1+1]+nums2[ptr2], ptr1+1, ptr2))
            
            if ptr2+1 < m and (ptr1, ptr2+1) not in visited:
                # try move second pointer
                visited.add((ptr1, ptr2+1))
                pq.put((nums1[ptr1]+nums2[ptr2+1], ptr1, ptr2+1))
        
        return res
```

