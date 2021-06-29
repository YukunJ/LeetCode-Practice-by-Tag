**567. Permutation in String**

```Tag: sliding window/hashtable```

**Description:**

Given two strings ```s1``` and ```s2```, return ```true``` if ```s2``` contains the permutation of ```s1```.

In other words, one of ```s1```'s permutations is the substring of ```s2```.



**Example1**:

		Input: s1 = "ab", s2 = "eidbaooo"
		Output: true
		Explanation: s2 contains one permutation of s1 ("ba").

**Example2**:

		Input: s1 = "ab", s2 = "eidboaoo"
		Output: false

**Hints**:

+ Obviously, brute force will result in TLE. Think of something else.

+ How will you check whether one string is a permutation of another string?

+ One way is to sort the string and then compare. But, Is there a better way?

+ If one string is a permutation of another string then they must one common metric. What is that?

+ Both strings must have same character frequencies, if one is permutation of another. Which data structure should be used to store frequencies?

+ What about hash table? An array of size 26?

-----------

**Solution1: Sliding window(not optimized)**

```python
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        """
        From the hints, we will do this problem in a fixed-size sliding window way
        we main two pointers and two hashtable with fixed size of 26(lower case characters)
        a permutation is just frequency being equal
        we use a sliding window of size len(s1)

        denote m := len(s1), n := len(s2)
        Time Complexity : O(n)
        Space Complexity : O(1) ASCII characters being finite
        """
        if len(s1) > len(s2): # boundary case checking
            return False
        count_s1, count_s2, offset = [0] * 26, [0] * 26, ord('a')
        # construct the first window
        for i in range(len(s1)):
            count_s1[ord(s1[i])-offset] += 1
            count_s2[ord(s2[i])-offset] += 1 
        # slide the window
        left = 0
        for right in range(len(s1), len(s2)):
            if count_s1 == count_s2:
                return True
            # abandon the left most element in window
            count_s2[ord(s2[left])-offset] -= 1
            # move left pointer
            left += 1
            # expand the window by moving right pointer by 1 position
            count_s2[ord(s2[right])-offset] += 1
        
        # the final window hasn't been checked
        if count_s1 == count_s2:
            return True
        # Failure
        return False
```

-----------

**Solution2: Sliding window(optimized)**

```python
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        """
        Based on solution 1,
        every time we do no necessarily need to check entire "count_s1 == count_s1"
        since every sliding brings only 1 char out, 1 char in

        denote m := len(s1), n := len(s2)
        Time Complexity : O(n)
        Space Complexity : O(1) ASCII characters being finite
        """
        if len(s1) > len(s2): # boundary case checking
            return False
        count_s1, count_s2, offset = [0] * 26, [0] * 26, ord('a')
        # construct the first window
        for i in range(len(s1)):
            count_s1[ord(s1[i])-offset] += 1
            count_s2[ord(s2[i])-offset] += 1 
        diff = [count_s2[i]-count_s1[i] for i in range(26)]
        diff_count = 0
        # check the first window
        for i in range(26):
            if diff[i] != 0:
                diff_count += 1
        if diff_count == 0:
            return True

        left = 0
        for right in range(len(s1), len(s2)):
            # pop out left most elements of the window
            pop = ord(s2[left]) - offset
            if diff[pop] == 0: # a bad pop
                diff_count += 1
            diff[pop] -= 1
            if diff[pop] == 0: # a good pop
                diff_count -= 1
            # move the left pointer
            left += 1
            # add the new right elements of the window
            add = ord(s2[right]) - offset
            if diff[add] == 0: # a bad add
                diff_count += 1
            diff[add] += 1
            if diff[add] == 0: # a good add
                diff_count -= 1
            # check if a success permutation found      
            if diff_count == 0:
                return True
        # Failure
        return False
```