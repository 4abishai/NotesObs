You are given a weighted undirected graph having **n** vertices numbered from **1 to n** and **m** edges along with their weights. Find the **shortest path** between the vertex 1 and the vertex **n**,  if there exists a path, and return a **list of integers** whose **first element** is the **weight** of the path, and the rest consist of the **nodes** on that path. If no path exists, then return a list containing a single element **-1**.

The input list of edges is as follows - **{a, b, w}**, denoting there is an edge between **a** and **b**, and **w** is the weight of that edge.

```cpp
class Solution {
  public:
    vector<int> shortestPath(int n, int m, vector<vector<int>>& edges) {
        vector<pair<int, int>> adj[n + 1];
        for (auto e : edges) {
            adj[e[0]].push_back({e[1], e[2]});
            adj[e[1]].push_back({e[0], e[2]});
        }
        
        // Min heap to store vertices with their distances
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        
        vector<int> dist(n + 1, INT_MAX);
        vector<int> parent(n + 1);
        
        for (int i = 0; i < n + 1; i++) parent[i] = i;
        
        int src = 1;
        dist[src] = 0;
        
        pq.push({0, src});
        
        while(!pq.empty()) {
            int dis = pq.top().first;
            int node = pq.top().second;
            pq.pop();
            
            for (auto neigh : adj[node]) {
                int adjNode = neigh.first;
                int wt = neigh.second;
                
                // If there is a shorter path to adjNode through node
                if (dis + wt < dist[adjNode]) {
                    dist[adjNode] = dis + wt;
                    parent[adjNode] = node;
                    pq.push({dist[adjNode], adjNode});
                }
            }
            
        }
        
        
        if (dist[n] == INT_MAX) return {-1};
        
        vector<int> path;
        int totalWt(0);
        
        int node = n;
        while(node != src) {
            path.push_back(node);
            int parentNode = parent[node];
            for (auto neigh : adj[parentNode]) {
                if (neigh.first == node) {
                    totalWt += neigh.second;
                    break;
                }
            }
            node = parent[node];
        }
        path.push_back(src);
        reverse(path.begin(), path.end());
        
        vector<int> ans;
        ans.push_back(totalWt);
        ans.insert(ans.end(), path.begin(), path.end());
        
        return ans;
    }
};
```