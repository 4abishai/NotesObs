You are given an integer array of unique positive integers `nums`. Consider the following graph:

- There are `nums.length` nodes, labeled `nums[0]` to `nums[nums.length - 1]`,
- There is an undirected edge between `nums[i]` and `nums[j]` if `nums[i]` and `nums[j]` share a common factor greater than `1`.

Return _the size of the largest connected component in the graph_.

![[Pasted image 20240728011150.png]]

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
    int largestComponentSize(vector<int>& nums) {
        int maxNum = *max_element(nums.begin(), nums.end());
        
        DisjointSet ds(maxNum);

        for (int num : nums) {
            for (int i = 2; i <= sqrt(num); i++) {
                if (num % i == 0) {
                    ds.unionBySize(num, i);
                    ds.unionBySize(num, num / i);
                }
            }
        }

        unordered_map<int, int> componentSize;
        int largestComponent = 0;

        for (int num : nums) {
            int parent = ds.findUPar(num);
            componentSize[parent]++;
            largestComponent = max(largestComponent, componentSize[parent]);
        }

        return largestComponent;
    }
};
```

```python
class DisjointSet:
    def __init__(self, n):
        self.size = [1] * (n + 1)
        self.parent = list(range(n + 1))

    def find_upar(self, node):
        if node == self.parent[node]:
            return node
        self.parent[node] = self.find_upar(self.parent[node])
        return self.parent[node]

    def union_by_size(self, u, v):
        up_u = self.find_upar(u)
        up_v = self.find_upar(v)
        if up_u == up_v:
            return

        if self.size[up_u] < self.size[up_v]:
            self.parent[up_u] = up_v
            self.size[up_v] += self.size[up_u]
        else:
            self.parent[up_v] = up_u
            self.size[up_u] += self.size[up_v]

class Solution:
    def largestComponentSize(self, nums: List[int]) -> int:
        max_num = max(nums)

        ds = DisjointSet(max_num)

        for num in nums:
            for i in range(2, int(math.sqrt(num)) + 1):
                if num % i == 0:
                    ds.union_by_size(num, i)
                    ds.union_by_size(num, num // i)
        
        comp_size = {}
        largest_comp = 0

        for num in nums:
            parent = ds.find_upar(num)
            comp_size[parent] = comp_size.get(parent, 0) + 1
            largest_comp = max(largest_comp, comp_size[parent])
        
        return largest_comp
```