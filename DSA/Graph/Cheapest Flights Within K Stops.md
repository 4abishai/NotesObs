There are `n` cities connected by some number of flights. You are given an array `flights` where `flights[i] = [fromi, toi, pricei]` indicates that there is a flight from city `fromi` to city `toi` with cost `pricei`.

You are also given three integers `src`, `dst`, and `k`, return _**the cheapest price** from_ `src` _to_ `dst` _with at most_ `k` _stops._ If there is no such route, return `-1`.

```cpp
class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        vector<pair<int, int>> adj[n];
        for (auto& e : flights) {
            adj[e[0]].push_back({e[1], e[2]});
        }
        
        queue<tuple<int, int, int>> q;
        vector<int> dist(n, INT_MAX);
        
        q.push({0, src, 0});
        dist[src] = 0;
        
        while(!q.empty()) {
            auto [steps, node, cost] = q.front();
            q.pop();
            
            if (steps > K) continue;
            
            for (auto& [adjNode, wt] : adj[node]) {
                if (cost + wt < dist[adjNode]) {
                    dist[adjNode] = cost + wt;
                    q.push({steps + 1, adjNode, dist[adjNode]});
                }
            }
        }
        
        return dist[dst] == INT_MAX ? -1 : dist[dst];
    }
};
```