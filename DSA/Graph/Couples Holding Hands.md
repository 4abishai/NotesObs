There are `n` couples sitting in `2n` seats arranged in a row and want to hold hands.

The people and seats are represented by an integer array `row` where `row[i]` is the ID of the person sitting in the `ith` seat. The couples are numbered in order, the first couple being `(0, 1)`, the second couple being `(2, 3)`, and so on with the last couple being `(2n - 2, 2n - 1)`.

Return _the minimum number of swaps so that every couple is sitting side by side_. A swap consists of choosing any two people, then they stand up and switch seats.

```python
O(n * n)

class Solution:
    def minSwapsCouples(self, row: List[int]) -> int:
        n = len(row) // 2
        nSwaps = 0

        def swap(i, j):
            row[i], row[j] = row[j], row[i]
        
        def findPartner(per) -> int:
            if per % 2 == 0:
                return per + 1
            else:
                return per - 1
            
        for i in range(0, 2*n, 2):
            if row[i] == findCouple(row[i + 1]):
                continue
            
            for j in range(i + 2, 2*n):
                if row[j] == findPartner(row[i]):
                    swap(j, i + 1)
                    nSwaps += 1
                    break
        
        return nSwaps
```

Given:
```
row = [1, 3, 4, 0, 2, 5, 6, 7, 8, 10, 9, 11]
```

Here are the 6 couples:
- Couple 0: (0, 1)
- Couple 1: (2, 3)
- Couple 2: (4, 5)
- Couple 3: (6, 7)
- Couple 4: (8, 9)
- Couple 5: (10, 11)

Let's go through the algorithm:

1. Initialize:
   - We have 6 couples (n = 12 / 2 = 6)
   - Create a UnionFind structure with 6 elements (0, 1, 2, 3, 4, 5 representing the six couples)

2. Process the row:

   a. [1, 3]: Union(0, 1)
   b. [4, 0]: Union(2, 0)
   c. [2, 5]: Union(1, 2)
   d. [6, 7]: Union(3, 3) (already correct, no actual union)
   e. [8, 10]: Union(4, 5)
   f. [9, 11]: Union(4, 5) (already unioned in previous step)

3. Final UnionFind structure:
   ```
   0 -- 1 -- 2    3    4 -- 5
   ```

4. Count components:
   - We have 3 components (0-1-2 connected, 3 separate, and 4-5 connected)

5. Calculate result:
   - Swaps needed = number of couples - number of components
   - Swaps needed = 6 - 3 = 3

Let's break down what happened:

- Couples 0, 1, and 2 formed one cycle: (1,3) -> (2,5) -> (4,0)
- Couple 3 (6,7) is already correctly seated
- Couples 4 and 5 formed another cycle: (8,10) -> (9,11)

To fix this arrangement, we need 3 swaps. For example:
1. Swap 3 and 4: [1, 4, 3, 0, 2, 5, 6, 7, 8, 10, 9, 11]
2. Swap 0 and 2: [1, 4, 0, 3, 2, 5, 6, 7, 8, 10, 9, 11]
3. Swap 10 and 9: [1, 4, 0, 3, 2, 5, 6, 7, 8, 9, 10, 11]

Final result: [1, 0, 4, 5, 2, 3, 6, 7, 8, 9, 10, 11]

- Why (n - c) Works:
    - If all couples are correctly seated, c = n, so n - c = 0 swaps.
    - If all couples form one big cycle, c = 1, so n - c = n - 1 swaps.
    - For any arrangement in between, n - c gives the correct number of swaps.

```python
O(n)

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

class Solution:
    def minSwapsCouples(self, row: List[int]) -> int:
        n = len(row) // 2

        # DisjointSest of couples idx
        ds = DisjointSet(n)

        # Union couple idx seated next to each other
        # If couples idx are different union is performed
        for i in range(0, 2*n, 2):
            A = row[i] // 2
            B = row[i + 1] // 2

            ds.unionBySize(A, B)
        
        ConnectedCouples = 0
        
        # for i in range(n):
        #     if ds.findUpar(i) == i:
        #         ConnectedCouples += 1

        UniquePars = set(ds.findUpar(i) for i in range(n))
        ConnectedCouples = len(UniquePars)
        
        # swaps = Total couples - Connected couples 
        return n - ConnectedCouples
```

- Initialization of `DisjointSet`: $$O(n)$$
- Union operations: $$O(n \cdot \alpha(n))$$
- Finding unique parents: $$O(n \cdot \alpha(n))$$
- The total time complexity of the `minSwapsCouples` function is:

$$ O(n) + O(n \cdot \alpha(n)) + O(n \cdot \alpha(n)) = O(n \cdot \alpha(n)) $$
Given that $$ \alpha(n) $$ is a very slowly growing function, for all practical purposes, the complexity can be considered nearly linear, i.e., $$ O(n) $$