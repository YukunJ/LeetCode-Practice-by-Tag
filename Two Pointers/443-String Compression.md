**443. String Compression**

```Tag : two pointers/string```

**Description:**

Given an array of characters ```chars```, compress it using the following algorithm:

Begin with an empty string ```s```. For each group of **consecutive repeating characters** in ```chars```:

+ If the group's length is ```1```, append the character to ```s```.

+ Otherwise, append the character followed by the group's length.

The compressed string ```s``` **should not be returned separately**, but instead, be stored **in the input character array** ```chars```. Note that group lengths that are ```10``` or longer will be split into multiple characters in ```chars```.

After you are done **modifying the input array**, return the new length of the array.

You must write an algorithm that uses only constant extra space.

**Example1:**

		Input: chars = ["a","a","b","b","c","c","c"]
		Output: Return 6, and the first 6 characters of the input array should be: ["a","2","b","2","c","3"]
		Explanation: The groups are "aa", "bb", and "ccc". This compresses to "a2b2c3".

**Example2:**

		Input: chars = ["a"]
		Output: Return 1, and the first character of the input array should be: ["a"]
		Explanation: The only group is "a", which remains uncompressed since it's a single character.

**Example3:**

		Input: chars = ["a","b","b","b","b","b","b","b","b","b","b","b","b"]
		Output: Return 4, and the first 4 characters of the input array should be: ["a","b","1","2"].
		Explanation: The groups are "a" and "bbbbbbbbbbbb". This compresses to "ab12".

**Example4:**

		Input: chars = ["a","a","a","b","b","a","a"]
		Output: Return 6, and the first 6 characters of the input array should be: ["a","3","b","2","a","2"].
		Explanation: The groups are "aaa", "bb", and "aa". This compresses to "a3b2a2". Note that each group is independent even if two groups have the same character.

-----------

```python
class Solution:
    def compress(self, chars: List[str]) -> int:
        """
        We use a sliding pointer to solve this problem in one pass-through
        be careful that the last group needs to be dealt with outside the loop
        and only count > 1 needs to be record the number
        
        denote n := len(chars)
        Time Complexity : O(n)
        Space Complexity : O(1)
        """
        ptr = 0
        n = len(chars)
        curr_char = None
        count = 0
        for idx, char in enumerate(chars):
            if 0 < idx and chars[idx] == chars[idx-1]:
                # a consecutive repeating pattern
                count += 1
            else:
                if idx > 0:
                    # overwrite the chars array
                    chars[ptr] = curr_char
                    ptr += 1
                    if count > 1:
                        for digit_str in str(count):
                            chars[ptr] = digit_str
                            ptr += 1
                # clear cache and read a new character
                count = 1
                curr_char = chars[idx]
            
        # don't forget the last group
        chars[ptr] = curr_char
        ptr += 1
        if count > 1:
            for digit_str in str(count):
                chars[ptr] = digit_str
                ptr += 1
                
        return ptr
```
