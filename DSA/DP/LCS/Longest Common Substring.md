Given two strings. The task is to find the length of the longest common substring.
```cpp
class Solution{
    public:
    
    int longestCommonSubstr (string S1, string S2, int n, int m)
    {
        // Create a dp table with dimensions (n+1) x (m+1) initialized to 0
        vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
        
        // Fill the dp table using bottom-up approach
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= m; ++j) {
                if (S1[i - 1] == S2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = 0;
                }
            }
        }
        
        int maxim = INT_MIN;
        
        for(int i = 0; i < n+1; i++){
            for(int j = 0; j < m+1; j++)
                maxim = max(maxim, dp[i][j]);
        }
    
    return maxim;
    }
};
```