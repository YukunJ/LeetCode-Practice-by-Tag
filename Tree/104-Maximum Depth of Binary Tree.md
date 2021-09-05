**104. Maximum Depth of Binary Tree**

```Tag : tree/bfs/dfs/recursion```

**Description:**

Given the ```root``` of a binary tree, return its *maximum depth*.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example1:**

![avatar](Fig/104-E1.jpg)

        Input: root = [3,9,20,null,null,15,7]
        Output: 3

**Example2:**
		
        Input: root = [1,null,2]
        Output: 2
        
**Example3:**       

        Input: root = []
        Output: 0

**Example4:**

        Input: root = [0]
        Output: 1

-----------

**Solution1: Recursion**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        """
        Recursion approach
        
        denote n := number of nodes in the binary tree
        Time Complexity : O(n)
        Space Complexity : O(n) in worst case skewed tree, O(logn) on average
        """
        def recur(node: TreeNode) -> int:
            """Helper function: return the maxDepth with node as the root """
            if not node: # boundary
                return 0
            left = recur(node.left)
            right = recur(node.right)
            return 1 + max(left, right)
        
        return recur(root)
```

-----------

**Solution2: Iterative**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        """
        Iterative approach
        
        denote n := number of nodes in the binary tree
        Time Complexity : O(n)
        Space Complexity : O(n) in worst case
        """
        from collections import deque
        if not root: # boundary case
            return 0
        queue =deque([(root, 1)])
        max_depth = 0
        while queue:
            # basically adopt a BFS search approach
            node, depth = queue.popleft()
            max_depth = max(max_depth, depth) # update the possible max depth
            if node.left:
                queue.append((node.left, depth+1))
            if node.right:
                queue.append((node.right, depth+1))
        return max_depth
```
