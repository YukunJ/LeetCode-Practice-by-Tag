**449. Serialize and Deserialize BST**

```Tag: Design/BST/traversal```

**Description:**

*Serialization* is converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a **binary search tree**. There is no restriction on how your serialization/deserialization algorithm should work. You need to ensure that a binary search tree can be serialized to a string, and this string can be deserialized to the original tree structure.

**The encoded string should be as compact as possible**.

**Example1**:

		Input: root = [2,1,3]
		Output: [2,1,3]

**Example2**:

		Input: root = []
		Output: []

-----------

**Solution1: Not optimized**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Codec:
    """
    This question is strongly related to Q105. <Construct Binary Tree from Preorder and Inorder Traversal>
    From that question, we know that if given the preorder and inorder traversal list of a BST,
        we could re-construct the BST
    Here in order to serialize and deserialize the BST,
        in serialization, we just create the preorder and inorder traversal list
        in de-serialization, we follow the recursion approach in Q105
    denote n := num of nodes in the BST
    Time Complexity : O(n) for both serialize() and deserialize()
    Space Complexity : O(n) for both serialize() and deserialize()
    """
    def serialize(self, root: TreeNode) -> str:
        """
        Encodes a tree to a single string.
        We create preorder and inorder traversal list
        """
        preorder = []
        inorder = []
        def inorder_traversal(node: TreeNode):
            nonlocal inorder
            if node:
                inorder_traversal(node.left)
                inorder.append(str(node.val))
                inorder_traversal(node.right)
        
        def preorder_traversal(node: TreeNode):
            nonlocal preorder
            if node:
                preorder.append(str(node.val))
                preorder_traversal(node.left)
                preorder_traversal(node.right)
        inorder_traversal(root)
        preorder_traversal(root)

        return  ",".join(preorder) + "|" + ",".join(inorder)
        

    def deserialize(self, data: str) -> TreeNode:
        """
        Decodes your encoded data to tree.
        we rely on the structure fact that:
        preorder = [node, left-subtree, right-subtree]
        inorder = [left-subtree, node, right-subtree]
        and we solve "how long is the left-subtree" and apply recursion to construct the left and right subtree accordingly 
        """
        if data == '|': # empty tree boundary case
            return None
        preorder_str, inorder_lst = data.split('|')
        preorder, inorder = preorder_str.split(','), inorder_lst.split(',')
        preorder, inorder = [int(s) for s in preorder], [int(s) for s in inorder] # transform to int type
        n = len(preorder)
        index = {val : idx for idx, val in enumerate(inorder)} # the mapping index -> value in the inorder traversal list
        def build_tree(pre_left: int, pre_right: int, in_left: int, in_right: int) -> TreeNode:
            """Helper function: recursively build tree depending in the index range"""
            if pre_left > pre_right: # boundary case
                return None
            pre_root = pre_left
            in_root = index[preorder[pre_root]] # the index of current root node in inorder list
            left_subtree_len = in_root - in_left
            root = TreeNode(val=preorder[pre_root])
            # recursively build left and right subtree
            root.left = build_tree(pre_left+1, pre_left+left_subtree_len, in_left, in_root-1)
            root.right = build_tree(pre_left+left_subtree_len+1, pre_right, in_root+1, in_right)
            
            return root

        return build_tree(0, n-1, 0, n-1)
```

-----------

**Solution2: Time/Space optimized by a factor of 2**


```python
class Codec:
    """
    We are not utiizing the fact that a BST is "sorted", being different from a normal binary tree
	we don't need inorder traversal list here, because sorted(pre-order list/ post-order list) = inorder list
    We could optimize the space by half that we only need either pre-order list or post-order list
    For space convenience in stack.pop(), we use post-order traversal

    denote n := num of nodes in the BST, both time and space runtime reduce by a factor of 2 compared with solution 1
    Time Complexity : O(n)
    Space Complexity: O(n) 
    """
    def serialize(self, root):
        """
        Encodes a tree to a single string.
        """
        postorder_lst = []
        def postorder_traversal(node: TreeNode):
            nonlocal postorder_lst
            if node:
                postorder_traversal(node.left)
                postorder_traversal(node.right)
                postorder_lst.append(str(node.val))
        postorder_traversal(root)
        return ','.join(postorder_lst)

    def deserialize(self, data):
        """
        Decodes your encoded data to tree.
        """
        if not data: # boundary case checking
            return None
        postorder_lst = [int(s) for s in data.split(',')]

        def build_tree(low: int, high: int) -> TreeNode:
            # boundary case checking
            if not postorder_lst or postorder_lst[-1] < low or postorder_lst[-1] > high:
                return None
            root_val = postorder_lst.pop()
            root = TreeNode(val=root_val)
            root.right = build_tree(root_val, high)
            root.left = build_tree(low, root_val)
            return root 
        
        return build_tree(float('-inf'), float('inf'))
```