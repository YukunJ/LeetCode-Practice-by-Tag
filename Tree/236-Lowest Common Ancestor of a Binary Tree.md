**236. Lowest Common Ancestor of a Binary Tree**

```Tag: Tree```

**Description:**

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the ```definition of LCA on Wikipedia```: “The lowest common ancestor is defined between two nodes ```p``` and ```q``` as the lowest node in ```T``` that has both ```p``` and ```q``` as descendants (where we allow **a node to be a descendant of itself**).”

**Example1**:

![avatar](Fig/263-E1.jpeg)

        Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
        Output: 3
        Explanation: The LCA of nodes 5 and 1 is 3.

 **Example2**:

![avatar](Fig/263-E2.jpeg)

        Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
        Output: 5
        Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.

 **Example3**:

        Input: root = [1,2], p = 1, q = 2
        Output: 1

-----------

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        """
        This is a tree recursion problem.
        A clear thinking is that, for a node, if p and q are in its left and right (or right and left) subtrees,
        then this node is the LCA we are looking for
        If the node is p or q itself, and we haven't found LCA, then p or q itself is the LCA for p and q two nodes
        we recursively call LCA function on node.left and node.right
        1) if node is Null or node == p or node ==q : then just return node
        2) if left and right both null : mean p and q are not under the subtree with root current node return Null
        3) if left = null, right not null: mean the LCA is found on the right hand side, return right
        4) if left not null, right = null: similar to 3) return left
        5) both left and right not null, return node

        denote N := number of nodes in the binary tree
        Time Complexity : O(N) 
        Space Complexity : O(N)
        """
        if not root or root == p or root == q: # 1)
            return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if not left and not right: # 2)
            return None
        elif not left: # 3)
            return right
        elif not right: # 4)
            return left
        else: # 5)
            return root 
```
