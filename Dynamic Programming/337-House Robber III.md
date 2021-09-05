**337. House Robber III**

```Tag : dynamic programming/tree/dfs```

**Description:**

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called ```root```.

Besides the ```root```, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if **two directly-linked houses were broken into on the same night**.

Given the ```root``` of the binary tree, return the maximum amount of money the thief can rob **without alerting the police**.

**Example1:**

![avatar](Fig/337-E1.jpg)

		Input: root = [3,2,3,null,3,null,1]
		Output: 7
		Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.

**Example2:**
		
![avatar](Fig/337-E2.jpg)

		Input: root = [3,4,5,1,3,null,1]
		Output: 9
		Explanation: Maximum amount of money the thief can rob = 4 + 5 = 9.

-----------

**Solution1: Dynamic Programming**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:
        """
        a more explicit dynamic programming solution would be to
        transform the tree into index-based array
        and then use two array
        dp_rob[i] := the max value starting at index i node and robbing this node
        dp_no_rob[i] := the max value starting at index i node and without robbing this node
        then dp_rob[i] = node[i].val + sum(dp_no_rob[child])
             dp_no_rob[i] = sum(max(dp_rob[child], dp_no_rob[child]))
        The the answer we are looking for is max(dp_rob[0], dp_np_rob[0])
        
        denote n := number of nodes in the binary tree
        Time Complexity : O(n)
        Space Complexity : O(n) the dp array storage
        """
        from collections import defaultdict, deque
        
        if not root: # boundary checking
            return 0
        
        # transform the tree into index-based array
        tree_val = []
        graph = defaultdict(list)
        curr_index = parent_index = -1
        queue = deque([(root, parent_index)])
        while queue:
            node, parent_index = queue.popleft()
            if node:
                curr_index += 1
                tree_val.append(node.val)
                graph[parent_index].append(curr_index)
                queue.append((node.left, curr_index))
                queue.append((node.right, curr_index))
                
        # start the dp process
        dp_rob = [0] * (curr_index+1)
        dp_no_rob = [0] * (curr_index+1)
        for i in range(curr_index, -1, -1):
            if not graph[i]: # a leaf node
                dp_rob[i] = tree_val[i]
                dp_no_rob[i] = 0
            else:
                dp_rob[i] = tree_val[i] + sum([dp_no_rob[child] for child in graph[i]])
                dp_no_rob[i] = sum([max(dp_rob[child], dp_no_rob[child]) for child in graph[i]])
        
        return max(dp_rob[0], dp_no_rob[0])
```

-----------

**Solution2: Recursion**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:
        """
        Recall that if we frame the robbery question in a linear line, 
        the state transition would be 
        dp[i] = (val[i] + dp[i-2], dp[i-1])
        where dp[i] denote the max value we can rob stopping before house i
        here since we cannot rob two directly linked node, we need to keep track of
        the dp value of its parent and grandparent, we carry the two values in recursion
        we need an extra variable to tell next level if the current level is robbed or not
        and to avoid repeative computation, we use memorization skills here
        
        denote n := number of nodes in the binary tree
        Time Complexity : O(n)
        Space Complexity : O(n) in worst case highly skewed tree
        """
        memory = [{}, {}]
        def recur(node: TreeNode, parentRobbed: bool) -> int:
            nonlocal memory
            curr_max = 0 # default value for None node
            if node:
                if node in memory[parentRobbed]:
                    # memorization based on whether the parent node is robbed
                    return memory[parentRobbed][node]
                
                not_rob_result = recur(node.left, False) + recur(node.right, False)
                rob_result = 0
                if not parentRobbed: # have the choice to rob current node
                    rob_result = node.val + recur(node.left, True) + recur(node.right, True)
                curr_max = max(not_rob_result, rob_result)
                memory[parentRobbed][node] = curr_max    
            return curr_max
        return recur(root, False)
```

