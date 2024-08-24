An `n x n` grid is composed of `1 x 1` squares where each `1 x 1` square consists of a `'/'`, `'\'`, or blank space `' '`. These characters divide the square into contiguous regions.

Given the grid `grid` represented as a string array, return _the number of regions_.

Note that backslash characters are escaped, so a `'\'` is represented as `'\\'`.

#DSU 

![[Pasted image 20240801000158.png]]

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
    def regionsBySlashes(self, grid: List[str]) -> int:
        n = len(grid)
        ds = DisjointSet(4 * n * n)

        # Function to get index of cells's root triangle 
        def getRoot(i, j):
            return (n * i + j) * 4 # cell's index * triangles in cell

        for i in range(n):
            for j in range(n):
                # Connect traingles in a cell
                if grid[i][j] != '/':
                    ds.unionBySize(getRoot(i, j) + 1, getRoot(i, j) + 2)
                    ds.unionBySize(getRoot(i, j), getRoot(i, j) + 3)
                if grid[i][j] != '\\':
                    ds.unionBySize(getRoot(i, j), getRoot(i, j) + 1)
                    ds.unionBySize(getRoot(i, j) + 2, getRoot(i, j) + 3)
                
                # Connect triangles in cell to nbr cell
                if i > 0:
                    ds.unionBySize(getRoot(i - 1, j) + 3, getRoot(i, j) + 1)
                if j > 0:
                    ds.unionBySize(getRoot(i, j - 1) + 2, getRoot(i, j))
        
        nPartitions = sum(1 for i in range(4 * n * n) if ds.findUpar(i) == i)
        
        return nPartitions
```