**95. Unique Binary Search Trees II**

```Tag: dynamic programming```

**Description:**

Given an integer ```n```, return all the structurally unique **BST**'s (binary search trees), which has exactly ```n``` nodes of unique values from ```1``` to ```n```. Return the answer in **any order**.

**Example1**:

![avatar](Fig/95-E1.jpg)

        Input: n = 3
        Output: [[1,null,2,null,3],[1,null,3,2],[2,1,3],[3,1,null,null,2],[3,2,null,1]]

**Example2**:

        Input: n = 1
        Output: [[1]]

-----------

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def generateTrees(self, n: int) -> List[TreeNode]:
        """
        we condition on the root node value
        by the property of binary search tree, 
        left-subtree < node and right-subtree > node
        we can recursion on [1, node-1] and [node+1, n]
        and combine them into a tree
        
        The number of possible Tree is associated with Catalan number of size O(4^n/ n^{1.5}), each tree has n node, therefore we get:
        Time Complexity : O(4^n/n^{0.5})
        Space Complexity : O(4^n/n^{0.5})
        """
        def recur(start: int, end:int) -> List[TreeNode]:
            res = []
            if start > end:
                return [None]
            for i in range(start, end+1):
                left_subtree = recur(start, i-1) # all possible tree of range [start, i-1]
                right_subtree = recur(i+1, end) # all possible tree of range [start, i-1]
                for left in left_subtree:
                    for right in right_subtree:
                        root = TreeNode(i) # generate an answer
                        root.left, root.right = left, right
                        res.append(root)
            return res
        
        return recur(1, n)
```
