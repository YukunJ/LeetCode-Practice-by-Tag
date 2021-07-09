**341. Flatten Nested List Iterator**

```Tag: Design/DFS```

**Description:**

You are given a nested list of integers ```nestedList```. Each element is either an integer or a list whose elements may also be integers or other lists. Implement an iterator to flatten it.

Implement the ```NestedIterator``` class:

+ ```NestedIterator(List<NestedInteger> nestedList)``` Initializes the iterator with the nested list nestedList.

+ ```int next()``` Returns the next integer in the nested list.

+ ```boolean hasNext()``` Returns ```true``` if there are still some integers in the nested list and ```false``` otherwise.

Your code will be tested with the following pseudocode:

	initialize iterator with nestedList
	res = []
	while iterator.hasNext()
    	append iterator.next() to the end of res
	return res

If ```res``` matches the expected flattened list, then your code will be judged as correct.

**Example1**:

	Input: nestedList = [[1,1],2,[1,1]]
	Output: [1,1,2,1,1]
	Explanation: By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,1,2,1,1].
 
**Example2**:

	Input: nestedList = [1,[4,[6]]]
	Output: [1,4,6]
	Explanation: By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,4,6].


-----------

```python
# """
# This is the interface that allows for creating nested lists.
# You should not implement it, or speculate about its implementation
# """
#class NestedInteger:
#    def isInteger(self) -> bool:
#        """
#        @return True if this NestedInteger holds a single integer, rather than a nested list.
#        """
#
#    def getInteger(self) -> int:
#        """
#        @return the single integer that this NestedInteger holds, if it holds a single integer
#        Return None if this NestedInteger holds a nested list
#        """
#
#    def getList(self) -> [NestedInteger]:
#        """
#        @return the nested list that this NestedInteger holds, if it holds a nested list
#        Return None if this NestedInteger holds a single integer
#        """

class NestedIterator:
    """
    We use a 'recursive' structure to solve the problem
    we 'do' real operation only if we are at a 'Integer' type,
    otherwise we just pass control to recursion and update the recursor

    denote n := num of elements in the nestedList
    Time Complexity := init O(1), next O(1), hasNext O(1)* (amortized)
    Space Complexity := O(n) in worst case if it's a linked list structure
    """
    def __init__(self, nestedList: [NestedInteger]):
        self.nestedList = nestedList
        self.size = len(nestedList) if nestedList else 0
        self.iterator = None 
        self.ptr = 0
        
    def next(self) -> int:
        # the current position in this level of nested list
        cursor = self.nestedList[self.ptr]
        if cursor.isInteger():
            self.ptr += 1
            return cursor.getInteger()
        else:
            # pass control to this position's nested structure
            return self.iterator.next()
        
    def hasNext(self) -> bool:
        while self.ptr < self.size:
            cursor = self.nestedList[self.ptr]
            if cursor.isInteger():
                return True
            else:
                # the current position is nested list, go deeper
                if self.iterator == None: # ini iterator
                    self.iterator = NestedIterator(cursor.getList())
                if self.iterator.hasNext():
                    # pass control to next level's nested structure
                    return True
                else:
                    # this cursor is bad, go next one
                    self.iterator = None
                    self.ptr += 1
        return False
	
# Your NestedIterator object will be instantiated and called as such:
# i, v = NestedIterator(nestedList), []
# while i.hasNext(): v.append(i.next())
```
