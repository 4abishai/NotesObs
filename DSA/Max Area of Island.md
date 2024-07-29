You are given an `m x n` binary matrix `grid`. An island is a group of `1`'s (representing land) connected **4-directionally** (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The **area** of an island is the number of cells with a value `1` in the island.

Return _the maximum **area** of an island in_ `grid`. If there is no island, return `0`.

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

class Solution {
private:
    bool isValid(int i, int j, int n, int m) {
        return i >= 0 && i < n && j >= 0 && j < m;
    }
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();
        DisjointSet ds(n * m);

        int dr[] = {-1, 0, 1, 0}; 
        int dc[] = {0, 1, 0, -1}; 
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j]) {
                    for (int k = 0; k < 4; k++) {
                        int ni = i + dr[k];
                        int nj = j + dc[k];
                        
                        if (isValid(ni, nj, n, m) && grid[ni][nj]) {
                            int node = i * m + j;
                            int adjNode = ni * m + nj;
                            ds.unionBySize(node, adjNode);
                        }
                    }
                }
            }
        }

        int maxArea = 0;
        
        // Then, find the maximum size among all land cells
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 1) {
                    int node = i * m + j;
                    maxArea = max(maxArea, ds.size[ds.findUPar(node)]);
                }
            }
        }

        return maxArea;
    }
};
```