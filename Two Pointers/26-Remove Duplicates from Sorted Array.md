**26. Remove Duplicates from Sorted Array**

```Tag : two pointers/sorting```

**Description:**

Given an integer array ```nums``` sorted in **non-decreasing order**, remove the duplicates in-place such that each unique element appears only **once**. The **relative order** of the elements should be kept the **same**.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the **first part** of the array ```nums```. More formally, if there are ```k``` elements after removing the duplicates, then the first ```k``` elements of ```nums``` should hold the final result. It does not matter what you leave beyond the first ```k``` elements.

Return ```k``` after placing the final result in the first ```k``` slots of ```nums```.

Do **not** allocate extra space for another array. You must do this by **modifying the input array** in-place with ```O(1)``` extra memory.

**Custom Judge:**

The judge will test your solution with the following code:

```
int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```

If all assertions pass, then your solution will be **accepted**.


**Example1**:


		Input: nums = [1,1,2]
		Output: 2, nums = [1,2,_]
		Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
		It does not matter what you leave beyond the returned k (hence they are underscores).

 
**Example2**:
 
		Input: nums = [0,0,1,1,1,2,2,3,3,4]
		Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
		Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
		It does not matter what you leave beyond the returned k (hence they are underscores).

**Hints:**:

+ In this problem, the key point to focus on is the input array being sorted. As far as duplicate elements are concerned, what is their positioning in the array when the given array is sorted? Look at the image above for the answer. If we know the position of one of the elements, do we also know the positioning of all the duplicate elements?

+ We need to modify the array in-place and the size of the final array would potentially be smaller than the size of the input array. So, we ought to use a two-pointer approach here. One, that would keep track of the current element in the original array and another one for just the unique elements.

+ Essentially, once an element is encountered, you simply need to **bypass** its duplicates and move on to the next unique element.
   
-----------


```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        """
        This is an easy two-pointers question
        we need to skip duplicate element and place unique elements in front part of the array
        
        denote n := len(nums)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        ptr = 0 # the position for next unique element
        i = 0 # the sliding pointer
        while i < len(nums):
            if i > 0 and nums[i] == nums[i-1]:
                # duplicat encountered, skip this one
                i += 1
                continue
            nums[ptr] = nums[i]
            i += 1
            ptr += 1
        return ptr # notice here we are 0-indexed 
```
