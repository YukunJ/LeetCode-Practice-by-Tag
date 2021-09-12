**165. Compare Version Numbers**

```Tag : two pointer/string```

**Description:**

Given two version numbers, ```version1``` and ```version2```, compare them.

Version numbers consist of **one or more revisions** joined by a dot ```'.'```. Each revision consists of **digits** and may contain leading **zeros**. Every revision contains **at least one character**. Revisions are **0-indexed from left to right**, with the leftmost revision being revision 0, the next revision being revision 1, and so on. For example ```2.5.33``` and ```0.1``` are valid version numbers.

To compare version numbers, compare their revisions in **left-to-right order**. Revisions are compared using their **integer value ignoring any leading zeros**. This means that revisions ```1``` and ```001``` are considered equal. If a version number does not specify a revision at an index, then treat the revision as ```0```. For example, version ```1.0``` is less than version ```1.1``` because their revision 0s are the same, but their revision 1s are ```0``` and ```1``` respectively, and ```0 < 1```.

Return the following:

+ If ```version1``` < ```version2```, return ```-1```.
+ If ```version1``` > ```version2```, return ```1```.
+ Otherwise, return ```0```.


**Example1:**

        Input: version1 = "1.01", version2 = "1.001"
        Output: 0
        Explanation: Ignoring leading zeroes, both "01" and "001" represent the same integer "1".
        
**Example2:**

        Input: version1 = "1.0", version2 = "1.0.0"
        Output: 0
        Explanation: version1 does not specify revision 2, which means it is treated as "0".
        
**Example3:**

        Input: version1 = "0.1", version2 = "1.1"
        Output: -1
        Explanation: version1's revision 0 is "0", while version2's revision 0 is "1". 0 < 1, so version1 < version2.

**Example4:**

        Input: version1 = "1.0.1", version2 = "1"
        Output: 1

**Example5:**

        Input: version1 = "7.5.2.4", version2 = "7.5.3"
        Output: -1

-----------

```python
class Solution:
    def compareVersion(self, version1: str, version2: str) -> int:
        """
        We adopt a two-pointer approach to slide through each sub-version of the two whole version number
        we need to first squeeze out all the leading zeros and then treat 'non-exisiting' as zero
        
        denote n := len(version1), m := len(version2)
        Time Complexity : O(n+m) one pass-through two version
        Space Complexity : O(max(n, m)) substring storage
        """
        def get_next_chunk(version, ptr) -> List[int]:
            """Helper function: retrieve the next version of the string version"""
            if ptr >= len(version): # boundary, at the end already
                return [0, ptr]
            else:
                start = ptr
                while ptr+1 < len(version) and version[ptr+1] != '.':
                    ptr += 1
                return [int(version[start:ptr+1]), ptr+2]
            
        n, m = len(version1), len(version2)
        ptr1, ptr2 = 0, 0 
        
        while ptr1 < n or ptr2 < m: # two pointer sliding
            sub1, ptr1 = get_next_chunk(version1, ptr1)
            sub2, ptr2 = get_next_chunk(version2, ptr2)
            if sub1 < sub2:
                return -1
            elif sub1 > sub2:
                return 1
        # reach here means all equal
        return 0
```


