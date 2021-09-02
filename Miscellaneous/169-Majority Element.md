**169. Majority Element**

```Tag : Boyer-Moore Voting/hashtable/counting/sorting/divide and conquer```

**Description:**

Given an array ```nums``` of size ```n```, return the *majority element*.

The majority element is the element that appears more than ```⌊n / 2⌋``` times. You may assume that the majority element always exists in the array.

**Example1:**

		Input: nums = [3,2,3]
		Output: 3

**Example2:**

		Input: nums = [2,2,1,1,1,2,2]
		Output: 2

-----------

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        """
        We learn a new algorithm called "Boyer-Moore Voting Algorithm" in this question
        Essentially we keep a candiate and count
        1) when we see a same number as candiate, count += 1
        2) when we see any other number, count -= 1
        when the count reach 0, we discard the current candidate and continue select next one as candiate
        the trick here is that we can safely discard all the visited candidate before 
            without affecting the accuracy of this algorithm
        Two possibility of discard:
        1) the candidate is not majority, nothing happens
        2) the candiate is indeed majority, then we discard equal number of the majority and other, so the majority is still the majority
        
        denote n := len(nums)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        candidate = None
        count = 0
        for num in nums:
            if count == 0:
                candidate = num
            count += (1 if candidate == num else -1)
        return candidate
```

