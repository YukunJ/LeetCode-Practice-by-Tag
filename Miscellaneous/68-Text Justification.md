**68. Text Justification**

```Tag : string/array```

**Description:**

Given an array of strings ```words``` and a width ```maxWidth```, format the text such that each line has exactly ```maxWidth``` characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces ```' '``` when necessary so that each line has exactly ```maxWidth``` characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line does not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left-justified and no extra space is inserted between words.

**Note**:

+ A word is defined as a character sequence consisting of non-space characters only.

+ Each word's length is guaranteed to be greater than 0 and not exceed maxWidth.

+ The input array ```words``` contains at least one word.

**Example1:**

        Input: words = ["This", "is", "an", "example", "of", "text", "justification."], maxWidth = 16
        Output:
        [
        "This    is    an",
        "example  of text",
        "justification.  "
        ]
        
**Example2:**

        Input: words = ["What","must","be","acknowledgment","shall","be"], maxWidth = 16
        Output:
        [
        "What   must   be",
        "acknowledgment  ",
        "shall be        "
        ]
        Explanation: Note that the last line is "shall be    " instead of "shall     be", because the last line must be left-justified instead of fully-justified.
        Note that the second line is also left-justified becase it contains only one word.
        
 **Example3:** 
 
        Input: words = ["Science","is","what","we","understand","well","enough","to","explain","to","a","computer.","Art","is","everything","else","we","do"], maxWidth = 20
        Output:
        [
        "Science  is  what we",
        "understand      well",
        "enough to explain to",
        "a  computer.  Art is",
        "everything  else  we",
        "do                  "
        ]

-----------

```python
class Solution:
    def fullJustify(self, words: List[str], maxWidth: int) -> List[str]:
        """
        There are not too much special algorithms in this question
        just need to be very careful about edge cases and string manipulations
        
        denote n := len(words)
        Time Complexity  : O(n) we iterate through each word
        Space Complexity : O(maxWidth) we disregard the final output vector; 
                            in the construction process the string buffer is at most of size O(maxWidth)
        """
        res = []
        curr = ""
        for word in words:
            if not curr:
                # a new line, guaranteed to contain a word
                curr += word
            else:
                if 1 + len(word) + len(curr) > maxWidth:
                    # too long a new word, need to break
                    # re-pad the finished line
                    space_remain = maxWidth - len(curr)
                    curr_lst = curr.split()
                    if len(curr_lst) == 1:
                        # a single word takes the whole line, do left-justfy
                        res.append(curr + " " * (maxWidth-len(curr)))
                    else:
                        # there should be 1 + 'common_add' spaces between every two words
                        # and the first 'left_extra' should have 1 extra space
                        common_add, left_extra = divmod(space_remain, len(curr_lst)-1)
                        string_buffer = [curr_lst[0], " "*(1+common_add), " " if left_extra > 0 else ""]
                        for i in range(1, len(curr_lst)):
                            string_buffer.append(curr_lst[i])
                            if i != len(curr_lst)-1:
                                # the last word doesn't need ending space
                                if i < left_extra:
                                    string_buffer.append(" "*(2+common_add))
                                else:
                                    string_buffer.append(" "*(1+common_add))
                        res.append("".join(string_buffer))
                    # clear the cache and move to next word
                    curr = word
                else:
                    # add a space and absorb a new word
                    curr += " " + word
        
        # have a last line 'curr' at hand
        res.append(curr + " " * (maxWidth-len(curr)))
        
        return res
```
