There are `n` servers numbered from `0` to `n - 1` connected by undirected server-to-server `connections` forming a network where `connections[i] = [ai, bi]` represents a connection between servers `ai` and `bi`. Any server can reach other servers directly or indirectly through the network.

A _critical connection_ is a connection that, if removed, will make some servers unable to reach some other server.

Return all critical connections in the network in any order.

```cpp
class Solution {
private:
    void dfs(int node, int& timer, vector<int>& disc, vector<int>& low, 
    vector<int>& visit, int parent, vector<vector<int>>& adj, 
    vector<vector<int>>& res) {
        visit[node] = 1;
        low[node] = disc[node] = timer++;

        for (auto neigh : adj[node]) {
            if (neigh == parent)
                continue;

            if(!visit[neigh]) {
                dfs(neigh, timer, disc, low, visit, node, adj, res);
                low[node] = min(low[node], low[neigh]);
                // Check if edge is bridge
                // If the low of neigh hasn't changed then
                // it must haven't found any other path
                if (low[neigh] > disc[node])
                    res.push_back({node, neigh});
            } 
            // If it has been visited then it's a back edge
            else {
                low[node] = min(low[node], disc[neigh]);
            } 
        }

    }
public:
    vector<vector<int>> criticalConnections(int n, 
    vector<vector<int>>& connections) {
        vector<vector<int>> adj(n);
        
        for (const auto& connection : connections) {
            int A = connection[0];
            int B = connection[1];
            
            adj[A].push_back(B);
            adj[B].push_back(A);
        }
        
        int timer = 0;
        vector<int> disc(n, -1); // Discovery time
        vector<int> low(n, -1); // Lowest discovery time
        vector<int> visit(n, 0);
        int parent = -1;

        vector<vector<int>> res;

        for (int i = 0; i < n; i++) {
            if (!visit[i])
                dfs(i, timer, disc, low, visit, parent, adj, res);
        }

        return res;
    }
};
```