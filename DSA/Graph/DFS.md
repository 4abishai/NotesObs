## Question
You are given a connected undirected graph. Perform a Depth First Traversal of the graph.  
**Note:** Use the recursive approach to find the DFS traversal of the graph starting from the 0th vertex from left to right according to the graph.

### Answer
```cpp
class Solution {
private:
    // This is a recursive function that performs Depth-First Search (DFS)
    // traversal on the graph starting from the given 'node'
    void dfs(int node, vector<int> adj[], int visit[], vector<int>& ls) {
    
        // Mark the current node as visited
        visit[node] = 1;
        
        // Add the current node to the traversal list
        ls.push_back(node);
        
        // Traverse all the neighbours of the current node
        for(auto neighbour : adj[node]) {
            // If the neighbour is not visited yet, recursively call dfs on it
            if(visit[neighbour] != 1) 
                dfs(neighbour, adj, visit, ls);
        }
        
    }
  public:
    // This function performs DFS traversal on the entire graph
    // and returns a vector containing the traversal order
    vector<int> dfsOfGraph(int V, vector<int> adj[]) {
    
        // Create a visited array to keep track of visited nodes
        int visit[V] = {0};
        
        // Choose an arbitrary start node (0 in this case)
        int start = 0; // 0 based indexing
        
        // Create a vector to store the traversal order
        vector<int> ls;
        
        // Call the dfs function with the start node
        dfs(start, adj, visit, ls);
        
        // Return the traversal order
        return ls;    
    }
};
```