In this problem, a rooted tree is a **directed** graph such that, there is exactly one node (the root) for which all other nodes are descendants of this node, plus every node has exactly one parent, except for the root node which has no parents.

The given input is a directed graph that started as a rooted tree with `n` nodes (with distinct values from `1` to `n`), with one additional directed edge added. The added edge has two different vertices chosen from `1` to `n`, and was not an edge that already existed.

The resulting graph is given as a 2D-array of `edges`. Each element of `edges` is a pair `[ui, vi]` that represents a **directed** edge connecting nodes `ui` and `vi`, where `ui` is a parent of child `vi`.

Return _an edge that can be removed so that the resulting graph is a rooted tree of_ `n` _nodes_. If there are multiple answers, return the answer that occurs last in the given 2D-array.

```sql
There are two cases for the tree structure to be invalid.
1) A node having two parents;
   including corner case: e.g. [[4,2],[1,5],[5,2],[5,3],[2,4]]
2) A circle exists
```

If we can remove exactly 1 edge to achieve the tree structure, a single node can have at most two parents. So my solution works in two steps.

```sql
1) Check whether there is a node having two parents. 
    If so, store them as first and second edge, and set the second edge invalid. 
2) Perform normal union find. 
    If the tree is now valid 
           simply return second edge
    else if candidates not existing 
           we find a circle, return current edge; 
    else 
           remove first edge instead of second edge.
```

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
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        vector<int> parent(n + 1, 0), frstEdge, secEdge;

        for (auto& edge : edges) {
            if (parent[edge[1]] == 0)
                parent[edge[1]] = edge[0];
            // If a second edge is found
            else {
                secEdge = edge;
                frstEdge = {parent[edge[1]], edge[1]};

                // Make second edge invalid
                edge[1] = 0;
            }
        }

        DisjointSet ds(n);

        // Check if the graph becomes a valid tree structure without invalid edge
        for (auto edge : edges) {
            // Skip the invalid edge
            if (edge[1] == 0) continue;

            int u = edge[0];
            int v = edge[1];
            // If the new edge creates a cycle
            if (ds.findUPar(u) == ds.findUPar(v))
                // If there's a node with two parents, and we also found a cycle, 
                // then the first edge giving the node two parents must be the one
                // causing the issue.
                return frstEdge.empty() ? edge : frstEdge;
            ds.unionBySize(u, v);
        }

        return secEdge;
    }
};
```
