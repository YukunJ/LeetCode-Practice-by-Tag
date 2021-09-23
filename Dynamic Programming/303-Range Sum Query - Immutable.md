**303. Range Sum Query - Immutable**

```Tag : dynamic programming/presum/array/design```

**Description:**

Given an integer array ```nums```, handle multiple queries of the following type:

+ Calculate the **sum** of the elements of ```nums``` between indices ```left``` and ```right``` **inclusive** where ```left <= right```.

Implement the ```NumArray``` class:

+ ```NumArray(int[] nums)``` Initializes the object with the integer array ```nums```.

+ ```int sumRange(int left, int right)``` Returns the **sum** of the elements of ```nums``` between indices ```left``` and ```right``` **inclusive** (i.e. ```nums[left] + nums[left + 1] + ... + nums[right]```).

**Example1:**

        Input
        ["NumArray", "sumRange", "sumRange", "sumRange"]
        [[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
        Output
        [null, 1, -1, -3]

        Explanation
        NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
        numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1
        numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1
        numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3

-----------

```python
class NumArray:
    """
    The idea is to adopt a dynamic programming scheme using presum array
    nums[left] + ... + nums[right] = presum[right+1] - presum[left]
    where presum[i] := sum(nums[:i])
    
    denote n := len(nums)
    Time Complexity : __init__() O(n) for presum construction, O(1) for sumRange()
    Space Complexity : O(n) for storage
    """
    def __init__(self, nums: List[int]):
        self.presum = [0]
        for i in range(len(nums)):
            self.presum.append(self.presum[-1] + nums[i])

    def sumRange(self, left: int, right: int) -> int:
        return self.presum[right+1] - self.presum[left]


# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# param_1 = obj.sumRange(left,right)
```
