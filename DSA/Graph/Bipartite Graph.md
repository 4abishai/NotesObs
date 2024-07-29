Given an adjacency list of a graph **adj** of V no. of vertices having 0 based index. Check whether the graph is bipartite or not.
#Bipartition #BFS #DFS #DSU
#### BFS

```cpp
class Solution {
private:
    bool isBipartiteBFS(int start, int V, vector<int> adj[], vector<int>& color) {
        queue<int> q;
        
        q.push(start);
        color[start] = 0;
        
        while (!q.empty()) {
            int node = q.front();
            q.pop();
            
            for (auto neigh : adj[node]) {
                // if neigh is not colored
                if (color[neigh] == -1) {
                    color[neigh] = !color[node];
                    q.push(neigh);
                }
                // if neigh is colored the same as adj node
                else if (color[neigh] == color[node])
                    return false;
            }
        }
        return true;
    }
public:
    bool isBipartite(int V, vector<int>adj[]){
        vector<int> color(V, -1);
        
        for (int i=0; i<V; i++) {
            if (color[i] == -1) {
                if (!isBipartiteBFS(i, V, adj, color)) return false;
            }
        }
        
        return true;
    }
};
```

#### DFS
```cpp
class Solution {
private:
    bool isBipartiteDFS(int node, int initColor, vector<int> adj[], vector<int>& color) {
        color[node] = initColor;
        
        for (auto neigh : adj[node]) {
            // If neigh is not colored
            if (color[neigh] == -1) {
                if (!isBipartiteDFS(neigh, !initColor, adj, color))
                    return false;
            }
            // if neigh is colored the same as adj node
            else if (color[neigh] == color[node])
                return false;
        }
        
        return true;
    }
public:
    bool isBipartite(int V, vector<int>adj[]){
        vector<int> color(V, -1);
        
        // Check for components
        for (int i=0; i<V; i++) {
            if (color[i] == -1) {
                if (!isBipartiteDFS(i, 0, adj, color)) return false;
            }
        }
        
        return true;
    }
};
```

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
    def isBipartite(self, graph: List[List[int]]) -> bool:

        n = len(graph)
        ds = DisjointSet(n)

        for node in range(n):
            for nbr in graph[node]:
                if ds.findUpar(node) == ds.findUpar(nbr):
                    return False
                else:
                    ds.unionBySize(graph[node][0], nbr)

        return True
```