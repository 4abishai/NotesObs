On a 2D plane, we place `n` stones at some integer coordinate points. Each coordinate point may have at most one stone.

A stone can be removed if it shares either **the same row or the same column** as another stone that has not been removed.

Given an array `stones` of length `n` where `stones[i] = [xi, yi]` represents the location of the `ith` stone, return _the largest possible number of stones that can be removed_.

https://youtu.be/OwMNX8SPavM?si=T8MgOs5C79Ojsc05

```cpp
class DisjointSet
{
public:
    vector<int> size, parent;
    
    DisjointSet(int n)
    {
        size.resize(n + 1, 1);
        parent.resize(n + 1);
        for (int i = 0; i < n + 1; i++)
            parent[i] = i;
    }

    // Find ultimate parent
    int findUPar(int node)
    {
        if (node == parent[node])
            return node;
        else
            return parent[node] = findUPar(parent[node]);
    }

    void unionBySize(int u, int v)
    {
        int up_u = findUPar(u);
        int up_v = findUPar(v);

        if (up_u == up_v)
            return;

        // We attach the smaller tree to the taller tree
        if (size[up_u] < size[up_v])
        {
            parent[up_u] = up_v;
            size[up_v] += size[up_u];
        }

        else
        {
            parent[up_v] = up_u;
            size[up_u] += size[up_v];
        }
    }
};

class Solution {
public:
    int removeStones(vector<vector<int>>& stones) {
        // As we don't know size of matrix
        int n(0), m(0);
        for (auto stone : stones) {
            n = max(n, stone[0]);
            m = max(m, stone[1]);
        }

        DisjointSet ds(n + m + 1);

        for (auto stone : stones) {
            int row = stone[0];
            int col = stone[1] + n + 1; 
            ds.unionBySize(row, col);
        }

        // Each component have unique parent
        set<int> st;
        
        for (auto stone : stones) {
            st.insert(ds.findUPar(stone[0]));
        }

        int n_stones = stones.size(); 
        int n_comps = st.size();

        return n_stones - n_comps;
    }
};
```