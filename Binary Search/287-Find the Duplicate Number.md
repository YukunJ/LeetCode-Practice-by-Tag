**287. Find the Duplicate Number**

```Tag : Binary Search/Bitwise Operation/Two Pointers/Linked List```

**Description:**

Given an array of integers ```nums``` containing ```n + 1``` integers where each integer is in the range ```[1, n]``` inclusive.

There is only **one repeated number** in ```nums```, return *this repeated number*.

You must solve the problem ```without``` modifying the array ```nums``` and uses only constant extra space.

**Follow up:**

+ How can we prove that at least one duplicate number must exist in nums?

+ Can you solve the problem in linear runtime complexity?

**Example1**:

        Input: nums = [1,3,4,2,2]
        Output: 2
        
**Example2**:      

        Input: nums = [3,1,3,4,2]
        Output: 3
        
**Example3**:   

        Input: nums = [1,1]
        Output: 1

**Example4**:   

        Input: nums = [1,1,2]
        Output: 1
        
-----------

**Solution1: Binary Search**

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        """
        A quick thought would be to sort the array and use binary search
        all the unique number to the left should be nums[i] = i+1 (index starting from 0)
        and search first the position such that nums[i] < i+1
        However, since we could not modify the array however, we need to use an alternative
        the min number in the array is 1, the max number is n,
        denote count[i] := number of elements in nums that are less than or equal i
        it should usually be count[i] = i
        the left most i position such that count[i] > i is the number we are looking for
        
        denote n := len(nums)
        Time Complexity : O(n*logn)
        Space Complexity : O(1)
        """
        def count(i: int) -> int:
            """Helper function: count how many elements <= i in nums, O(n) time"""
            cnt = 0
            for num in nums:
                if num <= i:
                    cnt += 1
            return cnt
        
        left, right = 1, len(nums)-1
        ans = -1
        while left <= right:
            mid = left + (right-left) // 2
            cnt_mid = count(mid)
            if cnt_mid <= mid:
                left = mid + 1
            else:
                ans = mid
                right = mid - 1
        return ans
```

-----------

**Solution2: Bitwise Operation**

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        """
        Another idea is to count bits
        we can reproduce the duplicate number bit by bit
        look at i-th bit, we should count [1, n] number's i-th bit sum "y"
        and then count number in nums's i-th bit sum "x"
        if "x" > "y", then the duplicate number clearly have this bit
        
        denote n := len(nums)
        Time Complexity : O(n*logn), there are logn bits to be searched
        Space Complexity : O(1)
        """
        n = len(nums)-1
        maxbit = 32
        while ((n) >> maxbit) == 0:
            # 32 bit might be too much, shrink it
            maxbit -= 1
        ans = 0
        for bit in range(maxbit+1):
            # check bit by bit
            y = 0
            for i in range(n+1):
                if (i >> bit) & 1:
                    y += 1
            x = 0
            for num in nums:
                if (num >> bit) & 1:
                    x += 1
            
            if x > y:
                # the duplicate number do have this bit set up
                ans |= (1 << bit)
        
        return ans
```

-----------

**Solution3: Two Pointers**

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        """
        we can construct an graph edge index i -> nums[i]
        because some nums[i] are duplicate, there are 2 index that mapped to nums[i]
        so there is a loop in our graph, or linked list if you prefer
        We init two pointers, slow and fast.
        denote L := length of loop, a := distance before the loop
        slow and fast pointer with meet, denote b := the distance at meeting point in the loop
        so define the rest distance c := L - a
        in a linear idea, we can get a + b +c = L
        when the two pointers meet,
        slow point goes a + b
        fast point goes a + b + x * L = twice slow pointer = 2(a+b)
        we solve for a = x * L -b = (x-1) * L + L-b = (x-1)* L + c
        so when they meet, we just init a new pointer to start from the beginning
        and when it meet with the slow pointer, that's the duplicate we are looking for
        
        denote n := len(nums)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        slow, fast = nums[0], nums[nums[0]]
        while slow != fast:
            # find the meeting place
            slow = nums[slow]
            fast = nums[nums[fast]]
        # at this time, slow is "c" distance away from the loop point
        new = 0
        while new != slow:
            new = nums[new]
            slow = nums[slow]
        return new
```
