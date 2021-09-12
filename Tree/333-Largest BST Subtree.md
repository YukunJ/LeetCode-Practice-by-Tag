**333. Largest BST Subtree**

```Tag : tree/dfs/recursion```

**Description:**

Given the root of a binary tree, find the largest subtree, which is also a Binary Search Tree (BST), where the largest means subtree has the largest number of nodes.

A **Binary Search Tree (BST)** is a tree in which all the nodes follow the below-mentioned properties:

+ The left subtree values are less than the value of their parent (root) node's value.

+ The right subtree values are greater than the value of their parent (root) node's value.

**Note**: A subtree must include all of its descendants.

**Follow up**: Can you figure out ways to solve it with ```O(n)``` time complexity?


**Example1:**

![avatar](Fig/333-E1.jpg)

        Input: root = [10,5,15,1,8,null,7]
        Output: 3
        Explanation: The Largest BST Subtree in this case is the highlighted one. The return value is the subtree's size, which is 3.
        
**Example2:**

        Input: root = [4,2,7,2,3,5,null,2,null,null,null,null,null,1]
        Output: 2

-----------

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def largestBSTSubtree(self, root: Optional[TreeNode]) -> int:
        """
        We utilize post-order traversal technique
        One property for BST is that all values in left subtree < node.val < all values in right subtree
        We could recursively call a function to return
        1) if a subtree is BST
        2) the largest size of BST nodes in that subtree
        3) the min value in this subtree
        4) the max value in this subtree
        And we can bottom-up recursion correspondingly
        
        denote n := number of nodes in the binary tree
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        self.res = 0
        
        def recur(node: TreeNode) -> Tuple[bool, int, int, int]:
            """Helper function: recursively check if BST and update min/max values"""
            if not node:
                return True, 0, float('inf'), float('-inf')
            left_bool, left_res, left_min, left_max = recur(node.left)
            right_bool, right_res, right_min, right_max = recur(node.right)
            if left_bool and right_bool and left_max < node.val < right_min:
                self.res = max(self.res, 1+left_res+right_res)
                # here the min and max is to deal with left_node is NULL or right_node is NULL
                return True, 1+left_res+right_res, min(left_min, node.val), max(right_max, node.val)
            else:
                return False, 0, float('inf'), float('-inf')
        
        recur(root)
        return self.res
```


