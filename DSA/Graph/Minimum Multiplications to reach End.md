Given **start**, **end** and an array **arr** of **n** numbers. At each step, **start** is multiplied with any number in the array and then mod operation with **100000** is done to get the new start.

Your task is to find the minimum steps in which **end** can be achieved starting from **start**. If it is not possible to reach **end**, then return **-1**.

```cpp
// O (length of array * maximum number of nodes that can be processed)
// O (length of array * 100000)

class Solution {
  public:
    int minimumMultiplications(vector<int>& arr, int start, int end) {
        if (start == end) return 0;
        queue<pair<int, int>> q;
        vector<int> dist(100000, INT_MAX);
        
        dist[start] = 0;
        q.push({0, start});
        
        int mod = 100000;
        
        while(!q.empty()) {
            int steps = q.front().first;
            int node = q.front().second;
            q.pop();
            
            for (auto e : arr) {
                int num = (e * node) % mod;
                
                if (steps + 1 < dist[num]) {
                    dist[num] = steps + 1;
                    if (num == end) return dist[num];
                    q.push({dist[num], num});
                }
            }
            
        }
        
        return -1;
    }
};
```