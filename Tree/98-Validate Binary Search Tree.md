**98. Validate Binary Search Tree**

```Tag: Tree/traversal/DFS```

**Description:**

Given the ```root``` of a binary tree, determine if it is a valid **binary search tree (BST)**.

A **valid BST** is defined as follows:

+ The left subtree of a node contains only nodes with keys less than the node's key.
+ The right subtree of a node contains only nodes with keys greater than the node's key.
+ Both the left and right subtrees must also be binary search trees.

**Example1**:

![avatar](Fig/98-E1.jpg)

        Input: root = [2,1,3]
        Output: true
        
**Example2**:

![avatar](Fig/98-E2.jpg)

        Input: root = [5,1,4,null,null,3,6]
        Output: false
        Explanation: The root node's value is 5 but its right child's value is 4.

-----------

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        """
        This is a tree in-order traversal problem
        If a binary tree is a valid BST,
        its in-order traversal should be a strictly increasing sequence
        
        denote N := number of nodes in the tree
        Time Complexity : O(N)
        Space Complexity : O(N)
        """
        valid = True
        prev = float('-inf')
        def inorder_traversal(node: TreeNode):
            nonlocal prev, valid
            if not valid: # already fail, just return 
                return
            if node.left:
                inorder_traversal(node.left)
            if prev >= node.val:
                valid = False
            prev = node.val # update the previous value
            if node.right:
                inorder_traversal(node.right)
        
        inorder_traversal(root)
        return valid
```
