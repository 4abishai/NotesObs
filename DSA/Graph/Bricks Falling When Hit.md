You are given an `m x n` binary `grid`, where each `1` represents a brick and `0` represents an empty space. A brick is **stable** if:

- It is directly connected to the top of the grid, or
- At least one other brick in its four adjacent cells is **stable**.

You are also given an array `hits`, which is a sequence of erasures we want to apply. Each time we want to erase the brick at the location `hits[i] = (rowi, coli)`. The brick on that location (if it exists) will disappear. Some other bricks may no longer be stable because of that erasure and will **fall**. Once a brick falls, it is **immediately** erased from the `grid` (i.e., it does not land on other stable bricks).

Return _an array_ `result`_, where each_ `result[i]` _is the number of bricks that will **fall** after the_ `ith` _erasure is applied._

**Note** that an erasure may refer to a location with no brick, and if it does, no bricks drop.

#DSU 

```python
class DisjointSet:
    def __init__(self, n):
        self.size = [1] * (n + 1)
        self.parent = list(range(n + 1))

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
    def hitBricks(self, grid: List[List[int]], hits: List[List[int]]) -> List[int]:

        copyGrid = [row[:] for row in grid]
        
        # Hit all bricks
        for i, j in hits:
            copyGrid[i][j] = 0

        # Union remaining bricks 
        n, m = len(grid), len(grid[0])
        def idx(i, j):
            return m * i + j

        ds = DisjointSet(n * m) #  +1 for roof (index m*n)

        for i in range(n):
            for j in range(m):
                # Skip empty spaces
                if copyGrid[i][j] == 0:
                    continue

                # Stable bricks at roof and serve as root
                if i == 0:
                    ds.unionBySize(n * m, idx(i, j))
                if i > 0 and copyGrid[i - 1][j] == 1:
                    ds.unionBySize(idx(i, j), idx(i - 1, j))
                if j > 0 and copyGrid[i][j - 1] == 1:
                    ds.unionBySize(idx(i, j), idx(i, j - 1))

        # Reverse all the hits
        result = [0] * len(hits)

        for k in range(len(hits) - 1, -1, -1):
            i, j = hits[k]

            # Skip empty spaces
            if grid[i][j] == 0:
                continue
            
            prevStableSize = ds.size[ds.findUpar(n * m)]
            copyGrid[i][j] = 1 # hit

            if i == 0:
                ds.unionBySize(n * m, idx(i, j))

            dr = [-1, 0, 1, 0]
            dc = [0, 1, 0, -1]
            for d in range(4):
                nr = dr[d] + i
                nc = dc[d] + j

                if 0 <= nr < n and 0 <= nc < m and copyGrid[nr][nc] == 1:
                    ds.unionBySize(idx(i, j), idx(nr, nc))

            newStableSize = ds.size[ds.findUpar(n * m)]
            result[k] = max(0, newStableSize - prevStableSize - 1)
        
        return result
```