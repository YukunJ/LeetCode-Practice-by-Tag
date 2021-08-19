**987. Vertical Order Traversal of a Binary Tree**

```Tag : tree/dfs/hashtable```

**Description:**

Given the ```root``` of a binary tree, calculate the **vertical order traversal** of the binary tree.

For each node at position ```(row, col)```, its left and right children will be at positions ```(row + 1, col - 1)``` and ```(row + 1, col + 1)``` respectively. The root of the tree is at ```(0, 0)```.

The **vertical order traversal** of a binary tree is a list of top-to-bottom orderings for each column index starting from the leftmost column and ending on the rightmost column. There may be multiple nodes in the same row and same column. In such a case, sort these nodes by their values.

Return the **vertical order traversal** of the binary tree.


**Example1:**

![avatar](Fig/987-E1.jpg]

		Input: root = [3,9,20,null,null,15,7]
		Output: [[9],[3,15],[20],[7]]
		Explanation:
		Column -1: Only node 9 is in this column.
		Column 0: Nodes 3 and 15 are in this column in that order from top to bottom.
		Column 1: Only node 20 is in this column.
		Column 2: Only node 7 is in this column.

**Example2:**

![avatar](Fig/987-E2.jpg]

		Input: root = [1,2,3,4,5,6,7]
		Output: [[4],[2],[1,5,6],[3],[7]]
		Explanation:
		Column -2: Only node 4 is in this column.
		Column -1: Only node 2 is in this column.
		Column 0: Nodes 1, 5, and 6 are in this column.
          	1 is at the top, so it comes first.
          	5 and 6 are at the same position (2, 0), so we order them by their value, 5 before 6.
		Column 1: Only node 3 is in this column.
		Column 2: Only node 7 is in this column.

**Example3:**

![avatar](Fig/987-E3.jpg]

		Input: root = [1,2,3,4,6,5,7]
		Output: [[4],[2],[1,5,6],[3],[7]]
		Explanation:
		This case is the exact same as example 2, but with nodes 5 and 6 swapped.
		Note that the solution remains the same since 5 and 6 are in the same location and should be ordered by their values.

-----------

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def verticalTraversal(self, root: Optional[TreeNode]) -> List[List[int]]:
        """
        This is essentially a traversal problem
        we will traversal the true while maintaining the the row and col index
        temporarily store the traversal result in tuple form (row, value) in each column
        so that if two nodes are at same row and same column, we might sort them by value. Otherwise, we just sort by row index
        notice here we first partition by column, and within that group we sort
        this is better than global sorting (column, row, value), where is for sure O(n*logn)
        we also need to keep track of the lowest col number and highest col number
        
        denote n := number of nodes in the binary tree
        Time Complexity : O( n*log(n/w) ) where w is the width of the binary tree
        Space Complexity : O(n) the dfs recursion stack takes O(height of tree) and temporary storage takes O(n)
        """
        from collections import defaultdict
        records = defaultdict(list)
        res = []
        min_col, max_col = float('inf'), float('-inf')
        
        def dfs(node: TreeNode, row:int, col:int):
            """Helper function: DFS traversal on binary tree"""
            nonlocal records, min_col, max_col
            if node:
                # update the minimum column and maximum column seen so far
                min_col = min(min_col, col)
                max_col = max(max_col, col)
                # add current node to records
                records[col].append((row, node.val))
                # further traversal
                dfs(node.left, row+1, col-1)
                dfs(node.right, row+1, col+1)
        
        dfs(root, 0, 0)
        for col in range(min_col, max_col+1):
            # sort by row index then value as required
            records[col].sort(key=lambda x: (x[0], x[1]))
            res.append([value for row, value in records[col]])
        
        return res
```