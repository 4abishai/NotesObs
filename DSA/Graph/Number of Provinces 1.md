Given an **undirected** graph with **V** vertices. We say two vertices u and v belong to a single province if there is a path from u to v or v to u. Your task is to find the number of provinces.  
  
**Note:** A province is a group of **directly** or **indirectly connected** cities and no other cities outside of the group.

```cpp
class DisjointSet
{

public:
    vector<int> size, parent;
    
    DisjointSet(int n)
    {
        size.resize(n + 1, 1);
        parent.resize(n + 1);
        for (int i = 0; i < n + 1; i++)
            parent[i] = i;
    }

    // Find ultimate parent
    int findUPar(int node)
    {
        if (node == parent[node])
            return node;
        else
            return parent[node] = findUPar(parent[node]);
    }

    void unionBySize(int u, int v)
    {
        int up_u = findUPar(u);
        int up_v = findUPar(v);

        if (up_u == up_v)
            return;

        // We attach the smaller tree to the taller tree
        if (size[up_u] < size[up_v])
        {
            parent[up_u] = up_v;
            size[up_v] += size[up_u];
        }

        else
        {
            parent[up_v] = up_u;
            size[up_u] += size[up_u];
        }
    }
};

class Solution {
  public:
    int numProvinces(vector<vector<int>> adj, int V) {
        DisjointSet ds(V);
        
        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                if (adj[i][j]) // if edge exits between i and j
                    ds.unionBySize(i, j);
            }
        }
        
        int num(0);
        
        for (int i = 0; i < V; i++) 
            if (ds.parent[i] == i)
                num++;
        
        return num; 
    }
};
```