Given a Directed Graph with **V** vertices **(**Numbered from **0 to V-1)** and **E** edges, Find the number of strongly connected components in the graph.

```cpp
class Solution
{   private:
    void dfs(int node, vector<vector<int>>& adj, vector<int>& visit) {
        visit[node] = 1;
        
        for (auto adjNode : adj[node]) {
            if (!visit[adjNode])
                dfs(adjNode, adj, visit);
        }
        
    }

    void dfsUtil(int node, vector<vector<int>>& adj, vector<int>& visit, stack<int>& stk) {
        visit[node] = 1;
        
        for (auto adjNode : adj[node]) {
            if (!visit[adjNode])
                dfsUtil(adjNode, adj, visit, stk);
        }
        
        stk.push(node);
    }
	public:
	//Function to find number of strongly connected components in the graph.
    int kosaraju(int V, vector<vector<int>>& adj)
    {
        // STEP 1: Sort edges wrt finishing time
        stack<int> stk;
        vector<int> visit(V, 0);
        
        for (int node = 0; node < V; node++) {
            if (!visit[node])
                dfsUtil(node, adj, visit, stk);
        }
        
        // STEP 2: Reverse graph
        vector<vector<int>> adjT(V);
        fill(visit.begin(), visit.end(), 0);
        
        for (int node = 0; node < V; node++) {
            for (auto adjNode : adj[node])
                adjT[adjNode].push_back(node);
        }
        
        // STEP 3:
        int n_comps(0);
        
        while(!stk.empty()) {
            int node = stk.top();
            stk.pop();
            
            if (!visit[node]) {
                n_comps++;
                dfs(node, adjT, visit);
            }
            
        }
        
        
        return n_comps;
    }
};
```