We want to split a group of `n` people (labeled from `1` to `n`) into two groups of **any size**. Each person may dislike some other people, and they should not go into the same group.

Given the integer `n` and the array `dislikes` where `dislikes[i] = [ai, bi]` indicates that the person labeled `ai` does not like the person labeled `bi`, return `true` _if it is possible to split everyone into two groups in this way_.

#Bipartition #DSU #BFS
#### DSU
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
    def possibleBipartition(self, n: int, dislikes: List[List[int]]) -> bool:
        if not dislikes:
            return True
        
        ds = DisjointSet(n)
        adjList = [[] for _ in range(n + 1)]

        for a, b in dislikes:
            adjList[a].append(b)
            adjList[b].append(a)
        
        for node in range(1, n + 1):
            for neigh in adjList[node]:
                if ds.findUpar(node) == ds.findUpar(neigh):
                    return False
                else:
                    # union between the first neighbor and the current neighbor
                    # to group neighbors together
                    ds.unionBySize(adjList[node][0], neigh)
        
        return True
```

#### Bipartition method using BFS
```python
class Solution:
    def possibleBipartition(self, n: int, dislikes: List[List[int]]) -> bool:
        if not dislikes:
            return True

        adjList = [[] for _ in range(n + 1)]
        
        for a, b in dislikes:
            adjList[a].append(b)
            adjList[b].append(a)

        color = [-1] * (n + 1)

        for node in range(1, n+1):
            if color[node] == -1: # If node is not colored
                queue = [node]
                color[node] = 0 # Color the node
                
                while queue:
                    curNode = queue.pop(0)
                    
                    for nbr in adjList[curNode]:
                        if color[nbr] == color[curNode]: # Conflict
                            return False

                        elif color[nbr] == -1: # Uncolored
                            color[nbr] = 1 - color[curNode] # Color opposite
                            queue.append(nbr)

        # No conflicts found               
        return True
```