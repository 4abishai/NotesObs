There are a total of N tasks, labeled from 0 to N-1. Some tasks may have prerequisites, for example to do task 0 you have to first complete task 1, which is expressed as a pair: [0, 1]  
Given the total number of **tasks N** and a list of **prerequisite pairs P**, find if it is possible to finish all tasks.

```cpp
class Solution {
public:
	bool isPossible(int V,int P, vector<pair<int, int> >& prerequisites) {
	    // Form a graph
	    vector<int> adj[V];
	    for (auto e : prerequisites)
	        adj[e.second].push_back(e.first);
	        
	   // Detect cycle using Kahn algorithm
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
	    
	    // If all nodes are visited, no cycle exists
	    // Task completion is possible
	    if (cnt == V)
	        return true;
	   
	   return false;
	}
};
```