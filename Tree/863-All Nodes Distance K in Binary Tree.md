**863. All Nodes Distance K in Binary Tree**

```Tag: Tree/DFS/BFS```

**Description:**

We are given a binary tree (with root node ```root```), a ```target``` node, and an integer value ```k```.

Return a list of the values of all nodes that have a distance ```k``` from the ```target``` node.  The answer can be returned in any order.

**Example1**:

![avatar](Fig/863-E1.png)

        Input: root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, k = 2

        Output: [7,4,1]

        Explanation: 
        The nodes that are a distance 2 from the target node (with value 5)
        have values 7, 4, and 1.
        
        Note that the inputs "root" and "target" are actually TreeNodes.
        The descriptions of the inputs above are just serializations of these objects.
        
-----------

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def distanceK(self, root: TreeNode, target: TreeNode, k: int) -> List[int]:
        
```
