**384. Shuffle an Array**

```Tag : design/math/array```

**Description:**

Given an integer array ```nums```, design an algorithm to randomly shuffle the array. All permutations of the array should be **equally likely** as a result of the shuffling.

Implement the ```Solution``` class:

+ ```Solution(int[] nums)``` Initializes the object with the integer array nums.

+ ```int[] reset()``` Resets the array to its original configuration and returns it.

+ ```int[] shuffle()``` Returns a random shuffling of the array.

**Example1:**

        Input
        ["Solution", "shuffle", "reset", "shuffle"]
        [[[1, 2, 3]], [], [], []]
        Output
        [null, [3, 1, 2], [1, 2, 3], [1, 3, 2]]

        Explanation
        Solution solution = new Solution([1, 2, 3]);
        solution.shuffle();    // Shuffle the array [1,2,3] and return its result.
                               // Any permutation of [1,2,3] must be equally likely to be returned.
                               // Example: return [3, 1, 2]
        solution.reset();      // Resets the array back to its original configuration [1,2,3]. Return [1, 2, 3]
        solution.shuffle();    // Returns the random shuffling of array [1,2,3]. Example: return [1, 3, 2]

-----------

**Solution1: Fisher-Yates**

```python
class Solution:
    """
    We adopt Fisher-Yates in-place version algorithm here
    essentially we will randomly pick an element from the bag without replacement
    The probability of each item at each iteration being chosen is the same
    Assume at iteration k, the probability of an item e being chosen is:
    P(e chosen at iter k) = P(e being picked at iteration k) * P(e not being picked before iter k)
                          = 1/n-k * n-1/n * n-2/n-1 * ... * n-k/n-k+1 = 1/n
    
    denote n := len(nums)
    Time Complexity : O(n) one pass-through swapping
    Space Complexity : O(n) for original array storage
    """
    def __init__(self, nums: List[int]):
        self.curr = nums
        self.original = list(nums)

    def reset(self) -> List[int]:
        return self.original
    
    def shuffle(self) -> List[int]:
        def swap(array, i, j):
            # Helper function: swapping two entries in an array
            array[i], array[j] = array[j], array[i]
            
        n = len(self.curr)
        for i in range(n):
            idx = random.randint(i, n-1)
            swap(self.curr, i, idx)
        return self.curr
```

