**173. Binary Search Tree Iterator**

```Tag: Design/BST```

**Description:**

Implement the ```BSTIterator``` class that represents an iterator over the **in-order traversal** of a binary search tree (**BST**):

+ ```BSTIterator(TreeNode root)``` Initializes an object of the ```BSTIterator``` class. The ```root``` of the BST is given as part of the constructor. The pointer should be initialized to a non-existent number smaller than any element in the BST.

+ ```boolean hasNext()``` Returns true if there exists a number in the traversal to the right of the pointer, otherwise returns ```false```.

+ ```int next()``` Moves the pointer to the right, then returns the number at the pointer.

Notice that by initializing the pointer to a non-existent smallest number, the first call to ```next()``` will return the smallest element in the BST.

You may assume that ```next()``` calls will always be valid. That is, there will be at least a next number in the in-order traversal when ```next()``` is called.

**Example1**:

![avatar](Fig/173-E1.png)

		Input
		["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]
		[[[7, 3, 15, null, null, 9, 20]], [], [], [], [], [], [], [], [], []]

		Output
		[null, 3, 7, true, 9, true, 15, true, 20, false]

		Explanation
		BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]);
		bSTIterator.next();    // return 3
		bSTIterator.next();    // return 7
		bSTIterator.hasNext(); // return True
		bSTIterator.next();    // return 9
		bSTIterator.hasNext(); // return True
		bSTIterator.next();    // return 15
		bSTIterator.hasNext(); // return True
		bSTIterator.next();    // return 20
		bSTIterator.hasNext(); // return False

**Follow up**:

+ Could you implement next() and hasNext() to run in average ```O(1)``` time and use ```O(h)``` memory, where ```h``` is the height of the tree?

-----------

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class BSTIterator:
    """
    We couldn't use traditional in-order recursion tree traversal here
    and we don't want to use O(n) space and time to do a traversal beforehand
    we want an "online" solution
    
    We rely on the traversal order "Left subtree -> Curr Node -> Right subtree"
    when we are in the right subtree, we are OK.
    But when we are in the left subtree, some time later we need to return to the "parent" node
    therefore we main a stack to store only all the left subtree's node, 
    and when we pop a node out of stack, we check if there is right subnodes
    If so, add all its left subnodes into the stack
    
    In a sense, the stack we are maintaing is "monotone stack" that 
        nodes in it are in decreasing order of height in the BST from bottom to top
        
    denote n := number of node in the BST, h := height of the BST
    Time Complexity : next(), hasNext() are O(1), __init__() is O(h)
    Space Complexity : O(h) since we store at most 1 left subnode on every level
    """
    def __init__(self, root: TreeNode):
        self.stack = []
        # init the left subnode stack
        while root:
            self.stack.append(root)
            root = root.left

    def next(self) -> int:
        node = self.stack.pop()
        walk = node.right
        while walk: # add all left-subnodes of node's right-subnode if exist
            self.stack.append(walk)
            walk = walk.left
        return node.val
            

    def hasNext(self) -> bool:
        return len(self.stack) > 0
```