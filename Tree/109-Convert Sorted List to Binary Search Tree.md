**109. Convert Sorted List to Binary Search Tree**

```Tag: Tree/DFS```

**Description:**

Given the ```head``` of a singly linked list where elements are sorted in **ascending order**, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

**Example1**:

![avatar](Fig/109-E1.jpg)

        Input: head = [-10,-3,0,5,9]
        Output: [0,-3,9,-10,null,5]
        Explanation: One possible answer is [0,-3,9,-10,null,5], which represents the shown height balanced BST.

**Example2**:

        Input: head = []
        Output: []

**Example3**:

        Input: head = [0]
        Output: [0]

**Example4**:

        Input: head = [1,3]
        Output: [3,1]
        
-----------

**Solution1: Divide and Conquer**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedListToBST(self, head: ListNode) -> TreeNode:
        """
        we use a divide and conquer approach, much similar to binary search
        we divide the array by half, take the middle point as the root node
        and recursively build the left subtree and right subtree
        denote n := length of the linked list
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        # use a list to store the ascending array
        values = []
        while head:
            values.append(head.val)
            head = head.next
            
        def build_tree(start: int, end: int) -> TreeNode:
            """Helper function: build the binary search tree recursively 
                                so that it's height-balanced"""
            if start > end: # beyond the boundary
                return None
            mid = start + (end - start) // 2
            root = TreeNode(val=values[mid])
            root.left = build_tree(start, mid-1)
            root.right = build_tree(mid+1, end)
            return root
        
        return build_tree(0, len(values)-1)
```

-----------

**Solution2: Inorder Traversal**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedListToBST(self, head: ListNode) -> TreeNode:
        """
        use inorder traversal to simulate
        
        denote n := length of the linked list
        Time Complexity : O(n)
        Space Complexity : O(logn) since it's well balanced
        """
        def get_length(node: ListNode) -> int:
            l = 0
            while node:
                l += 1
                node = node.next
            return l
        
        def build_tree(start: int, end: int) -> TreeNode:
            if start > end:
                return None
            nonlocal head
            mid = start + (end - start) // 2
            # do not fill the value right away, wait until left subtree filled
            node = TreeNode() # place holder
            node.left = build_tree(start, mid-1)
            node.val = head.val
            head = head.next
            node.right = build_tree(mid+1, end)
            return node 
        
        return build_tree(0, get_length(head)-1)
```
