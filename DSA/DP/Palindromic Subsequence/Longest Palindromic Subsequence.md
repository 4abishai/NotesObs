Given a String, find the longest palindromic subsequence.

**NOTE:** Subsequence of a given sequence is a sequence that can be derived from the given sequence by deleting some or no elements without changing the order of the remaining elements

```cpp
class Solution{
  public:
    int longestPalinSubseq(string A) {
        int n = A.size();
        
        string revA = A;
        
        reverse(A.begin(), A.end());
        
        vector<vector<int>> dp(n + 1, vector<int> (n + 1, 0));
        
        for(int i = 0; i <= n; i++) {
            for(int j = 0; j <= n; j++) {
                if (i == 0 || j == 0)
                    dp[i][j] = 0;
                else if(A[i - 1] == revA[j - 1])
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                else
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        
        return dp[n][n];
    }
};
```