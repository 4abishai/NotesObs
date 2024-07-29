Given a grid of dimension **nxm** where each cell in the grid can have values 0, 1 or 2 which has the following meaning:  
**0** : Empty cell  
**1** : Cells have fresh oranges  
**2** : Cells have rotten oranges

We have to determine what is the earliest time after which all the oranges are rotten. A rotten orange at index [i,j] can rot other fresh orange at indexes [i-1,j], [i+1,j], [i,j-1], [i,j+1] (**up**, **down**, **left** and **right**) in unit time.
### Code
```cpp
class Solution {
public:
    //Function to find minimum time required to rot all oranges.
    int orangesRotting(vector<vector<int>>& grid) {
        int n = grid.size();                  // Number of rows
        int m = grid[0].size();               // Number of columns

        // (row, col), time
        queue<pair<pair<int, int>, int>> q;   

        vector<vector<int>> res = grid; // Temporary grid to simulate rotting

        int freshCount = 0;
        // Enqueue all rotten oranges
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 2)
                    q.push({{i, j}, 0});      // Push rotten oranges with time 0
                else if (grid[i][j] == 1)
                    ++freshCount;             // Count fresh oranges
            }
        }

        int maxTime = 0;

        while (!q.empty()) {
	        // row and col of rotten orange
            int r = q.front().first.first;    
            int c = q.front().first.second;   
            int t = q.front().second;      // Time when  orange became rotten
            maxTime = t;     // Update the maximum time

            q.pop();                 

            vector<pair<int, int>> neighbours = {
                {r - 1, c}, {r, c + 1}, {r + 1, c}, {r, c - 1}
            };                                

            for (auto neighbour : neighbours) {
                int nrow = neighbour.first;
                int ncol = neighbour.second;

                // Simulate orange rotting in res
                if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < m
                    && res[nrow][ncol] != 2 && res[nrow][ncol] == 1) {
                    res[nrow][ncol] = 2;      // Mark the fresh orange as rotten
                    q.push({{nrow, ncol}, t + 1}); // Push the new rotten orange 
                    --freshCount;             // Decrement the fresh orange count
                }
            }
        }

        // Return -1 if there is non-rotten orange left
        if (freshCount > 0)
            return -1;

        return maxTime;
    }
};
```