**735. Asteroid Collision**

```Tag: stack/array```

**Description:**

We are given an array ```asteroids``` of integers representing asteroids in a row.

For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.

**Example1**:

		Input: asteroids = [5,10,-5]
		Output: [5,10]
		Explanation: The 10 and -5 collide resulting in 10. The 5 and 10 never collide.
 
**Example2**:

		Input: asteroids = [8,-8]
		Output: []
		Explanation: The 8 and -8 collide exploding each other.

**Example3**:

		Input: asteroids = [10,2,-5]
		Output: [10]
		Explanation: The 2 and -5 collide resulting in -5. The 10 and -5 collide resulting in 10.

**Example4**:
		
		Input: asteroids = [-2,-1,1,2]
		Output: [-2,-1,1,2]
		Explanation: The -2 and -1 are moving left, while the 1 and 2 are moving right. Asteroids moving the same direction never meet, so no asteroids will meet each other.

**Hints:**

+ Say a row of asteroids is stable. What happens when a new asteroid is added on the right?
    
-----------

```python
class Solution:
    def asteroidCollision(self, asteroids: List[int]) -> List[int]:
        """
        This is a Stack simulation problem, as indicated by the hint
        we consider a stable row of asteroids maintained with a stack and try to add a new one
        there are a few possible scenarios:
        0. stack is empty, just add the new one
        1. stack[-1] run in same direction/leaving direction(left and right) with the new one, good just add it 
        3. stack[-1] will bumpy into the new one, there are a few possible sub-scenarios:
            3#1. they are of equal mass, destory both, clear
            3#2. the new one is of smaller mass and get destoried, clear
            3#3. the stack[-1] is of smaller mass and get destoried, and this process recursively continue
                until stack is empty, or different direction met, or both exploded, or new one of smaller mass
        
        denote n := len(asteroids), since each asteroids enter and leave the stack at most once
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        def insert_asteroid(stack: List[int], new: int) -> None:
            """Helper function: deal with one new asteroid insertion(might be recursive)"""
            # case 0
            if not stack:
                stack.append(new)
                return
            else:
                # case 1: same direction, or new to the right, old to the left
                if (new * stack[-1] > 0) or (stack[-1] < 0 and new > 0):
                    stack.append(new)
                    return
                else: # case 3: bump happens
                    # case 3.1 equal mass, destory both
                    if abs(new) == abs(stack[-1]):
                        stack.pop()
                        return
                    # case 3.2 new of smaller mass, destory it
                    elif abs(new) < abs(stack[-1]):
                        return
                    # case 3.3 new of bigger mass, destory stack[-1] and continue
                    else:
                        stack.pop()
                        insert_asteroid(stack, new) # recursive
        stack = []
        for asteroid in asteroids:
            insert_asteroid(stack, asteroid)
        return stack
```