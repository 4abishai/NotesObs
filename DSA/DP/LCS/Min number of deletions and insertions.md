Given two strings **str1** and **str2**. The task is to remove or insert the minimum number of characters from/in **str1** so as to transform it into **str2**. It could be possible that the same character needs to be removed/deleted from one point of **str1** and inserted to some another point.

```cpp
class Solution{
		

	public:
	int minOperations(string str1, string str2) 
	{ 
	    int m = str1.size();
	    int n = str2.size();
	    
	  vector<vector<int>> dp(m + 1, vector<int> (n + 1, 0));
        
        for(int i = 0; i <= m; i++) {
            for(int j = 0; j <= n; j++) {
                if (i == 0 || j == 0)
                    dp[i][j] = 0;
                else if(str1[i - 1] == str2[j - 1])
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                else
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        
        // Subtract the common longest subsequence
        int insert = m - dp[m][n];
        int del = n - dp[m][n];
        
        return insert + del;
	} 
};
```