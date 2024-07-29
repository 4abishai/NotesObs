Given a set of **N** items, each with a weight and a value, represented by the array **w** and **val** respectively. Also, a knapsack with weight limit **W**.  
The task is to fill the knapsack in such a way that we can get the maximum profit. Return the maximum profit.  
**Note:** Each item can be taken any number of times.

```cpp
class Solution{
public:
    int knapSack(int N, int W, int val[], int wt[])
    {
        vector<vector<int>> dp(N + 1, vector<int> (W + 1, 0));
        
        for (int i = 1; i <= N; i++) {
            for (int j = 1; j <= W; j++) {
                if (wt[i - 1] <= j)
                    dp[i][j] = max(
			            val[i - 1] + dp[i][j - wt[i - 1]], // item taken no limit
	                    dp[i - 1][j]); // item rejected
                else
                    dp[i][j] = dp[i - 1][j];
            }
        }
        
        return dp[N][W];
    }
};
```