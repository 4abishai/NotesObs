Given two strings **X** and **Y** of lengths **m** and **n** respectively, find the length of the **smallest string** which has both, **X and Y** as its **sub-sequences**.  
**Note:** **X** and **Y** can have both uppercase and lowercase letters.

### TOP-DOWN APPROACH

```cpp
class Solution
{
    public:
    //Function to find length of shortest common supersequence of two strings.
    int shortestCommonSupersequence(string X, string Y, int m, int n)
    {
        vector<vector<int>> dp(m + 1, vector<int> (n + 1, 0));
        
        for(int i = 0; i <= m; i++) {
            for(int j = 0; j <= n; j++) {
                if (i == 0 || j == 0)
                    dp[i][j] = 0;
                else if(X[i - 1] == Y[j - 1])
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                else
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        
        // Subtract the common longest subsequence
        return m + n - dp[m][n];
    }
};
```

