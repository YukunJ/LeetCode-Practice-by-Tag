**99. Recover Binary Search Tree**

```Tag: Tree/DFS```

**Description:**

You are given the ```root of``` a binary search tree (BST), where exactly two nodes of the tree were swapped by mistake. Recover the tree without changing its structure.

**Follow up**: A solution using ```O(n)``` space is pretty straight forward. Could you devise a constant space solution?

**Example1**:

![avatar](Fig/99-E1.jpg)

        Input: root = [1,3,null,null,2]
        Output: [3,1,null,null,2]
        Explanation: 3 cannot be a left child of 1 because 3 > 1. Swapping 1 and 3 makes the BST valid.
        
**Example2**:

![avatar](Fig/99-E2.jpg)

        Input: root = [3,1,4,null,null,2]
        Output: [2,1,4,null,null,3]
        Explanation: 2 cannot be in the right subtree of 3 because 2 < 3. Swapping 2 and 3 makes the BST valid.

-----------

```python
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def recoverTree(self, root: TreeNode) -> None:
        """
        This is again a tree in-order traversal problem
        If the tree is a proper binary search tree, 
        the in-order traversal will be strictly increasing
                i.e. [1,2,3,4] (example2 after correction)
        if a pair of node's values are swaped
                i.e. [1,3,2,4] (example2 beforecorrection)
        we will find a_{i} > a_{i+1} violate the increasing
        By finding such a pair, we can swap them back
        
        denote N := number of nodes in the binary tree
        Time Complexity : O(N)
        Space Complexity : O(N)
        """
        prev = TreeNode(val=float("-inf"))
        A = None # the first node of mistaken swap
        B = None # the second node of mistaken swap
        
        def inorder_traversal(node: TreeNode) -> None:
            """Helper function: do in-order tree traversal"""
            nonlocal prev, A, B
            if not node:
                return
            # traversal left subtree
            inorder_traversal(node.left)
            if prev.val >= node.val:
                if A is None: 
                    # pick the left of a wrong-order pair
                    A = prev
                if A is not None:
                    # pick the right of a wrong-order pair
                    B = node
            prev = node
            # traversal right subtree
            inorder_traversal(node.right)
                
        inorder_traversal(root)
        A.val, B.val = B.val, A.val # swap and correct the BST
```
