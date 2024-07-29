There are `n` cities numbered from `0` to `n-1`. Given the array `edges` where `edges[i] = [fromi, toi, weighti]` represents a bidirectional and weighted edge between cities `fromi` and `toi`, and given the integer `distanceThreshold`.

Return the city with the smallest number of cities that are reachable through some path and whose distance is **at most** `distanceThreshold`, If there are multiple such cities, return the city with the greatest number.

Notice that the distance of a path connecting cities _**i**_ and _**j**_ is equal to the sum of the edges' weights along that path.

```cpp
class Solution {
public:
    int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) {
        vector<vector<int>> dist(n, vector<int>(n, 1e9));
        
        // Initialize distances
        for (int i = 0; i < n; i++) {
            dist[i][i] = 0;
        }
        
        for (auto edge : edges) {
            dist[edge[0]][edge[1]] = edge[2];
            dist[edge[1]][edge[0]] = edge[2];
        }

        // Floyd-Warshall algorithm

        for (int k = 0; k < n; k++) {
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
                }
            }
        }

        int minCities(INT_MAX);
        int res(-1);

        for (int i = 0; i < n; i++) {
            int reachableCities(0);

            for (int j = 0; j < n; j++)
                if (dist[i][j] <= distanceThreshold)
                    reachableCities++;
            
            if (reachableCities <= minCities) {
                minCities = reachableCities;
                res = i;
            }
        }

        return res;
    }
};
```