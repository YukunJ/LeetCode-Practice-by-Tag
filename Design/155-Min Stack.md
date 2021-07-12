**155. Min Stack**

```Tag: Design/Stack```

**Description:**

Design a stack that supports push, pop, top, and retrieving the minimum element in constant ```O(1)``` time.

Implement the ```MinStack``` class:

+ ```MinStack()``` initializes the stack object.

+ ```void push(val)``` pushes the element val onto the stack.

+ ```void pop()``` removes the element on the top of the stack.

+ ```int top()``` gets the top element of the stack.
int getMin() retrieves the minimum element in the stack.

**Example1**:

		Input
		["MinStack","push","push","push","getMin","pop","top","getMin"]
		[[],[-2],[0],[-3],[],[],[],[]]

		Output
		[null,null,null,null,-3,null,0,-2]

		Explanation
		MinStack minStack = new MinStack();
		minStack.push(-2);
		minStack.push(0);
		minStack.push(-3);
		minStack.getMin(); // return -3
		minStack.pop();
		minStack.top();    // return 0
		minStack.getMin(); // return -2

-----------

```python
class MinStack:
    """
    The problem is to implement a normal stack plus the functionality that
    retrieve the minimum element in the stack in constant O(1) time
    we maintain another stack to store and retrieve the min elements

    denote n := number of elements in MinStack at that momoment
    Time Complexity : all of __init__(), pop(), push(), getMin() are O(1)
    Space Complexity : O(n) for storage
    """
    def __init__(self):
        """
        initialize your data structure here.
        """
        from collections import deque
        self.stack = [] # the normal stack storage
        self.window = [] # the stack for minimun retrieve
        
    def push(self, val: int) -> None:
        self.stack.append(val) # normal stack push
        if not self.window or self.window[-1] >= val: # updating min stack
            self.window.append(val)

    def pop(self) -> None:
        pop_val = self.stack.pop()
        if self.window[-1] == pop_val: # the min value is poped out, need updating
            self.window.pop()

    def top(self) -> int:
        return self.stack[-1] # normal stack top

    def getMin(self) -> int:
        return self.window[-1] # the top element in min stack is Min    
```