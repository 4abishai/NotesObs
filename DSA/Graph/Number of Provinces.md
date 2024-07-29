Given an **undirected** graph with **V** vertices. We say two vertices u and v belong to a single province if there is a path from u to v or v to u. Your task is to find the number of provinces.  
  
**Note:** A province is a group of **directly** or **indirectly connected** cities and no other cities outside of the group.

### Code
```cpp

class Solution
{
private:
    void dfs(int node, int visit[], vector<vector<int>> adjLs)
    {
        visit[node] = 1;

        for (auto neighbour : adjLs[node])
        {
            if (visit[neighbour] != 1)
                dfs(neighbour, visit, adjLs);
        }
    }

public:
    int numProvinces(vector<vector<int>> adj, int V)
    {
        // Create adj list
        vector<vector<int>> adjLs(V);

        for (int i = 0; i < V; i++)
        {
            for (int j = 0; j < V; j++)
            {
                if (adj[i][j] == 1)
                {
                    adjLs[i].push_back(j);
                    adjLs[j].push_back(i);
                }
            }
        }

        int visit[V] = {0};
        int count = 0;

        for (int node = 0; node < V; node++)
        {
            if (visit[node] != 1)
            {                            // for each non visited node
                count++;                 // must be non neighbouring
                dfs(node, visit, adjLs); // call dfs
            }
        }
        return count;
    }
};
```