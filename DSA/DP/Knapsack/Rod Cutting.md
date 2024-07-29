Given a rod of length N inches and an array of prices, price[]. price[i] denotes the value of a piece of length i. Determine the maximum value obtainable by cutting up the rod and selling the pieces.

**Note:** Consider 1-based indexing.

```cpp
class Solution{
  public:
    int cutRod(int price[], int n) {
        
        vector<vector<int>> dp (n + 1, vector<int> (n + 1, 0));
        
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                // As price[i-1] represents the price for a rod of length i
                if (i <= j) 
                    dp[i][j] = max(
	                    price[i - 1] + dp[i][j - i], 
	                    dp[i - 1][j]);
                else
                    dp[i][j] = dp[i - 1][j];
            }
        }
        
        return dp[n][n];
    }
};
```