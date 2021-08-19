**938. Range Sum of BST**

```Tag : tree/dfs/bfs```

**Description:**

Given the ```root``` node of a binary search tree and two integers ```low``` and ```high```, return the *sum of values* of all nodes with a value in the **inclusive** range ```[low, high]```.


**Example1:**

![avatar](Fig/9389-E1.jpg)

		Input: root = [10,5,15,3,7,null,18], low = 7, high = 15
		Output: 32
		Explanation: Nodes 7, 10, and 15 are in the range [7, 15]. 7 + 10 + 15 = 32.

**Example2:**

![avatar](Fig/938-E2.jpg)

		Input: root = [10,5,15,3,7,13,18,1,null,6], low = 6, high = 10
		Output: 23
		Explanation: Nodes 6, 7, and 10 are in the range [6, 10]. 6 + 7 + 10 = 23.

-----------

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rangeSumBST(self, root: Optional[TreeNode], low: int, high: int) -> int:
        """
        A simple in-order traversal application
		we adopt an iterative approach here
        denote n := number of nodes in the binary tree
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        from collections import deque
        
        if not root: # boundary case checking
            return 0
        
        range_sum = 0
        queue = deque([root])
        while queue:
            node = queue.popleft()
            if low <= node.val <= high:
                range_sum += node.val
            if node.val > low and node.left: 
                # worth to check left subtree
                queue.append(node.left)
            if node.val < high and node.right:
                # worth to check right subtree
                queue.append(node.right)
                
        return range_sum
```