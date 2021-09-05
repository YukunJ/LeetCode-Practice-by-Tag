**230. Kth Smallest Element in a BST**

```Tag : tree/bfs/dfs/stack```

**Description:**

Given the ```root``` of a binary search tree, and an integer ```k```, return the ```k-th``` (**1-indexed**) smallest element in the tree.

**Example1:**

![avatar](Fig/230-E1.jpg)

        Input: root = [3,1,4,null,2], k = 1
        Output: 1

**Example2:**

![avatar](Fig/230-E1.jpg)
		
        Input: root = [5,3,6,2,4,null,null,1], k = 3
        Output: 3
        

**Follow up**: If the BST is modified often (i.e., we can do insert and delete operations) and you need to find the ```k-th``` smallest frequently, how would you optimize?

-----------

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        """
        We adopt an iterative approach to solve this problem
        To simulate a in-order traversal by BST property
        we use a stack to simulate the process and store all left subnode first
        Then we pop a node from the stack, process it and check its right subtree
        
        denote H := height of the BST
        Time Complexity : O(H+k)
        Space Complexity : O(H)
        """
        curr = root
        stack = []
        while True:
            while curr:
                # temporary store all left subtree in the stack
                # and later process them from deep to shallow depth
                stack.append(curr)
                curr = curr.left
            curr = stack.pop()
            k -= 1
            if k == 0:
                # found the k-th smallest
                return curr.val
            else:
                # search the right subtree
                curr = curr.right
```
