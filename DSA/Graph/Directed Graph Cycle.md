Given a Directed Graph with **V** vertices (Numbered from **0** to **V-1**) and **E** edges, check whether it contains any cycle or not.
![[Pasted image 20240630222806.png]]

```cpp
class Solution {
private:
    bool isCyclicDFS(int node, vector<int> adj[], vector<int>& visit, vector<int>& path_visit) {
        visit[node] = 1;
        path_visit[node] = 1;
        
        for (auto neigh : adj[node]) {
            // If the neigh is not visited
            if (!visit[neigh]) {
                if (isCyclicDFS(neigh, adj, visit, path_visit)) return true;
            }
            // If the neigh has been previously visited
            // and it is visiting same path again
            else if (path_visit[neigh]) return true;
        }
        
        path_visit[node] = 0;  // Backtrack
        return false;
    }
  public:
    // Function to detect cycle in a directed graph.
    bool isCyclic(int V, vector<int> adj[]) {
        vector<int> visit(V, 0);
        vector<int> path_visit(V, 0);
        
        for (int i = 0; i < V; i++) {
            if (!visit[i]) {
                if (isCyclicDFS(i, adj, visit, path_visit)) return true;
            }
        }
        
        return false;
    }
};
```