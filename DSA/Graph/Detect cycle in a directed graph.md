Given a Directed Graph with **V** vertices (Numbered from **0** to **V-1**) and **E** edges, check whether it contains any cycle or not.
### Code
#### DFS
```cpp
class Solution {
private:
    // DFS function to detect a cycle in the graph
    bool dfs(int node, vector<int> &visit, vector<int> &pathVisit, vector<int> adj[]) {
        visit[node] = 1;
        pathVisit[node] = 1;

        for (auto neighbour : adj[node]) {
            // If the neighbor is not visited
            if (!visit[neighbour]) {
                // Recursively call DFS on the neighbor
                if (dfs(neighbour, visit, pathVisit, adj))
                    return true; // If a cycle is found, return true
            }
            // If the neighbor is already part of the current recursive path,
            // it means we have found a cycle
            else if (pathVisit[neighbour])
                return true;
        }

        // After exploring all neighbors, remove the current node from the current recursive path
        pathVisit[node] = 0;
        return false; // No cycle found
    }

public:
    // Function to detect cycle in a directed graph
    bool isCyclic(int V, vector<int> adj[]) {
        vector<int> visit(V, 0);
        vector<int> pathVisit(V, 0);

        // Traverse all nodes in the graph
        for (int node = 0; node < V; ++node) {
            if (!visit[node]) {
                if (dfs(node, visit, pathVisit, adj))
                    return true; // If a cycle is found, return true
            }
        }

        // No cycle found
        return false;
    }
};
```

#### Kahn Algorithm
```cpp
class Solution { 
  public:
    // Function to detect cycle in a directed graph.
    bool isCyclic(int V, vector<int> adj[]) {
        vector<int> indeg(V, 0);
	    
	    for (int i=0; i<V; i++) {
	        for (auto neigh : adj[i]) 
	            ++indeg[neigh];
	    }
	        
	    queue<int> q;
	    
	    // Push all vertices with indegree 0 to queue
        for (int i = 0; i < V; i++) {
            if (indeg[i] == 0) {
                q.push(i);
            }
        }
	    
	    int cnt(0);
	    
	    while (!q.empty()) {
	        int node = q.front();
	        q.pop();
	        
            ++cnt;
	        
	        for (auto neigh : adj[node]) {
	            --indeg[neigh];
	            if (!indeg[neigh])
	                q.push(neigh);
	        }
	    }
	    
	    if (cnt == V)
	        return false;
	   
	   return true;
    }
};
```