**101. Symmetric Tree**

```Tag : tree/dfs```

**Description:**

Given the ```root``` of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

**Example1:**

![avatar](Fig/101-E1.jpg)

		Input: root = [1,null,2,3]
		Output: [1,3,2]

**Example2:**
		
![avatar](Fig/101-E2.jpg)

		Input: root = [1,2,2,null,3,null,3]
		Output: false

**Follow up**: Could you solve it both recursively and iteratively?

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
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        """
        We adopt a recursion approach to solve this problem   
        denote n := number of nodes in the binary tree
        
        Time Complexity : O(n)
        Space Complexity : O(n) when the binary tree is highly skewed
        """
        def sub_isSymmetric(nodeA: TreeNode, nodeB: TreeNode) -> bool:
            """Helper function: check if two trees are symmetric"""
            if not nodeA and not nodeB:
                return True
            elif not nodeA or not nodeB:
                return False
            else:
                return nodeA.val == nodeB.val and \
                        sub_isSymmetric(nodeA.left, nodeB.right) and \
                        sub_isSymmetric(nodeA.right, nodeB.left)
        
        return sub_isSymmetric(root.left, root.right)
```

-----------

**Solution2: Iterative**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        """
        We could also adopt an iterative approach to solve this problem using BFS 
        denote n := number of nodes in the binary tree
        
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        if not root: # boundary case
            return True
        queue = [root.left, root.right]
        while queue:
            # check if current level queue is mirror symmetric
            left_ptr, right_ptr = 0, len(queue)-1
            while left_ptr < right_ptr:
                # we check if the current level is "mirror symmetric"
                if (not queue[left_ptr] and queue[right_ptr]) or \
                    (queue[left_ptr] and not queue[right_ptr]):
                    # one is None while the other is not
                    return False   
                elif queue[left_ptr] and queue[right_ptr] and not (queue[left_ptr].val == queue[right_ptr].val):
                    # both not None yet value doesn't match
                    return False
                left_ptr += 1
                right_ptr -= 1
            next_queue = []
            for node in queue:
                if node:
                    next_queue.append(node.left)
                    next_queue.append(node.right)
            queue = next_queue
            
        return True
```
