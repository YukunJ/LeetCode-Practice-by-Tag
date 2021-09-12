**1733. Minimum Number of People to Teach**

```Tag : greedy/hashtable/array```

**Description:**

On a social network consisting of ```m``` users and some friendships between users, two users can communicate with each other if they know a common language.

You are given an integer ```n```, an array ```languages```, and an array ```friendships``` where:

+ There are ```n``` languages numbered ```1``` through ```n```,

+ ```languages[i]``` is the set of languages the ```i-​​​​​​th```​​​​ user knows, and

+ ```friendships[i] = [u​​​​​​i​​​, v​​​​​​i]``` denotes a friendship between the users ```u​​​​​​​​​​​i​​​​​``` and ```vi```.

You can choose **one** language and teach it to some users so that all friends can communicate with each other. Return the **minimum** number of users you need to teach.

Note that friendships are not transitive, meaning if ```x``` is a friend of ```y``` and ```y``` is a friend of ```z```, this doesn't guarantee that ```x``` is a friend of ```z```.


**Example1:**

        Input: n = 2, languages = [[1],[2],[1,2]], friendships = [[1,2],[1,3],[2,3]]
        Output: 1
        Explanation: You can either teach user 1 the second language or user 2 the first language.
        
**Example2:**

        Input: n = 3, languages = [[2],[1,3],[1,2],[3]], friendships = [[1,4],[1,2],[3,4],[2,3]]
        Output: 2
        Explanation: Teach the third language to users 1 and 3, yielding two users to teach.

-----------

```python
class Solution:
    def minimumTeachings(self, n: int, languages: List[List[int]], friendships: List[List[int]]) -> int:
        """
        There are two points important about this question: Mutualness and Greediness
        The Mutualness is that, if A cannot communicate with B yet they are friend, both of them need to learn a new language
        The Greediness is that, when we decide what language to teach a group of people who cannot communicate with their friends, we want to teach the majority language
    
        denote f := len(friendships), l := sum of the individual language count in languages array
        Time Complexity : 1) takes O(l) 
                          2) takes O(l), there are f intersections to be made, each takes at most O(l/f) 
                          3) takes O(m+l)
                          4) takes O(n)
                          Therefor in total it is O(l+m+n)
        Space Complexity : O(l+m+n)
        """
        from collections import defaultdict
        
        # 1) hash-set for everyone's language bag
        languages = [set(personal_languages) for personal_languages in languages]
        
        # 2) check which pair of friends cannot successfully communicate with each other
        cannot_communicate = set()
        for friendA, friendB in friendships:
            idx_A, idx_B = friendA-1, friendB-1
            if not languages[idx_A].intersection(languages[idx_B]):
                # don't have any shared language in common
                cannot_communicate.add(friendA)
                cannot_communicate.add(friendB)
        
        # 3) check what's the majority language
        language_count = defaultdict(int)
        for people in cannot_communicate:
            for personal_language in languages[people-1]:
                language_count[personal_language] += 1

        # 4) teach people the majority language
        return len(cannot_communicate) - max(language_count.values()) if language_count else 0
```


