Given a weighted, undirected, and connected graph with V vertices and E edges, your task is to find the sum of the weights of the edges in the Minimum Spanning Tree (MST) of the graph. The graph is represented by an adjacency list, where each element adj[i] is a vector containing pairs of integers. Each pair represents an edge, with the first integer denoting the endpoint of the edge and the second integer denoting the weight of the edge.

### Prim's Algorithm

```cpp
class Solution
{
	public:
	//Function to find sum of weights of edges of the Minimum Spanning Tree.
    int spanningTree(int V, vector<vector<int>> adj[])
    {
        priority_queue<pair<int, int>,
            vector<pair<int, int>>,
            greater<pair<int, int>>> pq;
            
        vector<int> visit(V, 0);
        
        pq.push({0, 0});
        int sum(0);
        
        while(!pq.empty()) {
            int wt = pq.top().first;
            int node = pq.top().second;
            pq.pop();
            
            if (visit[node]) continue;
            
            sum += wt;
            visit[node] = 1;
            
            for (auto e : adj[node]) {
                int adjNode = e[0];
                int adjWt = e[1];
                
                if (!visit[adjNode]) 
                    pq.push({adjWt, adjNode});
             }
        }
        
        return sum;
    }
};
```

### Kruskal's Algorithm
```cpp
class DisjointSet {
private:
    vector<int> size, parent;
public:
    DisjointSet(int n) {
        size.resize(n + 1, 1);
        parent.resize(n + 1);
        for (int i = 0; i <= n; i++)
            parent[i] = i;
    }
    
    int findUPar(int node) {
        if (node == parent[node])
            return node;
        return parent[node] = findUPar(parent[node]);
    }
    
    void unionBySize(int u, int v) {
        int u_par = findUPar(u);
        int v_par = findUPar(v);
        
        if (u_par == v_par) return;
        
        if (size[u_par] < size[v_par]) {
            parent[u_par] = v_par;
            size[v_par] += size[u_par];
        }
        else {
            parent[v_par] = u_par;
            size[u_par] += size[v_par];
        }
    }
};

class Solution
{
public:
    //Function to find sum of weights of edges of the Minimum Spanning Tree.
    int spanningTree(int V, vector<vector<int>> adj[])
    {
        // wt, node, adjNode
        vector<pair<int, pair<int, int>>> edges;
        
        for (int i = 0; i < V; i++) {
            for (auto e : adj[i]) {
                int node = i;
                int adjNode = e[0];
                int wt = e[1];
                
                edges.push_back({wt, {node, adjNode}});
            }
        }
        sort(edges.begin(), edges.end());
        int sum = 0;
        
        DisjointSet ds(V);
        
        for (auto e : edges) {
            int wt = e.first;
            int node = e.second.first;
            int adjNode = e.second.second;
            
            if (ds.findUPar(node) != ds.findUPar(adjNode)) {
                sum += wt;
                ds.unionBySize(node, adjNode);
            }
        }
        
        return sum;
    }
};
```