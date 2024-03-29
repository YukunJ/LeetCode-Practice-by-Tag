**71. Simplify Path**

```Tag : stack/string```

**Description:**

Given a string ```path```, which is an **absolute path** (starting with a slash ```'/'```) to a file or directory in a Unix-style file system, convert it to the simplified **canonical path**.

In a Unix-style file system, a period ```'.'``` refers to the current directory, a double period ```'..'``` refers to the directory up a level, and any multiple consecutive slashes (i.e. ```'//'```) are treated as a single slash ```'/'```. For this problem, any other format of periods such as ```'...'``` are treated as file/directory names.

The **canonical path** should have the following format:

+ The path starts with a single slash ```'/'```.

+ Any two directories are separated by a single slash ```'/'```.

+ The path does not end with a trailing ```'/'```.

+ The path only contains the directories on the path from the root directory to the target file or directory (i.e., no period ```'.'``` or double period ```'..'```)

Return the simplified **canonical path**.

**Example1:**

		Input: path = "/home/"
		Output: "/home"
		Explanation: Note that there is no trailing slash after the last directory name.

**Example2:**

		Input: path = "/../"
		Output: "/"
		Explanation: Going one level up from the root directory is a no-op, as the root level is the highest level you can go.

**Example3:**

		Input: path = "/home//foo/"
		Output: "/home/foo"
		Explanation: In the canonical path, multiple consecutive slashes are replaced by a single one.

**Example4:**

		Input: path = "/a/./b/../../c/"
		Output: "/c"


-----------

```python
class Solution:
    def simplifyPath(self, path: str) -> str:
        """
        This is an easy stack problem if one is familar with Linux system
        we first retrieve the real 'filepath' element from the string
        we traversal through the list and push pathname in a stack
        then skip on '.' and pop stack if '..'
        but be careful about poping from an empty stack, we assume there is always a highest root directory
        
        denote n := len(path)
        Time Complexity : O(n)
        Space Complexity : O(n)
        """
        path_lst = path.strip('/').split('/')
        stack = []
        for p in path_lst:
            if p:
                # filter out empty string by '//'
                if p == '.':
                    continue
                elif p == '..':
                    # careful empty stack possible here
                    if stack:
                        stack.pop()
                else:
                    stack.append(p)
        return '/' + '/'.join(stack)
```