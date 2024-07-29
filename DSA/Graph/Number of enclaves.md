You are given an `m x n` binary matrix `grid`, where `0` represents a sea cell and `1` represents a land cell.

A **move** consists of walking from one land cell to another adjacent (**4-directionally**) land cell or walking off the boundary of the `grid`.

Return _the number of land cells in_ `grid` _for which we cannot walk off the boundary of the grid in any number of **moves**_.

```cpp
class Solution {
private:
    void dfs(int i, int j, vector<vector<int>>& grid, vector<vector<int>>& visit) {
        int n = grid.size();
        int m = grid[0].size();
        
        if (i<0 || j<0 || i>=n || j>=m || grid[i][j]==0 || visit[i][j])
            return;
        
        visit[i][j] = 1;

        // Check all 4 directions
        dfs(i+1, j, grid, visit);
        dfs(i-1, j, grid, visit);
        dfs(i, j+1, grid, visit);
        dfs(i, j-1, grid, visit);
    }
public:
    int numEnclaves(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();

        // Visit 1s cells on the boundary
        // by calling dfs on them

        vector<vector<int>> visit(n, vector<int>(m, 0));

        // Traverse first and last row
        for (int j=0; j<m; j++) {
            if (grid[0][j] && !visit[0][j])
                dfs(0, j, grid, visit);
            
            if (grid[n - 1][j] && !visit[n - 1][j])
                dfs(n - 1, j, grid, visit);
        }

        // Traverse first and last col
         for (int i=0; i<n; i++) {
            if (grid[i][0] && !visit[i][0])
                dfs(i, 0, grid, visit);
            
            if (grid[i][m - 1] && !visit[i][m - 1])
                dfs(i, m - 1, grid, visit);
        }

        int cnt = 0;
        // count non-visited 1s as enclaves
        for (int i=0; i<n; i++) {
            for (int j=0; j<m; j++) {
                if (grid[i][j] && !visit[i][j])
                    ++cnt;
            }
        }
        return cnt;
    }
};
```