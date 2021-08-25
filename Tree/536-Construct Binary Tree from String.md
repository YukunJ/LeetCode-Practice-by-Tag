**536. Construct Binary Tree from String**

```Tag : tree/recursion/dfs/string```

**Description:**

You need to construct a binary tree from a string consisting of parenthesis and integers.

The whole input represents a binary tree. It contains an integer followed by zero, one or two pairs of parenthesis. The integer represents the root's value and a pair of parenthesis contains a child binary tree with the same structure.

You always start to construct the **left** child node of the parent first if it exists.

**Example1:**

![avatar](Fig/536-E1.jpg)

		Input: s = "4(2(3)(1))(6(5))"
		Output: [4,2,6,3,1,5]

**Example2:**

		Input: s = "4(2(3)(1))(6(5)(7))"
		Output: [4,2,6,3,1,5,7]

**Example3:**

		Input: s = "-4(2(3)(1))(6(5)(7))"
		Output: [-4,2,6,3,1,5,7]

-----------

**Solution1: Recursion**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def str2tree(self, s: str) -> Optional[TreeNode]:
        """
        To break the problem, we analyze that the string is recursively given as:
        [node value] + '(' + [left subtree content] + ')' + '(' + [right subtree content] + ')'
        denote n := len(s), h := height of binary tree
        Time Complexity : O(n) we process each character in s exactly once
        Space Complexity : O(h) for the recursion stack, in worst case could be O(n)
        """
        def retrieve_number(s: str, idx: int) -> Tuple[int, int]:
            """Helper function: retrieve a node's value and return the next idx"""
            sign = 1
            curr = 0
            if s[idx] == '-':
                idx += 1
                sign = -1
            while idx < len(s) and s[idx].isdigit():
                curr = 10 * curr + int(s[idx])
                idx += 1
            return sign*curr, idx
        
        def build_tree(s: str, idx: int) -> Tuple[TreeNode, int]:
            """Helper function: recursively build the tree"""
            if idx == len(s):
                return None, idx
            val, idx = retrieve_number(s, idx)
            node = TreeNode(val) # build current node
            # just be careful below how to handle the '(' and ')' by idx+1
            # we need to help the recursion to skip over the parenthesis
            if idx < len(s) and s[idx] == '(':
                node.left, idx = build_tree(s, idx+1)
            if node.left and idx < len(s) and s[idx] == '(':
                node.right, idx = build_tree(s, idx+1)
            # every node except the root itself is enclosed by a ')'
            # when you are done with your part, need to clean the ')' up
            # and turn the idx to next possibly '('
            return node, idx + 1 if idx < len(s) else idx
        
        return build_tree(s, 0)[0]
```

-----------

**Solution2: Iterative**

```python
class Solution:
    def str2tree(self, s: str) -> Optional[TreeNode]:
        """
        To iterative solve this problem, we use a stack
        we define 3 states for a node:
        1) created but not insert value
        2) value inserted and left node created
        3) left done and to deal with right part
        In our iterative approach, we will implicitly transfer a node between these 3 states
        
        Time Complexity : O(n) we process each character in s exactly once
        Space Complexity : O(h) for the recursion stack, in worst case could be O(n)
        """
        def retrieve_number(s: str, idx: int) -> Tuple[int, int]:
            """Helper function: retrieve a node's value and return the next idx"""
            sign = 1
            curr = 0
            if s[idx] == '-':
                idx += 1
                sign = -1
            while idx < len(s) and s[idx].isdigit():
                curr = 10 * curr + int(s[idx])
                idx += 1
            return sign*curr, idx
        
        if not s:
            return None
        idx = 0
        root = TreeNode()
        stack = [root]
        while idx < len(s):
            node = stack.pop() # the current node we are considering
            
            if s[idx].isdigit() or s[idx] == '-':
                # means we are in state 1, node's value not retrieved
                node.val, idx = retrieve_number(s, idx)
                # check for a possible left subtree
                if idx < len(s) and s[idx] == '(':
                    node.left = TreeNode()
                    stack.append(node)
                    stack.append(node.left)
            
            elif node.left and s[idx] == '(':
                # left done and to process right subtree
                node.right = TreeNode()
                stack.append(node)
                stack.append(node.right)
            
            idx += 1
        return root
```
