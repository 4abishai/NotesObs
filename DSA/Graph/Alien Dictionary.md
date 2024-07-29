Given a sorted dictionary of an alien language having N words and k starting alphabets of standard dictionary. Find the order of characters in the alien language.  
**Note:** Many orders may be possible for a particular test case, thus you may return any valid order and output will be 1 if the order of string returned by the function is correct else 0 denoting incorrect string returned.

![[Pasted image 20240705112742.png]]

```cpp
class Solution{
    private:
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
    public:
    string findOrder(string dict[], int N, int K) {
        // make graph
        vector<int> adj[K];
        
        for (int i = 0; i < N - 1; i++) {
            string s1 = dict[i];
            string s2 = dict[i + 1];
            int min_len = min(s1.size(), s2.size());
            
            for (int p = 0; p < min_len; p++) {
                if (s1[p] != s2[p]) {
                    adj[s1[p] - 'a'].push_back(s2[p] - 'a');
                    break;
                }
            }
        }
        
        // get topological sort
        vector<int> topo_sort = topoSort(K, adj);
        string ans = "";
        for (auto e : topo_sort)
            ans += char(e + 'a');
        
        return ans;
    }
};
```