You are given an **n x n binary matrix** grid. You are allowed to change **at most one 0** to be **1**. A group of **connected 1s** forms an island. Two 1s are connected if they share one of their sides with each other.

Return the size of the **largest island** in the grid after applying this operation.

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
            size[up_u] += size[up_v];
        }
    }
};
class Solution
{
private:
    bool isValid(int r, int c, int n, int m)
    {
        return r >= 0 && r < n && c >= 0 && c < m;
    }
public:
    int largestIsland(vector<vector<int>>& grid) 
    {
        int n = grid.size();
        int m = grid[0].size();
        
        DisjointSet ds(n * m);
        
        int dr[] = {-1, 0, 1, 0};
        int dc[] = {0, 1, 0, -1};
        // STEP 1
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 0)
                    continue;
            
                
                for (int k = 0; k < 4; k++) {
                    int nr = i + dr[k];
                    int nc = j + dc[k];
                    
                    if (isValid(nr, nc, n, m) && grid[nr][nc]) {
                        int node = i * m + j;
                        int adjNode = nr * m + nc;
                        
                        ds.unionBySize(node, adjNode);
                    }
                }
            }
        }
        
        // STEP 2
        int max_size(0);
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 1) continue;
                
                unordered_set<int> st;  
                
                for (int k = 0; k < 4; k++) {
                    int nr = i + dr[k];
                    int nc = j + dc[k];
                    
                    if (isValid(nr, nc, n, m) && grid[nr][nc]) {
                        int adjNode = nr * m + nc;
                        st.insert(ds.findUPar(adjNode));
                    }
                }
                
                int totalSize(0);
                
                for (auto node : st)
                    totalSize += ds.size[node];
                
                max_size = max(max_size, totalSize + 1);
            }
        }
        
       // Edge Case: If no 0 was found or the largest island wasn't formed by flipping a 0
        if (max_size == 0) {
            for (int cell = 0; cell < n * m; cell++)
                max_size = max(max_size, ds.size[ds.findUPar(cell)]);
        }
        
        return max_size;
    }
};
```