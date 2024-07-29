The problem is to find the shortest distances between every pair of vertices in a given **edge-weighted directed** graph. The graph is represented as an adjacency matrix of size **n*n**. **Matrix[i][j]** denotes the weight of the edge from **i to j.** If **Matrix[i][j]=-1,** it means there is no edge from **i to j.**  
**Note : Modify** the distances **for every pair in-place.**

```cpp
// O (V ^ 3)


// When two numbers that are close to INT_MAX are added, the result can exceed the maximum value that 
// can be stored in an int, causing overflow. Overflow wraps around to negative values,
// which would then be incorrectly considered as a shorter path.

// Using 1e9 even if you add two paths with this maximum value, you get 2e9, 
// which is still within the range of a 32-bit integer.

class Solution {
  public:
	void shortest_distance(vector<vector<int>>&matrix){
	    int V = matrix.size();
	    
	    for (int i = 0; i < V; i++) {
	        for (int j = 0; j < V; j++) {
	            if (matrix[i][j] == -1)
	                matrix[i][j] = 1e9;
	        }
	    }
	    
	    for (int k = 0; k < V; k++) {
    	    for (int i = 0; i < V; i++) {
    	        for (int j = 0; j < V; j++) 
    	            matrix[i][j] = min(matrix[i][j], matrix[i][k] + matrix[k][j]);
	        }
	    }
	    
	    // For unreachable nodes
        for (int i = 0; i < V; i++) {
	        for (int j = 0; j < V; j++) {
	            if (matrix[i][j] == 1e9)
	                matrix[i][j] = -1;
	        }
        }
	    
	}
};
```