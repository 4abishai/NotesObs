You are a hiker preparing for an upcoming hike. You are given **`heights[][]`**, a 2D array of size `**rows x columns**`, where **`heights[row][col]`** represents the height of cell `**(row, col)**`. You are situated in the top-left cell, `**(0, 0)**`, and you hope to travel to the bottom-right cell, `**(rows-1, columns-1)**` (i.e., **0-indexed**). You can move **up**, **down**, **left**, or **right**, and you wish to find the route with minimum **effort**.

**Note:** A route's **effort** is the **maximum absolute difference** in heights between two consecutive cells of the route.

![[Pasted image 20240728200231.png]]
![[Pasted image 20240728200239.png]]
##### CPP
```cpp
class Solution {
public:
    int MinimumEffort(int rows, int columns, vector<vector<int>> &heights) {
        int n = heights.size();
        int m = heights[0].size();
        
        // Initialize a 2D vector 'diff' to store the minimum effort needed to reach each cell
        vector<vector<int>> diff(n, vector<int>(m, INT_MAX));
        
        // Min heap to store the cells along with the effort required to reach them
        priority_queue<pair<int, 
        pair<int, int>>, 
        vector<pair<int, pair<int, int>>>, 
        greater<pair<int, pair<int, int>>>> q;
        
        // Start from the top-left corner with 0 effort
        // difference, r, c
        q.push({0, {0, 0}});
        diff[0][0] = 0;
        
        while(!q.empty()) {
            auto e = q.top();
            q.pop();
            int df = e.first;
            int r = e.second.first;
            int c = e.second.second;
            
            // If we have reached the bottom-right corner, return the effort
            if (r == n - 1 && c == m - 1)
                return df;
            
            // Directions for moving up, right, down, left
            int dr[] = {-1, 0, 1, 0};
            int dc[] = {0, 1, 0, -1};
            
            // Explore all 4 possible directions
            for (int i = 0; i < 4; i++) {
                int nr = r + dr[i];
                int nc = c + dc[i];
                
                // Check if the new cell is within the grid bounds
                if (nr >= 0 && nr < n && nc >= 0 && nc < m) {
                    // Calculate the new effort required to reach the new cell
                    // Effort (for a path): is max of all difference
                    int newEffort = max(abs(heights[r][c] - heights[nr][nc]), df);
                    // If the new effort is less than the current effort to reach the new cell, update it
                    if (newEffort < diff[nr][nc]) {
                        diff[nr][nc] = newEffort;
                        q.push({newEffort, {nr, nc}});
                    }
                }
            }
        }
        
        // If the bottom-right corner is not reachable, return 0
        return 0;
    }
};
```

##### Python
```python
class Solution:
    def minimumEffortPath(self, heights: List[List[int]]) -> int:
        n = len(heights)
        m = len(heights[0])
        diff = [[float('inf')] * m for _ in range(n)] # Minimum effort needed to reach each cell
        
        q = [(0, 0, 0)]  # (effort, row, column) -- Min heap
        diff[0][0] = 0
        
        while q:
            df, r, c = heapq.heappop(q)
            # bottom-right corner, return the effort
            if r == n - 1 and c == m - 1: 
                return df
            
            directions = [(-1, 0), (0, 1), (1, 0), (0, -1)]
            
            for dr, dc in directions:
                nr, nc = r + dr, c + dc
                
                if 0 <= nr < n and 0 <= nc < m:
                    # Calculate the new effort required to reach the new cell
                    # Effort (for a path): is max of all difference
                    new_effort = max(abs(heights[r][c] - heights[nr][nc]), df)
                    # If the new effort is less than the current effort to reach the new cell, update it
                    if new_effort < diff[nr][nc]:
                        diff[nr][nc] = new_effort
                        heapq.heappush(q, (new_effort, nr, nc))
        
        # If the bottom-right corner is not reachable, return 0
        return 0
```