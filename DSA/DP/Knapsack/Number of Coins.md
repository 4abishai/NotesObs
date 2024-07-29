Given a value **V** and array **coins[]** of size **M**, the task is to make the change for **V** cents, given that you have an infinite supply of each of coins{coins1, coins2, ..., coinsm} valued coins. Find the minimum number of coins to make the change. If not possible to make change then return -1.

```cpp
class Solution{
public:
    int minCoins(vector<int> &coins, int M, int V) 
    { 
        vector<vector<int>> dp(M + 1, vector<int> (V + 1));
        
        // Base case when sum is 0
        for(int i = 0; i <= M; i++)
            dp[i][0] = 0;
        
        // Base case when no coin is available
        for(int j = 1; j <= V; j++)
            dp[0][j] = INT_MAX - 1;
       
        for(int i = 1; i <= M; i++) {
            for(int j = 1; j <= V; j++) {
                if(coins[i - 1] <= j)
                    dp[i][j] = min(1 + dp[i][j - coins[i - 1]], dp[i - 1][j]);
                else
                    dp[i][j] = dp[i - 1][j];
            }
        }
        
        return (dp[M][V] == INT_MAX - 1) ? -1 : dp[M][V];
    } 
};
```

Consider the following example:

- `coins = [1, 2, 5]` (denominations of the available coins)
- `M = 3` (number of different coin types)
- `V = 11` (the value for which we need to find the minimum number of coins)

We'll fill in the DP table step-by-step.

### Step 1: Initialize the DP Table

First, we initialize a DP table `dp` of size `(M + 1) x (V + 1)`:
- `dp[i][0] = 0` for all `i` (because no coins are needed to make the value 0).
- `dp[0][j] = INT_MAX - 1` for all `j > 0` (because it's impossible to form any positive value with 0 types of coins).

Here's the initialized DP table:

```
   j  0  1  2  3  4  5  6  7  8  9 10 11
i
0     0  INF INF INF INF INF INF INF INF INF INF
1     0
2     0
3     0
```

For convenience, we'll use `INF` to represent `INT_MAX - 1`.

### Step 2: Fill the DP Table

Now we fill the table using the given coins:

#### When `i = 1` (using only the first coin type, 1):

- For each value `j` from 1 to 11:
  - If `coins[0] <= j` (i.e., 1 <= j), then `dp[1][j] = min(1 + dp[1][j - 1], dp[0][j])`
  - Otherwise, `dp[1][j] = dp[0][j]`

```
   j  0  1  2  3  4  5  6  7  8  9 10 11
i
0     0  INF INF INF INF INF INF INF INF INF INF
1     0   1   2   3   4   5   6   7   8   9  10  11
2     0
3     0
```

#### When `i = 2` (using the first two coin types, 1 and 2):

- For each value `j` from 1 to 11:
  - If `coins[1] <= j` (i.e., 2 <= j), then `dp[2][j] = min(1 + dp[2][j - 2], dp[1][j])`
  - Otherwise, `dp[2][j] = dp[1][j]`

```
   j  0  1  2  3  4  5  6  7  8  9 10 11
i
0     0  INF INF INF INF INF INF INF INF INF INF
1     0   1   2   3   4   5   6   7   8   9  10  11
2     0   1   1   2   2   3   3   4   4   5   5   6
3     0
```

#### When `i = 3` (using all three coin types, 1, 2, and 5):

- For each value `j` from 1 to 11:
  - If `coins[2] <= j` (i.e., 5 <= j), then `dp[3][j] = min(1 + dp[3][j - 5], dp[2][j])`
  - Otherwise, `dp[3][j] = dp[2][j]`

```
   j  0  1  2  3  4  5  6  7  8  9 10 11
i
0     0  INF INF INF INF INF INF INF INF INF INF
1     0   1   2   3   4   5   6   7   8   9  10  11
2     0   1   1   2   2   3   3   4   4   5   5   6
3     0   1   1   2   2   1   2   2   3   3   2   3
```

### Step 3: Retrieve the Result

The minimum number of coins needed to make the value 11 using the given coin types is `dp[3][11]`.

In this case, `dp[3][11] = 3`, so the function will return 3.

### Summary

Here's the final DP table with intermediate values filled in:

```
   j  0  1  2  3  4  5  6  7  8  9 10 11
i
0     0  INF INF INF INF INF INF INF INF INF INF
1     0   1   2   3   4   5   6   7   8   9  10  11
2     0   1   1   2   2   3   3   4   4   5   5   6
3     0   1   1   2   2   1   2   2   3   3   2   3
```

So, the minimum number of coins needed to make the value 11 is 3.