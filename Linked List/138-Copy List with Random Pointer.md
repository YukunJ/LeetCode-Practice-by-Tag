**138. Copy List with Random Pointer**

```Tag : linked list```

**Description:**

A linked list of length ```n``` is given such that each node contains an additional random pointer, which could point to any node in the list, or ```null```.

Construct a **deep copy** of the list. The deep copy should consist of exactly ```n``` **brand new** nodes, where each new node has its value set to the value of its corresponding original node. Both the ```next``` and ```random``` pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. **None of the pointers in the new list should point to nodes in the original list**.

For example, if there are two nodes ```X``` and ```Y``` in the original list, where ```X.random --> Y```, then for the corresponding two nodes ```x``` and ```y ```in the copied list, ```x.random --> y```.

Return the head of the copied linked list.

The linked list is represented in the input/output as a list of ```n``` nodes. Each node is represented as a pair of ```[val, random_index]``` where:

+ ```val:``` an integer representing ```Node.val```
+ ```random_index```: the index of the node (range from ```0``` to ```n-1```) that the ```random``` pointer points to, or ```null``` if it does not point to any node.

Your code will **only** be given the ```head```of the original linked list.


**Example1**:

![avatar](Fig/138-E1.png)

        Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
        Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]
        
**Example2**:      

![avatar](Fig/138-E2.png)

        Input: head = [[1,1],[2,1]]
        Output: [[1,1],[2,1]]
 
 **Example3**:      
 
 ![avatar](Fig/138-E3.png)
 
         Input: head = [[3,null],[3,0],[3,null]]
         Output: [[3,null],[3,0],[3,null]]
 
 **Example4**:      
 
         Input: head = []
         Output: []
         Explanation: The given linked list is empty (null pointer), so return null.
 
-----------

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, x: int, next: 'Node' = None, random: 'Node' = None):
        self.val = int(x)
        self.next = next
        self.random = random
"""

class Solution:
    def copyRandomList(self, head: 'Node') -> 'Node':
        """
        Following the hint, we are going to make duplicates of every old node
        in specific, we want the following structure:
        old_node_1 -> node_1_duplicate -> old_node_2 -> node_2_duplicate -> ...
        Then for index i, we can solve duplicate's random pointer as
        node_i_duplicate.random = old_node_i.random.next
        
        denote n := length of this linked list
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        if not head:
            return head
        
        curr = head
        # iterate through the linkedlist and make duplicate on the way
        while curr:
            newNode = Node(curr.val, next=curr.next)
            curr.next = newNode
            curr = curr.next.next
        
        curr = head
        # join the random pointer
        while curr:
            duplicate = curr.next
            duplicate.random = curr.random.next if curr.random else None
            curr = curr.next.next
        
        origin, duplicate = head, head.next
        duplicate_head = duplicate
        # recover the original list and duplicate list
        while origin:
            origin.next = origin.next.next
            if origin.next:
                duplicate.next = duplicate.next.next
            origin, duplicate = origin.next, duplicate.next
        
        return duplicate_head
```
