You are given an **Undirected Graph** having **unit weight** of the edges, find the shortest path from **src** to all the vertex and if it is **unreachable** to reach any vertex, then return **-1** for that vertex.

```cpp
class Solution {
public:
    vector<int> shortestPath(vector<vector<int>>& edges, int N, int M, int src) {
        vector<vector<int>> adj(N);
        for (auto e : edges) {
            int u = e[0];
            int v = e[1];
            adj[u].push_back(v);
            adj[v].push_back(u);
        }
        
        vector<int> dist(N, 1e9);
        
        queue<int> q;
        q.push(src);
        dist[src] = 0;
        
        while (!q.empty()) {
            int node = q.front();
            q.pop();
            
            for (auto neigh : adj[node]) {
                if (dist[node] + 1 < dist[neigh]) {
                    dist[neigh] = dist[node] + 1;
                    q.push(neigh);
                }
            }
        }
        
        vector<int> ans(N, -1);
        for (int i = 0; i < N; i++) {
            if (dist[i] != 1e9)
                ans[i] = dist[i];
        }
        
        return ans;
    }
};
```