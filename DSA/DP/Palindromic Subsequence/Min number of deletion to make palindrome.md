Given a string 'str' of size ‘n’. The task is to remove or delete the minimum number of characters from the string so that the resultant string is a palindrome. Find the minimum number of characters we need to remove.  
**Note:** The order of characters should be maintained.

```cpp
int minDeletions(string str, int n) { 
        string revStr = str;
        
        reverse(revStr.begin(), revStr.end());
        
        vector<vector<int>> dp(n + 1, vector<int> (n + 1, 0));
        
        for(int i = 0; i <= n; i++) {
            for(int j = 0; j <= n; j++) {
                if (i == 0 || j == 0)
                    dp[i][j] = 0;
                else if(str[i - 1] == revStr[j - 1])
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                else
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        
        // str length - palindrome length
        return n - dp[n][n]; 
} 
```