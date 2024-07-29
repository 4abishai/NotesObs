You are given an `n x n` integer matrix `grid` where each value `grid[i][j]` represents the elevation at that point `(i, j)`.

The rain starts to fall. At time `t`, the depth of the water everywhere is `t`. You can swim from a square to another 4-directionally adjacent square if and only if the elevation of both squares individually are at most `t`. You can swim infinite distances in zero time. Of course, you must stay within the boundaries of the grid during your swim.

Return _the least time until you can reach the bottom right square_ `(n - 1, n - 1)` _if you start at the top left square_ `(0, 0)`.

```python
class Solution:
    def swimInWater(self, grid: List[List[int]]) -> int:
        n = len(grid)
        m = len(grid[0])
        time = [[float('inf')] * m  for _ in range(n)]

        q = [(grid[0][0], 0, 0)]
        time[0][0] = grid[0][0]

        while q:
            t, r, c = heapq.heappop(q)

            if r == n - 1 and c == m - 1:
                return t
            
            dr = [-1, 0, 1, 0]
            dc = [0, 1, 0, -1]

            for i in range(4):
                nr = r + dr[i] 
                nc = c + dc[i]

                if 0 <= nr < n and 0 <= nc < m:
                    # Only move to the cell if the water level
                    # has risen to at least the elevation of that cell
                    newTime = max(grid[nr][nc], t)
                    # If new time is less than prev recorded time
                    if newTime < time[nr][nc]:
                        time[nr][nc] = newTime
                        heapq.heappush(q, (newTime, nr, nc))
        
        return 0
```