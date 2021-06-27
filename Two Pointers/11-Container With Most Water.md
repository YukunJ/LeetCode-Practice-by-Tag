**11. Container With Most Water**

```Tag: two pointers/greedy```

**Description:**

Given ```n``` non-negative integers ```a1, a2, ..., an```, where each represents a point at coordinate ```(i, ai)```. ```n``` vertical lines are drawn such that the two endpoints of the line ```i``` is at ```(i, ai)``` and ```(i, 0)```. Find two lines, which, together with the x-axis forms a container, such that the container contains the most water.

**Notice** that you may not slant the container.

**Example1**:

![avatar](Fig/11-E1.jpg)

        Input: height = [1,8,6,2,5,4,8,3,7]
        Output: 49
        Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

**Example2**:

        Input: height = [1,1]
        Output: 1
        
**Example3**:

        Input: height = [4,3,2,1,4]
        Output: 16

**Example4**:

        Input: height = [1,2,1]
        Output: 2

-----------

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        """
        There is a tradeoff between
        "the longest container" and "the highest container"
        we start with the "longest container" with two pointers at outer most
        Then, since the min-height(side1, side2) is the one actually determine the height,
        we want to "trade width for height", we move the side with smaller height in hope to find a new side with higher height
        
        denote n := len(height)
        Time Complexity : O(n) the movement of left and right sum up to n
        Space Complexity : O(1)
        """
        left, right = 0, len(height)-1
        area = 0
        while left < right:
            width = right - left
            if height[left] < height[right]:
                area = max(area, height[left] * width)
                left += 1
            else:
                area = max(area, height[right] * width)
                right -= 1
        return area
```
