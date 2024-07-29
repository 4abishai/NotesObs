You are given an array of variable pairs `equations` and an array of real numbers `values`, where `equations[i] = [Ai, Bi]` and `values[i]` represent the equation `Ai / Bi = values[i]`. Each `Ai` or `Bi` is a string that represents a single variable.

You are also given some `queries`, where `queries[j] = [Cj, Dj]` represents the `jth` query where you must find the answer for `Cj / Dj = ?`.

Return _the answers to all queries_. If a single answer cannot be determined, return `-1.0`.

**Note:** The input is always valid. You may assume that evaluating the queries will not result in division by zero and that there is no contradiction.

**Note:** The variables that do not occur in the list of equations are undefined, so the answer cannot be determined for them.

https://leetcode.com/problems/evaluate-division/description/comments/1899742

```cpp
class Solution {
private:
    double dfs(const string& cur, const string& target, double mul,
               unordered_map<string, vector<pair<string, double>>>& graph,
               unordered_set<string>& visit) {
        // Base case: if we reach the target variable
        if (cur == target) {
            return mul;
        }

        // Mark current node as visited
        visit.insert(cur);

        // Explore neighbors
        for (const auto& [neigh, wt] : graph[cur]) {
            if (visit.find(neigh) == visit.end()) { // If neighbor not visited
                double result = dfs(neigh, target, mul * wt, graph, visit);
                if (result != -1.0) {
                    return result; // Found a path to target
                }
            }
        }

        return -1.0; // No path found to target
    }

public:
    vector<double> calcEquation(vector<vector<string>>& equations, 
                                vector<double>& values, 
                                vector<vector<string>>& queries) {
        unordered_map<string, vector<pair<string, double>>> graph;

        // Build the graph
        for (int i = 0; i < equations.size(); ++i) {
            string A = equations[i][0];
            string B = equations[i][1];
            double value = values[i];
            
            graph[A].push_back({B, value});
            graph[B].push_back({A, 1.0 / value});
        }

        vector<double> results;
        for (const auto& query : queries) {
            string C = query[0];
            string D = query[1];

            if (graph.find(C) == graph.end() || graph.find(D) == graph.end()) {
                results.push_back(-1.0);
                continue;
            }

            unordered_set<string> visited;
            double result = dfs(C, D, 1.0, graph, visited);
            results.push_back(result);
        }

        return results;
    }
};
```