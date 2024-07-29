There are a total of **n** tasks you have to pick, labelled from 0 to n-1. Some tasks may have **prerequisite** tasks, for example to pick task 0 you have to first finish tasks 1, which is expressed as a pair: [0, 1]  
Given the total number of **n** tasks and a list of prerequisite pairs of size **m**. Find a ordering of tasks you should pick to finish all tasks.  
**Note:** There may be multiple correct orders, you just need to return any one of them. If it is impossible to finish all tasks, return an empty array. Driver code will print **"No Ordering Possible"**, on returning an empty array. Returning any correct order will give the output as **1**, whereas any invalid order will give the output **0**.

```cpp
class Solution
{
  public:
    vector<int> findOrder(int V, int m, vector<vector<int>> prerequisites) 
    {
        // Form a graph
	    vector<int> adj[V];
	    for (auto e : prerequisites)
	        adj[e[1]].push_back(e[0]);
    
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
	    
	    // No cycle exists => Tasks completion possible 
	    if (ans.size() == V)
	        return ans;
	   
	   return {};
    }
};
```