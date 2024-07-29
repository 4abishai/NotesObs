
You are given weights and values of **N** items, put these items in a knapsack of capacity **W** to get the maximum total value in the knapsack. Note that we have only **one quantity of each item**.  
In other words, given two integer arrays **val[0..N-1]** and **wt[0..N-1]** which represent values and weights associated with **N** items respectively. Also given an integer W which represents knapsack capacity, find out the maximum value subset of **val[]** such that sum of the weights of this subset is smaller than or equal to **W.** You cannot break an item, **either pick the complete item or dont pick it (0-1 property)**.

### TOP DOWN APPROACH
#### Recursive (not optimized -- will take more time) 

```cpp
class Solution
{
    public:
    //Function to return max value that can be put in knapsack of capacity W.
    int knapSack(int W, int wt[], int val[], int n) 
    {
        if(n == 0 || W == 0)
            return 0;
            
        if(wt[n - 1] <= W)
            return max(
                val[n - 1] + knapSack(W - wt[n - 1], wt, val, n - 1),
                knapSack(W, wt, val, n - 1));
            
        else
            return knapSack(W, wt, val, n - 1);
    }
};
```

#### Memoization
**Constraints:**  
1 ≤ N ≤ 1000  
1 ≤ W ≤ 1000  
1 ≤ wt[i] ≤ 1000  
1 ≤ v[i] ≤ 1000
```cpp
class Solution
{
private:
    static vector<vector<int>> dp;

public:
    //Function to return max value that can be put in knapsack of capacity W.
    int knapSack(int W, int wt[], int val[], int n)
    {
        if (n == 0 || W == 0)
            return 0;

        if (dp[n][W] != -1)
            return dp[n][W];

        if (wt[n - 1] <= W)
            return dp[n][W] = max(
                       val[n - 1] + knapSack(W - wt[n - 1], wt, val, n - 1),
                       knapSack(W, wt, val, n - 1)
                   );

        else
            return dp[n][W] = knapSack(W, wt, val, n - 1);
    }
};
```


### BOTTOM UP APPROACH
### Tabulation

```cpp
class Solution
{
    public:
    //Function to return max value that can be put in knapsack of capacity W.
    int knapSack(int W, int wt[], int val[], int n) 
    {
        // Create a DP array initialized to 0
        vector<vector<int>> dp(n + 1, vector<int>(W + 1, 0));

        // Build the dp array in a bottom-up manner
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= W; ++j) {
                if (wt[i - 1] <= j)
                    dp[i][j] = max(
	                    val[i - 1] + dp[i - 1][j - wt[i - 1]], 
	                    dp[i - 1][j]);
                else
                    dp[i][j] = dp[i - 1][j];
            }
        }

        // Return the value in the last cell which is the result
        return dp[n][W];
    }
};
```