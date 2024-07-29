Given an integer **N**, denoting the number of computers connected by cables forming a network and a 2d array **connections[][]**, with each row **(i, j)** representing a connection between **ith** and **jth** computer, the task is to connect all the computers either directly or indirectly by removing any of the given connections and connecting two disconnected computers.   
If its not possible to connect all the computers, return **-1**.   
Otherwise, return the minimum number of such operations required.

![[Pasted image 20240715220036.png]]

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

class Solution{
    public:
        int minimumConnections(int n , vector<vector<int>> &connections)
        {
            DisjointSet ds(n);
            int extra_edges(0);
            
            for (auto e : connections) {
                int u = e[0];
                int v = e[1];
                
                if (ds.findUPar(u) == ds.findUPar(v))
                    extra_edges++;
                else
                    ds.unionBySize(u, v);
            }
            
            int connected_comps(0);
            
            for (int i = 0; i < n; i++)
                if (ds.parent[i] == i)
                    connected_comps++;
                    
            int required_edges = connected_comps - 1;
            
            if (extra_edges >= required_edges)
                return required_edges;
            else
                return -1;
            
        }
};
```