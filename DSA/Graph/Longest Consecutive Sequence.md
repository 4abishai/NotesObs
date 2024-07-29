Given an unsorted array of integers `nums`, return _the length of the longest consecutive elements sequence._

You must write an algorithm that runs in `O(n)` time.

```cpp
class DisjointSet {
public:
    std::vector<int> size, parent;

    DisjointSet(int n) {
        size.resize(n, 1);
        parent.resize(n);
        for (int i = 0; i < n; ++i)
            parent[i] = i;
    }

    int findUPar(int node) {
        if (node == parent[node])
            return node;
        return parent[node] = findUPar(parent[node]);
    }

    void unionBySize(int u, int v) {
        int up_u = findUPar(u);
        int up_v = findUPar(v);

        if (up_u == up_v)
            return;

        if (size[up_u] < size[up_v]) {
            parent[up_u] = up_v;
            size[up_v] += size[up_u];
        } else {
            parent[up_v] = up_u;
            size[up_u] += size[up_v];
        }
    }

    int maxSize() {
        return *std::max_element(size.begin(), size.end());
    }
};

class Solution {
public:
    int longestConsecutive(std::vector<int>& nums) {
        if (nums.empty()) return 0;

        unordered_set<int> numSet(nums.begin(), nums.end());

        DisjointSet ds(numSet.size());
        unordered_map<int, int> valueToIndex;

        int index = 0;
        for (int num : numSet) {
            valueToIndex[num] = index++;
        }

        for (int num : numSet) {
            if (numSet.find(num + 1) != numSet.end()) {
                ds.unionBySize(valueToIndex[num], valueToIndex[num + 1]);
            }
        }

        return ds.maxSize();
    }
};
```