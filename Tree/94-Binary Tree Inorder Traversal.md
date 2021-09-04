**94. Binary Tree Inorder Traversal**

```Tag : tree/dfs/Morris's algorithm```

**Description:**

Given the ```root``` of a binary tree, return the inorder traversal of its nodes' values.

**Example1:**

![avatar](Fig/94-E1.jpg)

		Input: root = [1,null,2,3]
		Output: [1,3,2]

**Example2:**
		
		Input: root = []
		Output: []

**Example3:**

		Input: root = [1]
		Output: [1]

**Example4:**


![avatar](Fig/94-E4.jpg)

		Input: root = [1,2]
		Output: [2,1]

**Example5:**


![avatar](Fig/94-E5.jpg)

		Input: root = [1,null,2]
		Output: [1,2]

-----------

**Solution1: Iterative**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        """
        We try to achieve an iterative version of inorder traversal using stack
        while there is still left subtree for a node, we want to delay processing this node
        and save it for later
        Therefore we use a stack to store the 'left' node we encounter so far
        when the stack pops out a node, it means its left subtree is properly done
        then we add its value to the result and move to its right subtree
        
        denote n := number of node in the binary tree
        Time Complexity : O(n)
        Space Complexity: O(1)
        """
        stack = []
        res = []
        curr = root
        while curr or stack:
            while curr:
                stack.append(curr)
                curr = curr.left
            curr = stack.pop()
            res.append(curr.val)
            curr = curr.right
        return res
```

-----------

**Solution2: Morris's Algorithm**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        """
        We could also apply Morris's traversal algorithm
        
        denote n := number of node in the binary tree
        Time Complexity : O(n)
        Space Complexity: O(1)
        """
        res = []
        prev = None
        curr = root
        while curr:
            if not curr.left:
                # left subtree is clear, directly store value and go right subtree
                res.append(curr.val)
                curr = curr.right
            else:
                # the ultimate goal is that every node only use its right node
                # to advance to next position
                # the left node should be discarded for usage
                rightmost_node = curr.left
                while rightmost_node.right:
                    rightmost_node = rightmost_node.right
                rightmost_node.right = curr # threading it back upward
                next = curr.left
                curr.left = None # cut off the linking 
                curr = next
        return res
```
