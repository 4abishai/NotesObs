You are given a **n,m** which means the row and column of the 2D matrix and an array of  size k denoting the number of operations. Matrix elements is 0 if there is water or 1 if there is land. Originally, the 2D matrix is all 0 which means there is no land in the matrix. The array has k operator(s) and each operator has two integer A[i][0], A[i][1] means that you can change the cell matrix[A[i][0]][A[i][1]] from sea to island. Return how many island are there in the matrix after each operation.You need to return an array of size **k**.  
**Note :** An island means group of 1s such that they share a common side.

![[Pasted image 20240716123413.png]]

```cpp
class DisjointSet {
public:
    vector<int> size, parent;
 
    DisjointSet(int n) {
        size.resize(n + 1, 1);
        parent.resize(n + 1);
        for (int i = 0; i < n + 1; i++)
            parent[i] = i;
    }

    // Find ultimate parent
    int findUPar(int node) {
        if (node == parent[node])
            return node;
        else
            return parent[node] = findUPar(parent[node]);
    }

    void unionBySize(int u, int v) {
        int up_u = findUPar(u);
        int up_v = findUPar(v);

        if (up_u == up_v)
            return;

        // We attach the smaller tree to the taller tree
        if (size[up_u] < size[up_v]) {
            parent[up_u] = up_v;
            size[up_v] += size[up_u];
        } else {
            parent[up_v] = up_u;
            size[up_u] += size[up_v];
        }
    }
};

class Solution {
  public:
  bool isValid(int adjr, int adjc, int n, int m) {
      return adjr >= 0 && adjr < n && adjc >= 0 && adjc < m;
  }
    vector<int> numOfIslands(int n, int m, vector<vector<int>> &operators) {
        DisjointSet ds(n * m);
        
        vector<vector<int>> visit(n, vector<int> (m, 0));
        
        vector<int> ans;
        
        int cnt(0); 
        
        for (auto e : operators) {
            int r = e[0];
            int c = e[1];
            
            if (visit[r][c]) {
                ans.push_back(cnt);
                continue;
            }
            
            // If not visited node
            visit[r][c]++;
            cnt++;
            
            int dr[] = {-1, 0, 1, 0};
            int dc[] = {0, 1, 0, -1};
            
            for (int i = 0; i < 4; i++) {
                int adjr = r + dr[i];
                int adjc = c + dc[i];
                
                if (isValid(adjr, adjc, n, m) && visit[adjr][adjc]) {
                    int node = r * m + c;
                    int adjNode = adjr * m + adjc;
                    
                    if (ds.findUPar(node) != ds.findUPar(adjNode)) {
                        cnt--;
                        ds.unionBySize(node, adjNode);
                    }
                }
            }
            
            ans.push_back(cnt);
        }
        
        return ans;
    }
};
```