Given a Directed Acyclic Graph of N vertices from 0 to N-1 and a 2D Integer array(or vector) edges[ ][ ] of length M, where there is a directed edge from edge[i][0] to edge[i][1] with a distance of edge[i][2] for all i.

Find the **shortest** path from **src(0)** vertex to all the vertices and if it is impossible to reach any vertex, then return **-1** for that vertex.

![[Pasted image 20240705143319.png]]

```cpp
class Solution {
    private:
    void topoSort(int node, vector<pair<int, int>> adj[], vector<int>& visit, stack<int>& stk) {
        visit[node] = 1;
        
        for (auto e : adj[node]) {
            int neigh = e.first;
            if (!visit[neigh])
                topoSort(neigh, adj, visit, stk);
        }
        
        stk.push(node);
    }
  public:
     vector<int> shortestPath(int N,int M, vector<vector<int>>& edges){
        // Make adj list
        vector<pair<int, int>> adj[N];
        for (int i = 0; i < M; i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            int wt = edges[i][2];
            
            adj[u].push_back({v, wt});
        }    
         
        // Get the stack using topo sort
        vector<int> visit(N, 0);
        stack<int> stk;
        
        for (int i = 0; i < N; i++) {
            if (!visit[i])
                topoSort(i, adj, visit, stk);
        }
        
        // Calculate the distance
        vector<int> dist(N, 1e9);
        dist[0] = 0;
        while(!stk.empty()) {
            int node = stk.top();
            stk.pop();
            for (auto e : adj[node]) {
                int neigh = e.first;
                int wt = e.second;
                
                if (dist[node] + wt < dist[neigh]) {
                    dist[neigh] = dist[node] + wt;
                } 
            }
        }
        
        // Replace unreachable nodes with -1
        for (int& d : dist) {
            if (d == 1e9) d = -1;
        }
        
        return dist;
     }
};
```