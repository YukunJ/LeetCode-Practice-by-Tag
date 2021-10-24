**528. Random Pick with Weight**

```Tag : Binary Search/Presum```

**Description:**

You are given an array of positive integers ```w``` where ```w[i]``` describes the weight of ``i-th`` index (0-indexed).

We need to call the function ```pickIndex()``` which **randomly** returns an integer in the range ```[0, w.length - 1]```. ```pickIndex()``` should return the integer proportional to its weight in the ```w``` array. For example, for ```w = [1, 3]```, the probability of picking the index ```0``` is ```1 / (1 + 3) = 0.25``` (i.e ```25%```) while the probability of picking the index ```1``` is ```3 / (1 + 3) = 0.75``` (i.e ```75%```).

More formally, the probability of picking index ```i``` is ```w[i] / sum(w)```.

**Example1**:

        Input
        ["Solution","pickIndex"]
        [[[1]],[]]
        Output
        [null,0]

        Explanation
        Solution solution = new Solution([1]);
        solution.pickIndex(); // return 0. Since there is only one single element on the array the only option is to return the first element.
        
**Example2**:      

        Input
        ["Solution","pickIndex","pickIndex","pickIndex","pickIndex","pickIndex"]
        [[[1,3]],[],[],[],[],[]]
        Output
        [null,1,1,1,1,0]

        Explanation
        Solution solution = new Solution([1, 3]);
        solution.pickIndex(); // return 1. It's returning the second element (index = 1) that has probability of 3/4.
        solution.pickIndex(); // return 1
        solution.pickIndex(); // return 1
        solution.pickIndex(); // return 1
        solution.pickIndex(); // return 0. It's returning the first element (index = 0) that has probability of 1/4.

        Since this is a randomization problem, multiple answers are allowed so the following outputs can be considered correct :
        [null,1,1,1,1,0]
        [null,1,1,1,1,1]
        [null,1,1,1,0,0]
        [null,1,1,1,0,1]
        [null,1,0,1,0,0]
        ......
        and so on.
        
-----------


```python
class Solution:
    """
    denote n := len(w)
    Time Complexity : O(n) for the __init__() for presum vector construction, 
                      O(logn) for every pickIndex() because of binary search
    Space Complexity : O(n) for __init__() for the presum vector and w itself storage
                       O(1) for every pickIndex()
    """
    def __init__(self, w: List[int]):
        """
        Record the w, total weight, presum weight
        """
        self.w = w
        self.presum = [0]
        for weight in self.w:
            self.presum.append(self.presum[-1]+weight)
        self.total = self.presum[-1]

    def pickIndex(self) -> int:
        """
        we will generate a random number x between [1, self.total]
        and we use binary search in the self.presum array to find a index i 
        such that self.presum[i] < x <= self.presum[i+1]
        then we can return i as the "randomly generated"
        because here the "probability" is related to the length of its presum interval
        """
        x = random.randint(1, self.total)
        left, right = 0, len(self.presum)-1
        while left <= right:
            mid = left + (right-left) // 2
            if self.presum[mid] < x <= self.presum[mid+1]:
                return mid
            elif self.presum[mid] >= x:
                right = mid - 1
            else: # self.presum[mid+1] < x
                left = mid + 1
        


# Your Solution object will be instantiated and called as such:
# obj = Solution(w)
# param_1 = obj.pickIndex()
```
