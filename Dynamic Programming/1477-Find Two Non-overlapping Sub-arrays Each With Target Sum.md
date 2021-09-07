**1477. Find Two Non-overlapping Sub-arrays Each With Target Sum**

```Tag : dynamic programming/presum/hashtable/array```

**Description:**

Given an array of integers ```arr``` and an integer ```target```.

You have to find **two non-overlapping sub-arrays** of ```arr``` each with a sum equal ```target```. There can be multiple answers so you have to find an answer where the sum of the lengths of the two sub-arrays is **minimum**.

Return the minimum sum of the lengths of the two required sub-arrays, or return ```-1``` if you cannot find such two sub-arrays.

**Example1:**

        Input: arr = [3,2,2,4,3], target = 3
        Output: 2
        Explanation: Only two sub-arrays have sum = 3 ([3] and [3]). The sum of their lengths is 2.

**Example2:**

        Input: arr = [7,3,4,7], target = 7
        Output: 2
        Explanation: Although we have three non-overlapping sub-arrays of sum = 7 ([7], [3,4] and [7]), but we will choose the first and third sub-arrays as the sum of their lengths is 2.

**Example3:**

        Input: arr = [4,3,2,6,2,3,4], target = 6
        Output: -1
        Explanation: We have only one sub-array of sum = 6.

**Example4:**

        Input: arr = [5,5,4,4,5], target = 3
        Output: -1
        Explanation: We cannot find a sub-array of sum = 3.

**Example5:**

        Input: arr = [3,1,1,1,5,1,2,1], target = 3
        Output: 3
        Explanation: Note that sub-arrays [1,2] and [2,1] cannot be an answer because they overlap.

**Hints**:

+ Let's create two arrays prefix and suffix where ```prefix[i]``` is the minimum length of sub-array ends before ```i``` and has ```sum = k```, ```suffix[i]``` is the minimum length of sub-array starting at or after i and has ```sum = k```.

+ The answer we are searching for is ```min(prefix[i] + suffix[i])``` for all values of i from ```0``` to ```n-1``` where ```n == arr.length```.

+ If you are still stuck with how to build prefix and suffix, you can store for each index i the length of the sub-array starts at ```i``` and has sum = k or infinity otherwise, and you can use it to build both prefix and suffix.

-----------

```python
class Solution:
    def minSumOfLengths(self, arr: List[int], target: int) -> int:
        """
        Follow the hints, since the two arrays must be non-overlapping
        we will cut the array into halves, 
        and find one "sum target" on one side, and the other "sum target" the other side
        denote prefix[i] := the minimum length of subarray in arr[:i] such that sum up to target
               postfix[i] := the minimum length of subarray in arr[i:] such that sum up to target
        For the dynamic programming, the state transition would be:
        prefix[i] = min(prefix[i-1], length possible subarry ending with arr[i-1])
        postfix[i] = min(postfix[i+1], length possible subarray starting with arr[i])
        
        denote n := len(arr)
        Time Complexity : O(n) by using presum vectors
        Space Complexity : O(n)
        """
        n = len(arr)
        prefix = [float('inf')] * (n+1)
        postfix = [float('inf')] * (n+1)
        
        # construct prefix dp array
        presum = {0 : -1}
        curr = 0
        for i in range(1, n+1):
            curr_length = float('inf')
            curr += arr[i-1]
            curr_target = curr - target
            if curr_target in presum:
                curr_length = i-1 - presum[curr_target]
            presum[curr] = i-1
            prefix[i] = min(prefix[i-1], curr_length)
        
        # construct postfix dp array
        postsum = {0 : n}
        curr = 0
        for i in range(n-1, -1, -1):
            curr_length = float('inf')
            curr += arr[i]
            curr_target = curr - target
            if curr_target in postsum:
                curr_length = postsum[curr_target] - i
            postsum[curr] = i  
            postfix[i] = min(postfix[i+1], curr_length)

        # try cut the array in different positions to find minimal length solution
        res = float('inf')
        for i in range(1, n):
            res = min(res, prefix[i]+postfix[i])
            
        return res if res != float('inf') else -1
```

