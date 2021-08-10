**543. Diameter of Binary Tree**

```Tag: Tree```

**Description:**

Given the ```root``` of a binary tree, return the length of the **diameter** of the tree.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the root.

The **length** of a path between two nodes is represented by the number of edges between them.

**Example1**:

![avatar](Fig/543-E1.jpg)

        Input: root = [1,2,3,4,5]
        Output: 3
        Explanation: 3 is the length of the path [4,2,1,3] or [5,2,1,3].

 **Example2**:

        Input: root = [1,2]
        Output: 1

-----------

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        """
        This is a tree recursion problem
        instead of computing the diameter directly, 
        we compute the maximum number of nodes contained in a path
            and the diameter is # of nodes - 1
        
        denote N := number of nodes in the binary tree
        Time Complexity : O(N)
        Space Complexity : O(N)
        """
        def recur(node: TreeNode) -> int:
            """Helper function to recursively compute the maximal diameter"""
            nonlocal connectedNode
            if not node:
                return 0
            val = 1
            left, right = recur(node.left), recur(node.right)
            connectedNode = max(connectedNode, left+val+right)
            return val + max(left, right)
        
        connectedNode = float("-inf")
        recur(root)
        return connectedNode - 1
```
