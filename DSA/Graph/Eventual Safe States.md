A directed graph of **V** vertices and **E** edges is given in the form of an adjacency list **adj**. Each node of the graph is labelled with a distinct integer in the range **0** to **V - 1**.

A node is a **terminal node** if there are no outgoing edges. A node is a **safe node** if every possible path starting from that node leads to a **terminal node**.

You have to return an array containing all the **safe nodes** of the graph. The answer should be sorted in **ascending** order.

![[Pasted image 20240701173614.png]]

#### DFS

```cpp
class Solution {
private:
    bool isCyclicDFS(int node, vector<int> adj[], vector<int>& visit, vector<int>& path_visit, vector<int>& check_safe) {
        visit[node] = 1;
        path_visit[node] = 1;
        check_safe[node] = 0;
        
        for (auto neigh : adj[node]) {
            // If the neigh is not visited
            if (!visit[neigh]) {
                if (isCyclicDFS(neigh, adj, visit, path_visit, check_safe)) {
                    // neigh leads to the cycle
                    return true;
                }
            }
            // If the neigh has been previously visited
            // and it is visiting same path again
            // i.e, neigh is part of cycle
            else if (path_visit[neigh])
                return true;
        }
        
        // Backtrack
        // If not part of the cycle
        check_safe[node] = 1;   // node is safe
        path_visit[node] = 0;  
        return false;
    }
  public:
    vector<int> eventualSafeNodes(int V, vector<int> adj[]) {
        vector<int> visit(V, 0);
        vector<int> path_visit(V, 0);
        vector<int> check_safe(V, 0);
        vector<int> safe_nodes;
        
        for (int i = 0; i < V; i++) {
            if (!visit[i]) {
                isCyclicDFS(i, adj, visit, path_visit, check_safe);
            }
        }
        
        for (int i = 0; i < V; i++) {
            if (check_safe[i]) safe_nodes.push_back(i);
        }
        
        return safe_nodes;
    }
};
```

#### Kahn Algorithm
```cpp
class Solution {
public:
    vector<int> eventualSafeNodes(int V, vector<int> adj[]) {
        vector<int> adj_rev[V];
        vector<int> indeg(V, 0);
        // Reverse the edges
        for (int i = 0; i < V; i++) {
            for (auto e : adj[i]) {
                adj_rev[e].push_back(i);
                ++indeg[i];
            } 
        }
        
        queue<int> q;
        vector<int> safe_nodes;
        
        for (int i = 0; i < V; i++) {
            if (!indeg[i])
                q.push(i);
        }
        
        while (!q.empty()) {
            int node = q.front();
            q.pop();
            safe_nodes.push_back(node);
            
            for (auto neigh : adj_rev[node]) {
                --indeg[neigh];
                if (!indeg[neigh])
                    q.push(neigh);
            }
        }
        
        sort(safe_nodes.begin(), safe_nodes.end());
        return safe_nodes;
    }
};
```