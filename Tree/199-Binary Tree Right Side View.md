**199. Binary Tree Right Side View**

```Tag: Tree```

**Description:**

Given the ```root``` of a binary tree, imagine yourself standing on the **right side** of it, return the ```values of the nodes``` you can see ordered from top to bottom.

**Example1**:

![avatar](Fig/199-E1.jpg)

        Input: root = [1,2,3,null,5,null,4]
        Output: [1,3,4]
        
**Example2**:

        Input: root = [1,null,3]
        Output: [1,3]

**Example3**:

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
    def rightSideView(self, root: TreeNode) -> List[int]:
        """
        This is a basic tree BFS search problem
        we use a queue to traversal the tree level by level
        and record the rightmost value on every level
        
        denote N := number of nodes in the tree
        Time Complexity : O(N)
        Space Complexity : O(N)
        """
        if not root: # boundary checking
            return []
        ans = list()
        queue = [root]
        while queue:
            newQueue = list() # init next level's queue
            ans.append(queue[-1].val) # add the rightmost one on this level
            for node in queue: # left to right, append next level's nodes
                if node.left:
                    newQueue.append(node.left)
                if node.right:
                    newQueue.append(node.right)
            queue = newQueue
        
        return ans
```
