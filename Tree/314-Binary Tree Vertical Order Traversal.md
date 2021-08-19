**314. Binary Tree Vertical Order Traversal**

```Tag : tree/bfs/hashtable```

**Description:**

Given the ```root``` of a binary tree, return **the vertical order traversal** of its nodes' values. (i.e., from top to bottom, column by column).

If two nodes are in the same row and column, the order should be from **left to right**.


**Example1:**

![avatar](Fig/314-E1.jpg)

		Input: root = [3,9,20,null,null,15,7]
		Output: [[9],[3,15],[20],[7]]

**Example2:**

![avatar](Fig/314-E2.jpg)

		Input: root = [3,9,8,4,0,1,7]
		Output: [[4],[9],[3,0,1],[8],[7]]

**Example3:**

![avatar](Fig/314-E3.jpg)

		Input: root = [3,9,8,4,0,1,7,null,null,null,2,5]
		Output: [[4],[9,5],[3,0,1],[8,2],[7]]

**Example4:**

		Input: root = []
		Output: []

-----------

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def verticalOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        """
        This is essentially a traversal problem similar to Q987
        However, we could avoid sorting in this question
        we do a BFS traversal, on each level from left to right
        and also we maintain the column index to add the node value to proper hashtable entry
        
        denote n := number of nodes in the binary tree
        Time Complexity : O(n) each node is processed once
        Space Complexity : O(n) the queue is at most n/2 wide
        """
        from collections import defaultdict, deque
        if not root: # boundary case checking
            return []
        
        records = defaultdict(list)
        res = []
        min_col, max_col = float('inf'), float('-inf')
        queue = deque([(root, 0)])
        while queue:
            node, col = queue.popleft()
            min_col, max_col = min(min_col, col), max(max_col, col)
            records[col].append(node.val)
            if node.left:
                queue.append((node.left, col-1))
            if node.right:
                queue.append((node.right, col+1))
                
        for col in range(min_col, max_col+1):
            res.append(records[col])
        return res 
```
