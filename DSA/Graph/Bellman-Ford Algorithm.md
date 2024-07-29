Given a weighted and directed graph of V vertices and E edges, Find the shortest distance of all the vertex's from the source vertex S. If a vertices can't be reach from the S then mark the distance as 10^8. Note: If the Graph contains a negative cycle then return an array consisting of only -1.

```cpp
//O(V * E)

class Solution {
  public:
    /*  Function to implement Bellman Ford
    *   edges: vector of vectors which represents the graph
    *   S: source vertex to start traversing graph with
    *   V: number of vertices
    */
    vector<int> bellman_ford(int V, vector<vector<int>>& edges, int S) {
            
        vector<int> dist(V, 1e8);
        dist[S] = 0;
        
        // Relax edges for V - 1 iteration
        // to find the shortest paths from a source node to all other nodes
        for (int i = 0; i < V - 1; i++) {
            for (auto edge : edges) {
                int node = edge[0];
                int adj_node = edge[1];
                int wt = edge[2];
                //if the distance is more then no need to update distance
                if(dist[node]!=(1e8) && dist[node]+wt < dist[adj_node])
                    dist[adj_node] = dist[node]+wt;
                
            }
        }
        
        // If we perform an Nth iteration and still find a way to reduce the distance to any node
        // cycle exists
        for (auto edge : edges) {
            int node = edge[0];
            int adj_node = edge[1];
            int wt = edge[2];
            //if the distance is more then no need to update distance
            if(dist[node]!=(1e8) && dist[node]+wt < dist[adj_node])
                return {-1};
        }
        
        return dist;
    }
};
```