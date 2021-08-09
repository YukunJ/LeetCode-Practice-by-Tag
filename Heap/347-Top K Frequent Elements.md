**347. Top K Frequent Elements**

```Tag : Heap/Hash Table```

**Description:**

Given an integer array ```nums``` and an integer ```k```, return the ```k``` most frequent elements. You may return the answer in **any order**.

**Follow up**: Your algorithm's time complexity must be better than ```O(n log n)```, where ```n``` is the array's size.
**Example1**:

        Input: nums = [1,1,1,2,2,3], k = 2
        Output: [1,2]

**Example2**:

        Input: nums = [1], k = 1
        Output: [1]

        
-----------

**Solution1: Heap**

```python
class Freq:
    """Helper Class: for the heap comparison"""
    def __init__(self, num, freq):
        self.num = num
        self.freq = freq
    
    def __lt__(self, another):
        """Overload the '<' operator"""
        if self.freq < another.freq:
            return True
        return False

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        """
        A very direct application of hashmap and heap
        1) traversal the list to count the frequency of each number using a hashtable,
            this step would take O(n)
        2) maintain a minheap of size k to filtering out the k most frequent element
            1] if there are less than k elements in the heap, directly add it
            2] else, check if the frequency is greater than the heap top's frequency
                1} if so, pop out the heap top and push it in
                2} else, there are already at least k elements more frequent, abandon this one
            this step would take O(n*logk)

        denote n := len(nums)
        Time Complexity : O(n*logk)
        Space Complexity : O(n)
        """
        import heapq
        from collections import defaultdict

        # count frequency
        hashtable = defaultdict(int)
        for num in nums:
            hashtable[num] += 1
        
        # maintain a size=k min heap
        elements = [Freq(num, freq) for num, freq in hashtable.items()]
        heap = []
        for e in elements:
            if len(heap) < k:
                heapq.heappush(heap, e)
            else:
                if e.freq > heap[0].freq:
                    heapq.heapreplace(heap, e)
        
        return [e.num for e in heap]
```

-----------

**Solution2: QuickSort**

```python
class Freq:
    """Helper Class: for the heap comparison"""
    def __init__(self, num, freq):
        self.num = num
        self.freq = freq

    def __lt__(self, another):
        """Overload the '<' operator"""
        if self.freq < another.freq:
            return True
        return False

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        """
        denote n := len(nums)
        Time Complexity : O(n^2) in worst case, but on average is O(n) (find the k-th largest algorithm)
        Space Complexity : O(n)
        """
        import random
        from collections import defaultdict

        def quicksort_partition(elements: List['Freq'], l: int, r: int) -> int:
            pivot = elements[r]
            i = l - 1
            for j in range(l, r):
                if elements[j] > pivot:
                    i += 1
                    elements[i], elements[j] = elements[j], elements[i]
            i += 1
            elements[i], elements[r] = elements[r], elements[i]
            return i
        
        def quicksort(elements: List['Freq'], l: int, r: int, k:int):
            if l >= r:
                return
            # random select a pivot to swap 
            pivot = random.randint(l, r)
            elements[pivot], elements[r] = elements[r], elements[pivot]
            index = quicksort_partition(elements, l, r)
            if index == k-1:
                return
            elif index > k-1:
                quicksort(elements, l, index-1, k)
            else:
                quicksort(elements, index+1, r, k)

        # count frequency
        hashtable = defaultdict(int)
        for num in nums:
            hashtable[num] += 1

        elements = [Freq(num, freq) for num, freq in hashtable.items()]

        # call quicksort and retrieve first k elements
        quicksort(elements, 0, len(elements)-1, k)
        
        return [e.num for e in elements[:k]]
```
