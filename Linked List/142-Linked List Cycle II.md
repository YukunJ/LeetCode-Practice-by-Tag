**142. Linked List Cycle II**

```Linked List```

**Description:**

Given a linked list, return the node where the cycle begins. If there is no cycle, return ```null```.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's ```next``` pointer is connected to. **Note that** ```pos``` **is not passed as a parameter**.

**Notice** that you **should not modify** the linked list.

**Follow up**: Can you solve it using ```O(1)``` (i.e. constant) memory?

**Example1**:
![avatar](Fig/142-E1.png)

        Input: head = [3,2,0,-4], pos = 1
        Output: tail connects to node index 1
        Explanation: There is a cycle in the linked list, where tail connects to the second node.

**Example2**:
![avatar](Fig/142-E2.png)

        Input: head = [1,2], pos = 0
        Output: tail connects to node index 0
        Explanation: There is a cycle in the linked list, where tail connects to the first node.  
        
**Example3**:
![avatar](Fig/142-E3.png)

        Input: head = [1], pos = -1
        Output: no cycle
        Explanation: There is no cycle in the linked list.

-----------

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        """
        In order to get O(1) space, we need to use slow and fast pointers techniques
        
        # 1. Firstly, prove if there is a loop, fast and slow points will meet
        Assume the loop length is S 
        and when slow pointer enter the loops, the distance between fast and slow pointers is R
        for any time span t, slow pointer move t steps, fast move from the loop entrance R+2*t steps
        if they are to meet, there exists an integer n, such that R+2*t-t = R+t = nS
        Since R < S, pick n=1, there exist "t" satisfies this equation
        Therefore, slow/fast pointers method could work
        
        # 2. To find the loop entrance.
        Assume the distance before the loop is L, the place fast meets slow is S far away from loop entrance
        So when fast and slow meets, because fast move twice as much distance as slow, we must have
        2*(L+S) = L+S+nR, where R denotes the loop lengths
        This is L+S = nR
        pick n = 1, L = R-S = distance between the slow pointer to the loop entrance.
        So we can init a new pointer when fast meets slow, and the new pointer will
        meet the slow pointer at the loop entrance
        
        Time Complexity : O(n), where n = length of Listed List given, 
                        the algorithm terminates when slow pointer re-reach the loop entrance or end of the list
        Space Complexity : O(1)
        """
        if not head: # boundary case
            return None
        
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                # meets, init a new pointer at beginning
                new = head
                while new != slow:
                    new = new.next
                    slow = slow.next
                return new
        return None
```
