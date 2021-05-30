**96. Unique Binary Search Trees**

```Tag: dynamic programming```

**Description:**

Given an integer ```n```, return the number of structurally unique ***BST***'s (binary search trees) which has exactly ```n``` nodes of unique values from ```1``` to ```n```.

**Example1**:

![avatar](Fig/96-E1.jpg)

        Input: n = 3
        Output: 5

**Example2**:

        Input: n = 1
        Output: 1

-----------

```python
class Solution:
    def numTrees(self, n: int) -> int:
        """
        We use a dynamic programming scheme
        Denote G(n) := number of distinct binary tree 1,2,..,n
        And F(i,n) := number of distinct binary tree 1,2,...n, but with i being root
        We could write the state transition formula:
            G(n) = \sum_{i=1}^{n} F(i,n)
                 = \sum_{i=1}^{n} G(i-1) * G(n-i)
        and the initial state is G(0) = G(1) = 1, empty or single root
        
        Time Complexity : O(n^2) nested loop
        Space Complexity : O(n) to store the G dp vector
        """
        
        # initialization and boundary checking
        if n <= 1:
            return n
        G = [0] * (n+1)
        G[0], G[1] = 1, 1
        
        # start DP programming
        for i in range(2, n+1):
            for j in range(1, i+1):
                G[i] += G[j-1] * G[i-j]
        
        return G[n]
```
