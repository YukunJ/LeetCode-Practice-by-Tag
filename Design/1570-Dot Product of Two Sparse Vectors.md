**1570. Dot Product of Two Sparse Vectors**

```Tag : Design/Hashtable```

**Description:**

Given two sparse vectors, compute their dot product.

Implement class ```SparseVector```:

+ ```SparseVector(nums)``` Initializes the object with the vector ```nums```

+ ```dotProduct(vec)``` Compute the dot product between the instance of SparseVector and ```vec```

A **sparse vector** is a vector that has mostly zero values, you should store the sparse vector **efficiently** and compute the dot product between two SparseVector.

**Follow up**: What if only one of the vectors is sparse?

**Example1:**

		Input: nums1 = [1,0,0,2,3], nums2 = [0,3,0,4,0]
		Output: 8
		Explanation: v1 = SparseVector(nums1) , v2 = SparseVector(nums2)
		v1.dotProduct(v2) = 1*0 + 0*3 + 0*0 + 2*4 + 3*0 = 8

**Example2:**

		Input: nums1 = [0,1,0,0,0], nums2 = [0,0,0,0,2]
		Output: 0
		Explanation: v1 = SparseVector(nums1) , v2 = SparseVector(nums2)
		v1.dotProduct(v2) = 0*0 + 1*0 + 0*0 + 0*0 + 0*2 = 0

**Example3:**

		Input: nums1 = [0,1,0,0,2,0,0], nums2 = [1,0,0,0,3,0,4]
		Output: 6

**Hints:**

+ Because the vector is sparse, use a data structure that stores the index and value where the element is nonzero.

-----------

```python
"""
The core idea in this question is that,
since we are dealing with sparse vector, many indices are zero-valued,
we don't want to store them; instead we only store the non-zero entry
we use hashtable to do key <--> value storage for non-zero entry

denote n := len(nums)
Time Complexity : __init__() creating hashmap takes O(n), dotProduct() takes O(v) where v := num of non-zero entry 
Space Complexity : __init__() takes O(v) where v := num of non-zero entry, dotProduct() takes O(1)
"""
class SparseVector:
    def __init__(self, nums: List[int]):
        self.nums = {idx : value for idx, value in enumerate(nums) if value != 0}
        

    # Return the dotProduct of two sparse vectors
    def dotProduct(self, vec: 'SparseVector') -> int:
        res = 0
        shorter, longer = self.nums, vec.nums
        # only want to traversal the smaller hashmap key set
        if len(self.nums) > len(vec.nums):
            shorter, longer = vec.nums, self.nums
        for key in shorter:
            if key in longer:
                res += shorter[key] * longer[key]
        return res
        

# Your SparseVector object will be instantiated and called as such:
# v1 = SparseVector(nums1)
# v2 = SparseVector(nums2)
# ans = v1.dotProduct(v2)
```