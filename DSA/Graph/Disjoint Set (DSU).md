Disjoint Set data structure, also known as a Union-Find data structure is used to efficiently manage a collection of disjoint sets and perform operations like union and find on them.

#DSU 
#### Union By Rank
```cpp
#include <bits/stdc++.h>
using namespace std;

class DisjointSet
{
public:
    vector<int> rank, parent;

    DisjointSet(int n)
    {   
		// n + 1 for 0 and 1 based indexing
        rank.resize(n + 1, 0);
        parent.resize(n + 1);
        for (int i = 0; i < n + 1; i++)
            parent[i] = i;
    }

    // PATH COMPRESSION: Find ultimate parent
    int findUPar(int node)
    {
        if (node == parent[node])
            return node;
        else
            return parent[node] = findUPar(parent[node]);
    }

    void unionByRank(int u, int v)
    {
        int up_u = findUPar(u);
        int up_v = findUPar(v);

        if (up_u == up_v)
            return;

        // We attach the shorter tree to the taller tree
        if (rank[up_u] < rank[up_v])
            parent[up_u] = up_v;

        else if (rank[up_u] > rank[up_v])
            parent[up_v] = up_u;

        else // ranks are equal
        {
            parent[up_v] = up_u;
            rank[up_u]++;
        }
    }
};

int main()
{
    DisjointSet ds(7);
    ds.unionByRank(1, 2);
    ds.unionByRank(2, 3);
    ds.unionByRank(4, 5);
    ds.unionByRank(6, 7);
    ds.unionByRank(5, 6);

    if (ds.findUPar(3) == ds.findUPar(7))
        cout << "Same\n";
    else
        cout << "Not same\n";

    ds.unionByRank(3, 7);

    if (ds.findUPar(3) == ds.findUPar(7))
        cout << "Same\n";
    else
        cout << "Not same\n";
    return 0;
}
```


1. The `DisjointSet` class:
   - It uses two vectors: `rank` and `parent`.
   - `parent` stores the parent of each node.
   - `rank` stores the height of the tree rooted at each node.

2. Constructor:
   - Initializes `rank` and `parent` vectors.
   - Initially, each node is its own parent.

3. `findUPar` method:
   - Finds the ultimate parent (root) of a node.
   - Uses path compression for optimization.

4. `unionByRank` method:
   - Unites two sets by rank.
   - If ranks are different, the smaller rank tree is attached to the larger rank tree.
   - If ranks are equal, one is arbitrarily chosen as the parent and its rank is incremented.

5. `main` function:
   - Creates a DisjointSet with 7 elements.
   - Performs several union operations.
   - Checks if nodes 3 and 7 are in the same set before and after unioning them.

We attach the shorter tree to the taller tree. This doesn't increase the height of the taller tree, so the rank of remains unchanged.

The rank is only increased when the two trees have equal rank. In this case, attaching one tree to the other will increase the height of the resulting tree by 1, so we increment the rank.

##### Time Complexity

The time complexity of the `findUPar` and `unionByRank` operations in the Disjoint Set data structure can be analyzed using the concept of the inverse Ackermann function, typically denoted as \( \alpha \). 

### Key Points:

1. **Path Compression (`findUPar`):**
   - Path compression is an optimization that flattens the structure of the tree whenever `findUPar` is called.
   - This ensures that all nodes directly point to the root, reducing the depth of the tree.
   - The amortized time complexity of `findUPar` with path compression is \( O(\alpha(n)) \).

2. **Union by Rank (`unionByRank`):**
   - Union by rank is an optimization that ensures the smaller tree (in terms of rank or height) is always attached under the root of the larger tree.
   - This keeps the trees balanced, and prevents the height from growing too large.

### Amortized Analysis of `unionByRank`:

- When analyzing the `unionByRank` operation, it is essential to consider the cost of finding the ultimate parents of the nodes being united, which involves the `findUPar` operation.

- The rank of a node only increases when two trees of the same rank are united. The rank of a node can increase at most \( O(\log n) \) times. Thus, the height of the tree is logarithmic in the worst case.

### Combined Operations (Union-Find with Path Compression and Union by Rank):

- The amortized time complexity of a sequence of \( m \) operations, which include both `findUPar` and `unionByRank`, on a disjoint-set forest with \( n \) elements is \( O(m \cdot \alpha(n)) \).

- This is because:
  - The `findUPar` operation with path compression has an amortized time complexity of \( O(\alpha(n)) \).
  - The `unionByRank` operation, although it is \( O(1) \) in its basic form, must account for the `findUPar` calls it makes to determine the roots of the trees being united. Since these `findUPar` calls have an amortized complexity of \( O(\alpha(n)) \), the `unionByRank` operation effectively inherits this complexity in the amortized analysis.
### Inverse Ackermann Function:
- The inverse Ackermann function \( \alpha(n) \) grows extremely slowly, more slowly than the logarithm.
- For all practical purposes, (alpha(n)) is a very small constant (no more than 4 for any conceivable value of (n)).

### Conclusion:
The amortized time complexity of operations in the Disjoint Set class using path compression and union by rank is indeed \( O(\alpha(n)) \), where \( \alpha \) is the inverse Ackermann function. For all practical purposes, this can be considered a very efficient constant-time operation.
#### Union By Size

```cpp
#include <bits/stdc++.h>
using namespace std;
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
```

```cpp
int main()
{
    DisjointSet ds(7);
    ds.unionBySize(1, 2);
    ds.unionBySize(2, 3);
    ds.unionBySize(4, 5);
    ds.unionBySize(6, 7);
    ds.unionBySize(5, 6);

    if (ds.findUPar(3) == ds.findUPar(7))
        cout << "Same\n";
    else
        cout << "Not same\n";

    ds.unionBySize(3, 7);

    if (ds.findUPar(3) == ds.findUPar(7))
        cout << "Same\n";
    else
        cout << "Not same\n";
    return 0;
}
```
##### Python
```python
class DisjointSet:
    def __init__(self, n):
        self.size = [1] * (n + 1)
        self.parent = list(range(n + 1))

    def findUpar(self, node):
        if node == self.parent[node]:
            return node
        self.parent[node] = self.findUpar(self.parent[node])
        return self.parent[node]

    def unionBySize(self, u, v):
        upU = self.findUpar(u)
        upV = self.findUpar(v)
        if upU == upV:
            return

        if self.size[upU] < self.size[upV]:
            self.parent[upU] = upV
            self.size[upV] += self.size[upU]
        else:
            self.parent[upV] = upU
            self.size[upU] += self.size[upV]
```

The main difference between the two implementations is the strategy used for merging sets: union by rank vs union by size. Let's break down the key differences:

1. Data Structure:
   - Union by Rank: Uses `vector<int> rank`
   - Union by Size: Uses `vector<int> size`

2. Initialization:
   - Union by Rank: Initializes `rank` to 0 for all nodes
   - Union by Size: Initializes `size` to 1 for all nodes

3. Union Operation:
   - Union by Rank (`unionByRank`):
     - Compares ranks of the roots
     - Attaches the tree with lower rank to the tree with higher rank
     - If ranks are equal, arbitrarily chooses one as parent and increments its rank
   
   - Union by Size (`unionBySize`):
     - Compares sizes of the trees
     - Always attaches the smaller tree to the larger tree
     - Updates the size of the new root by adding the size of the attached tree

4. Performance Implications:
   - Union by Rank generally ensures a more balanced tree structure
   - Union by Size can sometimes lead to slightly flatter structures in practice

5. Space Complexity:
   - Both use O(n) extra space, but union by size might use slightly more memory as it stores actual set sizes rather than ranks

6. Time Complexity:
   - Both achieve nearly identical amortized time complexity for operations

In practice, both methods are very efficient and the choice between them often comes down to personal preference or specific use case requirements. Union by size might be more intuitive in some scenarios where you need to keep track of the actual set sizes.