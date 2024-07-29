Given a directed graph. The task is to do Breadth First Traversal of this graph starting from 0.  
**Note:** One can move from node u to node v only if there's an edge from u to v. Find the BFS traversal of the graph starting from the 0th vertex, from left to right according to the input graph. Also, you should only take nodes directly or indirectly connected from Node 0 in consideration.
### Code
```cpp
class Solution {
public:
   // Function to return Breadth First Traversal of given graph.
   vector<int> bfsOfGraph(int V, vector<int> adj[]) {
       
       // Create a queue to store the nodes for BFS traversal
       queue<int> q;
       
       // Start the BFS from node 0 (0-based indexing)
       int start = 0;
       
       // Create a boolean array to keep track of visited nodes
       int visit[V] = {0};
       
       // BFS traversal order
       vector<int> ls;
       
       q.push(start); // enqueue starting node
       visit[start] = 1; // mark it as visited
       
       // Continue BFS until the queue is empty
       while(!q.empty()) {
           // Dequeue a node from the queue
           int node = q.front();
           q.pop();
           
           // Add the dequeued node to the traversal order vector
           ls.push_back(node);
           
           // Enqueue all unvisited neighbors of the dequeued node
           for(auto neighbour : adj[node]) {
               if (visit[neighbour] != 1) {
                   visit[neighbour] = 1;
                   q.push(neighbour);
               }
           }
       }
       
       return ls;
   }
};
```