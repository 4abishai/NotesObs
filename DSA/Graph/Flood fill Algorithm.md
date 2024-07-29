Given a grid of size n * m (n is the number of rows and m is the number of columns in the grid) consisting of '0's (Water) and '1's(Land). Find the number of islands.  
  
**Note:** An island is either surrounded by water or boundary of grid and is formed by connecting adjacent lands horizontally or vertically or diagonally i.e., in all 8 directions.
### Code
```cpp
class Solution {
private: 
    void dfs(int r, int c,int initColor, int newColor, vector<vector<int>>& res) {
        int n = res.size();
        int m = res[0].size();
        
        res[r][c] = newColor;
        
        vector<pair<int, int>> neighbours = {
        {r - 1, c}, {r, c + 1}, {r + 1, c}, {r, c - 1}
        };
        
        for(auto neighbour : neighbours) {
            int nr = neighbour.first;
            int nc = neighbour.second;
            
            if(nr >= 0 && nr < n && nc >= 0 && nc < m
                && res[nr][nc] == initColor && res[nr][nc] != newColor)
                dfs(nr, nc, initColor, newColor, res);
        }
    }
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        vector<vector<int>> res = image; // separate result matrix
        int initColor = image[sr][sc];
        
        dfs(sr, sc, initColor, newColor, res);
        
        return res;
    }
};
```