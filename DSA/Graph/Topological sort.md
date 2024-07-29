Given an adjacency list for a Directed Acyclic Graph (DAG) where **adj_list[i]** contains a list of all vertices j such that there is a directed edge from vertex i to vertex j, with  **V**  vertices and **E**  edges, your task is to find any valid topological sorting of the graph.

In a topological sort, for every directed edge u -> v,  u must come before v in the ordering.

#### DFS

```cpp
class Solution
{   private:
    void dfs(int node, vector<int> adj[], vector<int>& visit, stack<int>& stk) {
        visit[node] = 1;
        
        for (auto neigh : adj[node]) {
            if (!visit[neigh]) {
                dfs(neigh, adj, visit, stk);
            }
        }
        // Backtrack
        stk.push(node);
    }
	public:
	//Function to return list containing vertices in Topological order. 
	vector<int> topoSort(int V, vector<int> adj[]) 
	{
	    vector<int> visit(V, 0);
	    stack<int> stk;
	    
	    for (int i=0; i<V; i++) {
	        if (!visit[i]) 
	            dfs(i, adj, visit, stk);
	    }
	        
	    vector<int> ans;
	    
	    while (!stk.empty()) {
	        ans.push_back(stk.top());
	        stk.pop();
	    }
	    
	    return ans;
	}
};
```

#### Kahn algorithm
```cpp
class Solution
{
	public:
	//Function to return list containing vertices in Topological order. 
	vector<int> topoSort(int V, vector<int> adj[]) 
	{
	    vector<int> indeg(V, 0);
	    
	    for (int i=0; i<V; i++) {
	        for (auto neigh : adj[i]) 
	            ++indeg[neigh];
	    }
	        
	    queue<int> q;
	    
	    // Push all vertices with indegree 0 to queue
        for (int i = 0; i < V; i++) {
            if (indeg[i] == 0)
                q.push(i);
        }
        
	    vector<int> ans;
	    
	    while (!q.empty()) {
	        int node = q.front();
	        q.pop();
	        
	        ans.push_back(node);
	        
	        for (auto neigh : adj[node]) {
	            --indeg[neigh];
	            if (!indeg[neigh])
	                q.push(neigh);
	        }
	    }
	    
	    return ans;
	}
};
```