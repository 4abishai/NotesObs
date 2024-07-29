Given an array **arr[]** of size **N**, check if it can be partitioned into two parts such that the sum of elements in both parts is the same.

```cpp
class Solution{
private:
    int subsetSum(int arr[], int n, int sum) {
        
        vector<vector<int>> dp(n + 1, vector<int> (sum + 1, 0));
        
        for(int i = 0; i <= n; i++)
            dp[i][0] = 1;
        
        for(int i = 1; i <= n; i++) {
            for(int j = 1; j <= sum; j++) {
                if (arr[i - 1] <= j)
                    dp[i][j] = dp[i - 1][j - arr[i - 1]] || dp[i - 1][j];
                else
                    dp[i][j] = dp[i - 1][j];
            }
        }
        
        return dp[n][sum];
    }
public:
    int equalPartition(int N, int arr[])
    {
        int arrSum = 0;
        for (int i = 0; i < N; i++)
            arrSum += arr[i];
        
        if(arrSum % 2 != 0)
            return 0;
            
        int targetPartSum = arrSum/2;
        
        return subsetSum(arr, N, targetPartSum);
    }
};
```