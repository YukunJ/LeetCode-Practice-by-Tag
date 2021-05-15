**114. Flatten Binary Tree to Linked List**

```Tag: DFS```

**Description:**

Given the ```root``` of a binary tree, flatten the tree into a "linked list":

The "linked list" should use the same ```TreeNode``` class where the ```right``` child pointer points to the next node in the list and the ```left``` child pointer is always null.

The "linked list" should be in the same order as a ```pre-order traversal``` of the binary tree.

**Follow up**: Can you flatten the tree in-place (with ```O(1)``` extra space)?

**Example1**:

![avatar](Fig/114-E1.jpg)

        Input: root = [1,2,5,3,4,null,6]
        Output: [1,null,2,null,3,null,4,null,5,null,6]

**Example2**:

        Input: root = []
        Output: []
        
**Example3**:

        Input: root = [0]
        Output: [0]

-----------

**Solution1: DFS Recursion**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def flatten(self, root: TreeNode) -> None:
        """
        denote n = # of node in the binary tree
        Time Complexity : O(n) need to visit each node once
        Space Complexity : O(n) by recursion stack
        """
        # use a global variable to record the last visited node
        self.prev = None 
        
        def recur(node: TreeNode):
            if not node:
                return
            if self.prev:
                self.prev.right = node
            # record the original left and right subtree first
            left, right = node.left, node.right
            node.left = None
            self.prev = node
            recur(left)
            recur(right)
            
        recur(root)
```

-----------

**Solution2: In-place Iterative**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def flatten(self, root: TreeNode) -> None:
        """
        The reason why we need recursion before is that,
        when we iterate in the pre-order traversal manner,
        we lose the current node's right subtree as we go deeper
        The remedy is to find the rightmost node in current node's left subtree
        and make its "next" field point to the current node's right subtree
        
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        curr = root
        while curr:
            if curr.left:
                prev = next = curr.left
                while prev.right:
                    prev = prev.right
                prev.right = curr.right
                curr.left = None
                curr.right = next
            curr = curr.right
        return 
```
