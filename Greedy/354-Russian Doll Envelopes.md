**354. Russian Doll Envelopes**

```Tag : greedy/dynamic programming/sorting```

**Description:**

You are given a 2D array of integers ```envelopes``` where ```envelopes[i] = [wi, hi]``` represents the width and the height of an envelope.

One envelope can fit into another if and only if both the width and height of one envelope are greater than the other envelope's width and height.

Return the *maximum number of envelopes you can Russian doll (i.e., put one inside the other)*.

**Note**: You cannot rotate an envelope.

**Example1:**

        Input: envelopes = [[5,4],[6,4],[6,7],[2,3]]
        Output: 3
        Explanation: The maximum number of envelopes you can Russian doll is 3 ([2,3] => [5,4] => [6,7]).

**Example2:**

        Input: envelopes = [[1,1],[1,1],[1,1]]
        Output: 1


-----------

```python
class Solution:
    def maxEnvelopes(self, envelopes: List[List[int]]) -> int:
        """
        This question is a 2D version of Q300 <Longest Increasing Subsequence>
        Recall that in Q300 we use a greedy approach to get a O(nlogn) solution
        using d[i] := the smallest element of increasin subsequence of length i
        we can reduce this question into the LIS problem, one thing to notice is that
        if the width is the same, the largest height will get into first, then the later smaller height
        will only 'update' but not 'append' to the same longest increasing subsequence
        And notice a key difference between this question and traditional LIS is that
        in LIS question, we are not allowed to change the ordering of the array given
        However, here we can
        so we want to first properly place the array in an ordering such that LIS could generate optimal solution for sure
        
        denote n := len(envelops)
        Time Complexity : O(nlogn)
        Space Complexity : O(n)
        """
        # sort based on first dimension by ascending and if tie descending by second dimension
        envelopes.sort(key=lambda x : (x[0], -x[1]))
        second_coordinate = [e[1] for e in envelopes]
        
        # run a LIS on the second coordinate
        d = []
        for i, e in enumerate(second_coordinate):
            if not d or e > d[-1]:
                d.append(e)
            else:
                # binary search to find a suitable replacement index
                # find the biggest ending that's smaller than current element
                # and replace current element at next position
                left, right = 0, len(d)-1
                ans = -1
                while left <= right:
                    mid = left + (right - left) // 2
                    if d[mid] >= e:
                        right = mid - 1
                    else: 
                        ans = mid
                        left = mid + 1
                d[ans+1] = e
        return len(d)
```

