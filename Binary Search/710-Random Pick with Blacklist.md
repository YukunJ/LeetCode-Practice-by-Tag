**710. Random Pick with Blacklist**

```Tag : binary search/hashtable/sorting```

**Description:**

You are given an integer ```n``` and an array of **unique** integers ```blacklist```. Design an algorithm to pick a random integer in the range ```[0, n - 1]``` that is **not** in ```blacklist```. Any integer that is in the mentioned range and not in ```blacklist``` should be **equally likely** to be returned.

Optimize your algorithm such that it minimizes the number of calls to the **built-in** random function of your language.

Implement the ```Solution``` class:

+ ```Solution(int n, int[] blacklist)``` Initializes the object with the integer ```n``` and the blacklisted integers ```blacklist```.

+ ```int pick()``` Returns a random integer in the range ```[0, n - 1]``` and not in ```blacklist```.

**Example1:**

        Input
        ["Solution", "pick", "pick", "pick", "pick", "pick", "pick", "pick"]
        [[7, [2, 3, 5]], [], [], [], [], [], [], []]
        Output
        [null, 0, 4, 1, 6, 1, 0, 4]

        Explanation
        Solution solution = new Solution(7, [2, 3, 5]);
        solution.pick(); // return 0, any integer from [0,1,4,6] should be ok. Note that for every call of pick,
                         // 0, 1, 4, and 6 must be equally likely to be returned (i.e., with probability 1/4).
        solution.pick(); // return 4
        solution.pick(); // return 1
        solution.pick(); // return 6
        solution.pick(); // return 1
        solution.pick(); // return 0
        solution.pick(); // return 4

-----------

```python
class Solution:

    """
    denote b := len(blacklist)
    Time Complexity : __init__() O(b*logb), pick() O(logb)
    Space Complexity : O(b)
    """
    def __init__(self, n: int, blacklist: List[int]):
        # sort the blacklist ascending
        # and calculate how many remaining "white" element we have
        self.blacklist = sorted(blacklist)
        self.B = len(blacklist)
        self.n = n
        self.filtered_n = self.n - self.B
                
    def pick(self) -> int:
        """
        Generate a number between [0, filtered_n - 1] inclusively, call it k
        We want to find the k-th (0-indexed) largest number not in blacklist
        Find the largest blacklist number that's smaller than the random number generated
        its index is how many we want to "skip" in giving the correct index of the k-th white element
        """
        k = random.randint(0, self.filtered_n-1)
        left, right = 0, self.B - 1
        ans = -1
        while left <= right:
            mid = left + (right - left) // 2
            if self.blacklist[mid] - mid > k:
                right = mid - 1
            else:
                ans = mid
                left = mid + 1
        if ans == -1:
            # even the smallest blacknumber is greater than k, directly return k
            return k
        else:
            # ans is 0-index, there are (1+ans) blacklist number in-between 
            return k + ans + 1
```

