**708. Insert into a Sorted Circular Linked List**

```Tag : linked list```

**Description:**

Given a Circular Linked List node, which is sorted in ascending order, write a function to insert a value ```insertVal``` into the list such that it remains a sorted circular list. The given node can be a reference to any single node in the list and may not necessarily be the smallest value in the circular list.

If there are multiple suitable places for insertion, you may choose any place to insert the new value. After the insertion, the circular list should remain sorted.

If the list is empty (i.e., the given node is ```null```), you should create a new single circular list and return the reference to that single node. Otherwise, you should return the originally given node.

**Example1:**

![avatar](Fig/708-E1.jpg)

		Input: head = [3,4,1], insertVal = 2
		Output: [3,4,1,2]
		Explanation: In the figure above, there is a sorted circular list of three elements. You are given a reference to the node with value 3, 
		and we need to insert 2 into the list. The new node should be inserted between node 1 and node 3. 		After the insertion, the list should look like this, and we should still return node 3.

![avatar](Fig/708-E1-1.jpg)

**Example2:**

		Input: head = [], insertVal = 1
		Output: [1]
		Explanation: The list is empty (given head is null). We create a new single circular list and return the reference to that single node.

**Example3:**

		Input: head = [1], insertVal = 0
		Output: [1,0]

-----------

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, next=None):
        self.val = val
        self.next = next
"""

class Solution:
    def insert(self, head: 'Node', insertVal: int) -> 'Node':
        """
        The rough idea in this problem is to maintain two pointers to traversal the list
        However, there are many tricky boundary cases we need to cater to:
        1) if prev.val <= insertVal <= curr.val, good, insert it
        2) if insertVal >= maxvalue or insertVal <= minvalue, insert after the tail
        3) the value in linked list is uniform, insert it anywhere
        
        denote n := length of this circular linked list
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        if not head:
            # empty linked list 
            newHead = Node(val=insertVal)
            newHead.next = newHead # self-circular
            return newHead
        
        prev, curr = head, head.next
        insert = False
        while True:
            if prev.val <= insertVal <= curr.val:
                # case (1)
                insert = True
            elif prev.val > curr.val:
                # tail find, case (2)
                if insertVal >= prev.val or insertVal <= curr.val:
                    insert = True
            
            if insert:
                prev.next = Node(val=insertVal, next=curr)
                return head
            
            prev, curr = curr, curr.next
            if prev == head:
                # whole loop finish
                break
        
        # reaching here means case (3)
        prev.next = Node(val=insertVal, next=curr)
        return head
```