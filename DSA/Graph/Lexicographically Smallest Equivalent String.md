You are given two strings of the same length `s1` and `s2` and a string `baseStr`.

We say `s1[i]` and `s2[i]` are equivalent characters.

- For example, if `s1 = "abc"` and `s2 = "cde"`, then we have `'a' == 'c'`, `'b' == 'd'`, and `'c' == 'e'`.

Equivalent characters follow the usual rules of any equivalence relation:

- **Reflexivity:** `'a' == 'a'`.
- **Symmetry:** `'a' == 'b'` implies `'b' == 'a'`.
- **Transitivity:** `'a' == 'b'` and `'b' == 'c'` implies `'a' == 'c'`.

For example, given the equivalency information from `s1 = "abc"` and `s2 = "cde"`, `"acd"` and `"aab"` are equivalent strings of `baseStr = "eed"`, and `"aab"` is the lexicographically smallest equivalent string of `baseStr`.

Return _the lexicographically smallest equivalent string of_ `baseStr` _by using the equivalency information from_ `s1` _and_ `s2`.

```python
class DisjointSet:
    def __init__(self, n):
        self.size = [1] * n
        self.parent = list(range(n))

    def findUpar(self, node):
        if node == self.parent[node]:
            return node
        self.parent[node] = self.findUpar(self.parent[node])
        return self.parent[node]

    def unionBySize(self, u, v):
        upU = self.findUpar(u)
        upV = self.findUpar(v)
        if upU == upV:
            return
        if self.size[upU] < self.size[upV]:
            self.parent[upU] = upV
            self.size[upV] += self.size[upU]
        else:
            self.parent[upV] = upU
            self.size[upU] += self.size[upV]

class Solution:
    def smallestEquivalentString(self, s1: str, s2: str, baseStr: str) -> str:
        # Initialize DisjointSet for 26 letters of the alphabet
        ds = DisjointSet(26)

        # Union corresponding characters from s1 and s2
        for c1, c2 in zip(s1, s2):
            u = ord(c1) - ord('a')
            v = ord(c2) - ord('a')
            ds.unionBySize(u, v)

        # Create groups for characters based on their top parent
        grps = [[] for _ in range(26)]
        for u in range(26):
            uPar = ds.findUpar(u)
            grps[uPar].append(u)
        
        # Sort each group to ensure smallest lexicographical order
        for grp in grps:
            grp.sort()
        
        # Construct the result string
        res = []
        for c in baseStr:
            ch = ord(c) - ord('a')
            uPar = ds.findUpar(ch)
            # Get the smallest character in the group of the current character
            newCh = grps[uPar][0]
            res.append(chr(newCh + ord('a')))

		# Concatenate chars to string
        return ''.join(res)
```