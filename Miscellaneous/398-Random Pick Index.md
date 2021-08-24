**398. Random Pick Index**

```Tag : reservoir sampling/hashtable```

**Description:**

Given an integer array ```nums``` with possible **duplicates**, randomly output the index of a given ```target``` number. You can assume that the given target number must exist in the array.

Implement the ```Solution``` class:

+ ```Solution(int[] nums)``` Initializes the object with the array ```nums```.

+ ```int pick(int target)``` Picks a random index ```i``` from nums where ```nums[i] == target```. If there are multiple valid i's, then each index should have an equal probability of returning.

**Example1:**

		Input
		["Solution", "pick", "pick", "pick"]
		[[[1, 2, 3, 3, 3]], [3], [1], [3]]
		Output
		[null, 4, 0, 2]

		Explanation
		Solution solution = new Solution([1, 2, 3, 3, 3]);
		solution.pick(3); // It should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.
		solution.pick(1); // It should return 0. Since in the array only nums[0] is equal to 1.
		solution.pick(3); // It should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.

-----------

**Solution1: Hashmap**

```python
class Solution:
    from collections import defaultdict
    """
    we pre-compute the num <-> index mapping and cache it for later look up
    denote n := len(nums)
    Time Complexity : O(n) for __init__(), O(1) for pick()
    Space Complexity : O(n) for hashmap storage
    """
    def __init__(self, nums: List[int]):
        self.num2idx = defaultdict(list)
        for idx, num in enumerate(nums):
            self.num2idx[num].append(idx)

    def pick(self, target: int) -> int:
        duplicate = len(self.num2idx[target])
        return self.num2idx[target][random.randint(0, duplicate-1)]

```

-----------

**Solution2: Reservoir Sampling**

```
class Solution:
    """
    denote n := len(nums)
    Time Complexity : O(1) for __init__(), O(n) for pick()
    Space Complexity : O(1)
    """
    def __init__(self, nums: List[int]):
        self.nums = nums

    def pick(self, target: int) -> int:
        """
        If the dataset is really big that cannot fit into the memory at once
        we should not try to use a hashmap to store all index appearance
        Instead, we try a reservoir sampling algorithm:
        to pick k elements from n with equal probability:
        we first pick first k elements
        the for k+1 th element, make a random number x between [1, k+1],
        if x fall in [1, k], we range the x-th element in the reservoir
        Then for any elements,
        1) if it's the first n elements it's probability of being chosen is
        1 * (1-1/k+1) * (1-1/k+2) * ...* (1-1/n) = k/n 
        2) similarly
        """
        index = None
        count = 0
        n = len(self.nums)
        for idx, num in enumerate(self.nums):
            # assume there are 10 target number in the nums array
            # the i-th has probability 1/i to be picked
            # and its probability not to be replaced afterwards is
            # 5/6 * 6/7 * ... * 9/10
            # so in total its being picked probability is 1/10
            if num == target:
                count += 1
                if random.randint(1,count)  == count:
                    index = idx
        return index
```