**158. Read N Characters Given Read4 II - Call multiple times**

```Tag : design/interactive/string```

**Description:**

Given a ```file``` and assume that you can only read the file using a given method ```read4```, implement a method ```read``` to read ```n``` characters. Your method ```read``` may be **called multiple times**.

**Method read4**:

The API ```read4``` reads **four consecutive characters** from ```file```, then writes those characters into the buffer array ```buf4```.

The return value is the number of actual characters read.

Note that ```read4()``` has its own file pointer, much like ```FILE *fp``` in C.

**Definition of read4**:

    Parameter:  char[] buf4
    Returns:    int

	buf4[] is a destination, not a source. The results from read4 will be copied to buf4[].

Below is a high-level example of how ```read4``` works:

![avatar](Fig/158-instruction.png)

		File file("abcde"); // File is "abcde", 		initially file pointer (fp) points to 'a'
		char[] buf4 = new char[4]; // Create buffer with enough space to store characters
		read4(buf4); // read4 returns 4. Now buf4 = "abcd", fp points to 'e'
		read4(buf4); // read4 returns 1. Now buf4 = "e", fp points to end of file
		read4(buf4); // read4 returns 0. Now buf4 = "", fp points to end of file

**Method read**:

By using the ```read4``` method, implement the method read that reads ```n``` characters from ```file``` and store it in the buffer array ```buf```. Consider that you cannot manipulate ```file``` directly.

The return value is the number of actual characters read.

**Definition of read**:

    Parameters:	char[] buf, int n
    Returns:	int

	buf[] is a destination, not a source. You will need to write the results to buf[].


**Note**:

+ Consider that you cannot manipulate the file directly. The file is only accessible for read4 but not for read.

+ The read function may be called multiple times.
Please remember to RESET your class variables declared in Solution, as static/class variables are persisted across multiple test cases. Please see here for more details.

+ You may assume the destination buffer array, buf, is guaranteed to have enough space for storing n characters.

+ It is guaranteed that in a given test case the same buffer buf is called by read.

**Example1:**

		Input: file = "abc", queries = [1,2,1]
		Output: [1,2,0]
		Explanation: The test case represents the following scenario:
		File file("abc");
		Solution sol;
		sol.read(buf, 1); // After calling your read method, buf should contain "a". We read a total of 1 character from the file, so return 1.
		sol.read(buf, 2); // Now buf should contain "bc". We read a total of 2 characters from the file, so return 2.
		sol.read(buf, 1); // We have reached the end of file, no more characters can be read. So return 0.
		Assume buf is allocated and guaranteed to have enough space for storing all characters from the file.

**Example2:**

		Input: file = "abc", queries = [4,1]
		Output: [3,0]
		Explanation: The test case represents the 		following scenario:
		File file("abc");
		Solution sol;
		sol.read(buf, 4); // After calling your read method, buf should contain "abc". We read a total of 3 characters from the file, so return 3.
		sol.read(buf, 1); // We have reached the end of file, no more characters can be read. So return 0.

-----------

```python
# The read4 API is already defined for you.
# def read4(buf4: List[str]) -> int:

class Solution:
    
    def __init__(self):
        # fixed size of 4 array
        self.buf = ['', '', '', '']
        self.isEof = False
        self.buf_ptr = 0 # the remaining pointer
    
    def read(self, buf: List[str], n: int) -> int:
        """
        The idea is that, since the 'n' is quite flexible in this case
        we will try to use 'read 4' to approximate the 'read n', storing in another buffer
        The tricky part in this question is that, the function 'read' may be called multiple times
        so we need to take care of the remaining read from last call
        
        Time Complexity: O(n) read and copy n chars
        Space Complexity: O(1) at most store 4 chars in self.buf
        """
        char_read = 0
        if self.buf_ptr > 0:
            # deal with last call's remaining
            while self.buf_ptr < 4 and char_read < n:
                buf[char_read] = self.buf[self.buf_ptr]
                self.buf[self.buf_ptr] = ''
                self.buf_ptr += 1
                char_read += 1
            if self.buf_ptr == 4:
                # finish all the remainings
                self.buf_ptr = 0
            if char_read == n:
                # done already, no further need to read
                return char_read
        while not self.isEof and char_read < n:
            single_read = read4(self.buf)
            self.buf_ptr = 0
            if single_read < 4:
                # no more chars to be read
                self.isEof = True
            if single_read + char_read > n:
                # some overflow, take partial of it
                single_read = n - char_read
            for i in range(single_read):
                # copy character one by one and reset
                buf[char_read] = self.buf[i]
                self.buf[i] = ''
                char_read += 1
                self.buf_ptr += 1
        return char_read
```