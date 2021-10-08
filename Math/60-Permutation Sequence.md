**60. Permutation Sequence**

```Tag : math/recursion```

**Description:**

The set ```[1, 2, 3, ..., n]``` contains a total of ```n!``` unique permutations.

By listing and labeling all of the permutations in order, we get the following sequence for ```n = 3```:

- "123"
- "132"
- "213"
- "231"
- "312"
- "321"

Given ```n``` and ```k```, return the ```k-th``` permutation sequence.

**Example1:**

        Input: n = 3, k = 3
        Output: "213"

**Example2:**

        Input: n = 4, k = 9
        Output: "2314"

**Example3:**

        Input: n = 3, k = 1
        Output: "123"

-----------

```python
class Solution:
    def getPermutation(self, n: int, k: int) -> str:
        """
        For a fixed n, there are n! permutations.
        When we first fix the first element, then it reduces to (n-1)! permutations
        we could label each permutation by its ordered index starting from 0 to n!-1
        we could use factorial-based number system to represent it, just like binary encoding
        for example 7 = 1 x 3! + 0 x 2! + 1 x 1! + 0 x 0!
        
        Time Complexity : O(n^2) one delete may take at worst case O(n)
        Space Complexity : O(n)
        """
        
        factorials, nums = [1], ['1'] # 0! =1
        for i in range(1, n):
            # generate 1!, 2!, ..., (n-1)!
            factorials.append(factorials[i-1] * i)
            nums.append(str(i+1))
        k -= 1
        
        # compute the factorial representation of k
        res = []
        for i in range(n-1, -1, -1):
            idx = k // factorials[i]
            k -= idx * factorials[i]
            res.append(nums[idx])
            # after fixing one position
            # we need to disgard that one, and continue with the resut of numbers we have at hand
            del nums[idx]
            
        return ''.join(res)
```

