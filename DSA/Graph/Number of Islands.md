Given a grid of size n*m (n is the number of rows and m is the number of columns in the grid) consisting of '0's (Water) and '1's(Land). Find the number of islands.  
  
**Note:** An island is either surrounded by water or boundary of grid and is formed by connecting adjacent lands horizontally or vertically or diagonally i.e., in all 8 directions.

![[Pasted image 20240605001922.png]]

### Code

#### DFS
```cpp
class Solution
{
private:
    void dfsUtil(pair<int, int> start, vector<vector<char>> &grid)
    {
        int n = grid.size();
        int m = grid[0].size();

        grid[start.first][start.second] = '0';

        for (int delrow = -1; delrow <= 1; delrow++)
        {
            for (int delcol = -1; delcol <= 1; delcol++)
            {
                int nrow = delrow + start.first;
                int ncol = delcol + start.second;

                if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < m &&
                    grid[nrow][ncol] == '1')
                    dfsUtil({nrow, ncol}, grid);
            }
        }
    }

    void dfs(int row, int col, vector<vector<char>> &grid)
    {

        pair<int, int> start = {row, col};

        dfsUtil(start, grid);
    }

public:
    // Function to find the number of islands.
    int numIslands(vector<vector<char>> &grid)
    {
        int n = grid.size();
        int m = grid[0].size();

        int count = 0;

        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < m; j++)
            {
                if (grid[i][j] == '1')
                {
                    ++count;
                    dfs(i, j, grid);
                }
            }
        }

        return count;
    }
};
```
#### BFS
```cpp
 class Solution {
private:
    void bfs(int row, int col, vector<vector<char>>& grid) {
        int n = grid.size();  
        int m = grid[0].size();  

        queue<pair<int, int>> q;   //coordinates of cells to be visited
        grid[row][col] = '0';   // Mark the starting cell as visited
        q.push({row, col});   

        // Perform BFS
        while (!q.empty()) {
            int row = q.front().first; 
            int col = q.front().second; 
            q.pop();   

            // Check all eight neighboring cells
            for (int delrow = -1; delrow <= 1; delrow++) {
                for (int delcol = -1; delcol <= 1; delcol++) {
                    int nrow = delrow + row;
                    int ncol = delcol + col; 

                    // If the neighboring cell is valid and is a land cell ('1')
                    if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < m && grid[nrow][ncol] == '1') {
                        grid[nrow][ncol] = '0';   
                        q.push({nrow, ncol}); 
                    }
                }
            }
        }
    }

public:
    // Function to find the number of islands in the grid.
    int numIslands(vector<vector<char>>& grid) {
        int n = grid.size();   // Number of rows in the grid
        int m = grid[0].size();   // Number of columns in the grid

        int count = 0;   // Counter to keep track of the number of islands

        // Iterate over each cell in the grid
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                // If the current cell is a land cell ('1')
                if (grid[i][j] == '1') {
                    ++count;
                    bfs(i, j, grid);
                }
            }
        }

        return count;   // Return the number of islands
    }
};
```