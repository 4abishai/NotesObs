Given a boolean 2D matrix **grid** of size **n** * **m**. You have to find the number of distinct islands where a group of connected 1s (horizontally or vertically) forms an island. Two islands are considered to be distinct if and only if one island is not equal to another (not rotated or reflected).
![[Pasted image 20240630222838.png]]
```cpp
class Solution {
private:
    void dfs(int r, int c, vector<vector<int>>& grid, vector<vector<int>>& visit, vector<pair<int,int>>& v, int r_beg, int c_beg) {
        int n = grid.size();
        int m = grid[0].size();
        
        if (r < 0 || r >= n || c < 0 || c >= m || !grid[r][c] || visit[r][c]) {
            return;
        }
        
        visit[r][c] = 1;
        v.push_back({r - r_beg, c - c_beg});
        
        // Check all 4 directions
        dfs(r-1, c, grid, visit, v, r_beg, c_beg);
        dfs(r+1, c, grid, visit, v, r_beg, c_beg);
        dfs(r, c+1, grid, visit, v, r_beg, c_beg);
        dfs(r, c-1, grid, visit, v, r_beg, c_beg);
    }
  public:
    int countDistinctIslands(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();
        
        vector<vector<int>> visit(n, vector<int> (m, 0));
        // set ignores duplicate values
        set<vector<pair<int, int>>> st; 
    
        for (int i=0; i<n; i++) {
            for (int j=0; j<m; j++) {
                if (grid[i][j] && !visit[i][j]) {
                    vector<pair<int,int>> v;
                    dfs(i, j, grid, visit, v, i, j);
                    st.insert(v);
                }
            }
        }
        
        return st.size();
    }
};
```