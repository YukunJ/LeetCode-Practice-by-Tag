**1762. Buildings With an Ocean View**

```Tag : stack/monotone stack```

**Description:**

There are ```n``` buildings in a line. You are given an integer array ```heights``` of size ```n``` that represents the heights of the buildings in the line.

The ocean is to the right of the buildings. A building has an ocean view if the building can see the ocean without obstructions. Formally, a building has an ocean view if all the buildings to its right have a **smaller** height.

Return a list of indices **(0-indexed)** of buildings that have an ocean view, sorted in increasing order.

**Example1:**

		Input: heights = [4,2,3,1]
		Output: [0,2,3]
		Explanation: Building 1 (0-indexed) does not have an ocean view because building 2 is taller.

**Example2:**

		Input: heights = [4,3,2,1]
		Output: [0,1,2,3]
		Explanation: All the buildings have an ocean view.

**Example3:**

		Input: heights = [1,3,2,4]
		Output: [3]
		Explanation: Only building 3 has an ocean view.

**Example4:**

		Input: heights = [2,2,2,2]
		Output: [3]
		Explanation: Buildings cannot see the ocean if there are buildings of the same height to its right.


-----------

```python
class Solution:
    def findBuildings(self, heights: List[int]) -> List[int]:
        """
        This is a question of monotone stack
        starting from the rightmost closest to the ocean,
        whether a building can see the ocean depends on the highest building height to its right
        Therefore this is essentially 'one-element' monotone stack
        
        denote n := len(heights)
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        highest = float('-inf') # the rightmost building can always see the ocean
        ans = []
        ptr = len(heights) - 1
        while ptr >= 0:
            if heights[ptr] > highest:
                ans.append(ptr)
                highest = heights[ptr]
            ptr -= 1
        ans.reverse() # return in ascending order
        return ans
```