Given a **n * m** matrix **grid** where each element can either be **0** or **1**. You need to find the shortest distance between a given source cell to a destination cell. The path can only be created out of a cell if its value is 1. 

If the path is not possible between source cell and destination cell, then return **-1**.

**Note :** You can move into an adjacent cell if that adjacent cell is filled with element 1. Two cells are adjacent if they share a side. In other words, you can move in one of the four directions, Up, Down, Left and Right. The source and destination cell are based on the zero based indexing. The destination cell should be 1.

```cpp
class Solution {
  public:
    int shortestPath(vector<vector<int>> &grid, pair<int, int> src,
                     pair<int, int> dst) {
        // Edgecase
        if (src == dst) return 0;                 
        
        int n = grid.size();
        int m = grid[0].size();
        
        vector<vector<int>> dist(n, vector<int> (m, INT_MAX));
        queue<pair<int, pair<int, int>>> q;
        
        q.push({0, {src.first, src.second}});
        dist[src.first][src.second] = 0;
        
        while(!q.empty()) {
            auto e = q.front();
            q.pop();
            int dis = e.first;
            int r = e.second.first;
            int c = e.second.second;
            
            int dr[] = {1, 0, -1, 0};
            int dc[] = {0, 1, 0, -1};
            
            int nr, nc;
            
            for (int i = 0; i < 4; i++) {
                nr = r + dr[i];
                nc = c + dc[i];
                
                if (nr >= 0 && nr < n && nc >= 0 && nc < m && grid[nr][nc] && dis + 1 < dist[nr][nc]) {
                    if (nr == dst.first && nc == dst.second)
                        return dis + 1;
                    dist[nr][nc] = dis + 1;
                    q.push({dis + 1, {nr, nc}});
                }
            }
            
        }
        
        return -1;
    }
};

```