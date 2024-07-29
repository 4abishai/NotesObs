Given an array **arr** of size **n** containing **non-negative** integers, the task is to divide it into two sets **S1** and **S2** such that the absolute difference between their sums is minimum and find the **minimum** difference
```cpp
class Solution{
private:
    vector<vector<bool>> getDPTable(int arr[], int n, int sum) 
    {
        vector<vector<bool>> dp(n + 1, vector<bool> (sum/2 + 1, false));
        
        for(int i = 0; i <= n; i++) 
            dp[i][0] = true;
        
        for(int i = 1; i <= n; i++) 
        {
            for(int j = 1; j <= sum/2; j++) 
            {
                if(arr[i - 1] <= j)
                    dp[i][j] = dp[i - 1][j - arr[i - 1]] || dp[i - 1][j];
                
                else
                    dp[i][j] = dp[i - 1][j]; 
            }
        }
        
        return dp;
    }

  public:
	int minDifference(int arr[], int n)  
    { 
	    // Calculate the sum of the array
	    int sum = accumulate(arr, arr + n, 0);  
	    
	    auto dp = getDPTable(arr, n, sum);
        
        int minDiff = INT_MAX;
        
         // Find the largest j such that dp[n][j] is true 
         // where j is the closest to sum/2
        for (int j = sum/2; j >= 0; j--) {
            if (dp[n][j]) {
                minDiff = sum - 2 * j; break;
            }
        }
         
	    return minDiff;
	} 
};
```