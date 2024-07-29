Given an undirected connected graph with **V** vertices and adjacency list **adj**. You are required to find all the vertices removing which (and edges through it) disconnects the graph into 2 or more components and return it in sorted manner.  
**Note:** Indexing is zero-based i.e nodes numbering from (0 to V-1). There might be loops present in the graph.

https://youtu.be/fqkqx6OBRDE?si=MHdwDhTOIeTt60T8

```cpp
class Solution {
private:
    void dfs(int node, int parent, int& timer, vector<int>& disc, vector<int>& low, vector<int>& visit,
    vector<vector<int>>& graph, set<int>& articulationPoints) {
        visit[node] = 1;
        disc[node] = low[node] = timer++;
        
        int child = 0;
        for (auto neigh : graph[node]) {
            if (neigh == parent) continue;
            
            if (!visit[neigh]) {
                dfs(neigh, node, timer, disc, low, visit, graph, articulationPoints);
                low[node] = min(low[node], low[neigh]);
                // Check if it's AP or not
                if (low[neigh] >= disc[node] && parent != -1)
                    articulationPoints.insert(node);
            
                child++;
            }
            else {
                low[node] = min(low[node], disc[neigh]);
            }
        }
        
        if (parent == -1 && child > 1)
            articulationPoints.insert(node);
    }
public:
    vector<int> articulationPoints(int V, vector<int>adj[]) {
        vector<vector<int>> graph(V);
        
        for (int u = 0; u < V; u++) {
            for (int v : adj[u]) {
                graph[u].push_back(v);
            }
        }
        
        int timer = 0;
        vector<int> disc(V, -1);
        vector<int> low(V, -1);
        vector<int> visit(V, 0);
        set<int> articulationPoints;
        
        for (int i = 0; i < V; i++) {
            if (!visit[i])
                dfs(i, -1, timer, disc, low, visit, graph, articulationPoints);
        }
        
        vector<int> res(articulationPoints.begin(), articulationPoints.end());
        
        if (res.empty()) res.push_back(-1);
        
        return res;
    }
};
```