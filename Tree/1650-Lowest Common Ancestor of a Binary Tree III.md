**1650. Lowest Common Ancestor of a Binary Tree III**

```Tag : Tree/Hashtable```

**Description:**

Given two nodes of a binary tree ```p``` and ```q```, return their *lowest common ancestor (LCA)*.

Each node will have a reference to its parent node. The definition for ```Node``` is below:

	class Node {
		public int val;
		public Node left;
		public Node right;
		public Node parent;
	}

According to the **definition of LCA** on Wikipedia: "The lowest common ancestor of two nodes p and q in a tree T is the lowest node that has both p and q as descendants (where we allow **a node to be a descendant of itself**)."

**Example1:**

![avatar](Fig/1650-E1.png)

		Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
		Output: 3
		Explanation: The LCA of nodes 5 and 1 is 3.

**Example2:**

![avatar](Fig/1650-E2.png)

		Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
		Output: 5
		Explanation: The LCA of nodes 5 and 4 is 5 since a node can be a descendant of itself according to the LCA definition.

**Example3:**

		Input: root = [1,2], p = 1, q = 2
		Output: 1


-----------

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None
        self.parent = None
"""

class Solution:
    def lowestCommonAncestor(self, p: 'Node', q: 'Node') -> 'Node':
        """
        This is a simplified version of lowestCommonAncestor problem
        we have the problem node so that
        Since we know both p and q are going to reaching root if keep going upward
        their upgoing path will have join together sooner or later
        the first intersection is their common ancestor
        
        denote h := height of this binary tree
        Time Complexity : O(h)
        Space Complexity : O(h)
        """
        p_uppath = set() # use a hashset for quick membership checking later
        
        # record the upgoing path for node p
        while p:
            p_uppath.add(p)
            p = p.parent
        
        # find the first upgoing intersection point between p and q
        while q:
            if q in p_uppath:
                return q
            q = q.parent
```