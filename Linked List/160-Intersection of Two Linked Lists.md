**160. Intersection of Two Linked Lists**

```Tag : linked list/two pointers```

**Description:**

Given the heads of two singly linked-lists ```headA``` and ```headB```, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return ```null```

For example, the following two linked lists begin to intersect at node ```c1```:

![avatar](Fig/160-Example.png)

The test cases are generated such that there are no cycles anywhere in the entire linked structure.

**Note** that the linked lists must **retain their original structure** after the function returns.

**Example1:**

![avatar](Fig/160-E1.png)

        Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
        Output: Intersected at '8'
        Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect).
        From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.

**Example2:**

![avatar](Fig/160-E2.png)

        Input: intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
        Output: Intersected at '2'
        Explanation: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect).
        From the head of A, it reads as [1,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.

**Example3:**

![avatar](Fig/160-E3.png)

        Input: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
        Output: No intersection
        Explanation: From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values.
        Explanation: The two lists do not intersect, so return null.

**Follow up**: Could you write a solution that runs in ```O(n)``` time and use only ```O(1)``` memory?

-----------

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        """
        A clever way of doing this problem is to observe that
        assume LinkedList A has length=a of pre-common part, and length=c of common part
               LinkedList B has length=b of pre-common part, and length=c of common part
        if we let list A and list B go freely until they hit the end, 
        and switch walkA to headB and walkB to headA, 
        they will go a+b+c and reach the intersection point
        
        denote na := length of LinkedListA, nb := length of LinkedListB
        Time Complexity : O(na+nb)
        Space Complexity : O(1)
        """
        walkA, walkB = headA, headB
        
        while walkA != walkB:
            walkA = walkA.next if walkA else headB
            walkB = walkB.next if walkB else headA
        
        return walkA
```

