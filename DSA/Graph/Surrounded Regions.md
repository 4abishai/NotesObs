You are given an `m x n` matrix `board` containing **letters** `'X'` and `'O'`, **capture regions** that are **surrounded**:

- **Connect**: A cell is connected to adjacent cells horizontally or vertically.
- **Region**: To form a region **connect every** `'O'` cell.
- **Surround**: The region is surrounded with `'X'` cells if you can **connect the region** with `'X'` cells and none of the region cells are on the edge of the `board`.

A **surrounded region is captured** by replacing all `'O'`s with `'X'`s in the input matrix `board`.
![[Pasted image 20240630223224.png]]

```cpp
class Solution {
private:
    void dfs(int i, int j, vector<vector<char>>& board, vector<vector<int>>& visit) {
        int n = board.size();
        int m = board[0].size();
        
        if (i<0 || j<0 || i>=n || j>=m || board[i][j]!='O' || visit[i][j])
            return;
        
        visit[i][j] = 1;

        // Check all 4 directions
        dfs(i+1, j, board, visit);
        dfs(i-1, j, board, visit);
        dfs(i, j+1, board, visit);
        dfs(i, j-1, board, visit);
    }
public:
    void solve(vector<vector<char>>& board) {
        int n = board.size();
        int m = board[0].size();
        vector<vector<int>> visit(n, vector<int>(m, 0));

        // traverse first and last row 
        for (int j=0; j<m; j++) {
            // first row
            if (board[0][j]=='O' && !visit[0][j])
                dfs(0, j, board, visit);
            
            // last row
            if (board[n - 1][j]=='O' && !visit[n - 1][j])
                dfs(n - 1, j, board, visit);
        }

        // traverse first and last col 
        for (int i=0; i<n; i++) {
            // first col
            if (board[i][0]=='O' && !visit[i][0])
                dfs(i, 0, board, visit);
            
            // last col
            if (board[i][m - 1]=='O' && !visit[i][m - 1])
                dfs(i, m - 1, board, visit);
        }

        // convert unvisited O rest to X
        for (int i=0; i<n; i++) {
            for (int j=0; j<m; j++) {
                if (board[i][j]=='O' && !visit[i][j])
                    board[i][j] = 'X';
            }
        }
    }
};
```