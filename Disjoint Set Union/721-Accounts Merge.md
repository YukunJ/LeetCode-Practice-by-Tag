**721. Accounts Merge**

```Tag : Disjoint Set Union/Hashtable```

**Description:**

Given a list of ```accounts``` where each element ```accounts[i]``` is a list of strings, where the first element ```accounts[i][0]``` is a name, and the rest of the elements are **emails** representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some common email to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails **in sorted order**. The accounts themselves can be returned in **any order**.

**Example1:**

		Input: accounts = [["John","johnsmith@mail.com","john_newyork@mail.com"],["John","johnsmith@mail.com","john00@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
		Output: [["John","john00@mail.com","john_newyork@mail.com","johnsmith@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
		Explanation:
		The first and third John's are the same person as they have the common email "johnsmith@mail.com".
		The second John and Mary are different people as none of their email addresses are used by other accounts.
		We could return these lists in any order, for example the answer [['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'], ['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']] would still be accepted.

**Example2:**

		Input: accounts = [["Gabe","Gabe0@m.co","Gabe3@m.co","Gabe1@m.co"],["Kevin","Kevin3@m.co","Kevin5@m.co","Kevin0@m.co"],["Ethan","Ethan5@m.co","Ethan4@m.co","Ethan0@m.co"],["Hanzo","Hanzo3@m.co","Hanzo1@m.co","Hanzo0@m.co"],["Fern","Fern5@m.co","Fern1@m.co","Fern0@m.co"]]
		Output: [["Ethan","Ethan0@m.co","Ethan4@m.co","Ethan5@m.co"],["Gabe","Gabe0@m.co","Gabe1@m.co","Gabe3@m.co"],["Hanzo","Hanzo0@m.co","Hanzo1@m.co","Hanzo3@m.co"],["Kevin","Kevin0@m.co","Kevin3@m.co","Kevin5@m.co"],["Fern","Fern0@m.co","Fern1@m.co","Fern5@m.co"]]

-----------

```python
class DSU:
    """Helper class: Disjoint Set Union"""
    def __init__(self, n: int) -> None:
        self.parent = list(range(n))
    
    def union(self, idx1: int, idx2: int) -> None:
        self.parent[self.find(idx2)] = self.find(idx1)
    
    def find(self, idx: int) -> int:
        if self.parent[idx] != idx:
            self.parent[idx] = self.find(self.parent[idx])
        return self.parent[idx]

class Solution:
    def accountsMerge(self, accounts: List[List[str]]) -> List[List[str]]:
        """
        denote n := number of unique emails in accounts >= len(accounts)
        Time Complexity : O(n*logn) the final sorting takes O(n*logn) time
                            for the DSU operation, each operation is a(n), where we can think of a(n) as a constant time
                            for two unique email, we need to do 2 "find parents" and 1 "union" operations at most.
                            Therefore, there are at most 2n "find parent" and n "union" operations
                            And we traversal all emails and accounts a few times
        Space Complexity : O(n) for DSU storage, hashmap and recursion stack height in 'find' functionality
        """
        from collections import defaultdict
        emailToIndex = dict()
        emailToName = dict()
        
        for account in accounts:
            # enumerate all the emails and name
            # assign a unique index to each email
            # and its first appearing name
            name = account[0]
            for email in account[1:]:
                if email not in emailToIndex:
                    emailToIndex[email] = len(emailToIndex)
                    emailToName[email] = name
        
        dsu = DSU(len(emailToIndex))
        for account in accounts:
            # for all emails attached to the same name
            # merge them together to a same parent
            firstIndex = emailToIndex[account[1]]
            for other_email in account[2:]:
                dsu.union(firstIndex, emailToIndex[other_email])
                
        
        indexToEmail = defaultdict(list)
        for email, index in emailToIndex.items():
            # assign each email to its parent index cluster
            parent_index = dsu.find(index)
            indexToEmail[parent_index].append(email)
        
        ans = []
        for emails in indexToEmail.values():
            # return in sorted order
            ans.append([emailToName[emails[0]]] + sorted(emails))
            
        return ans
```