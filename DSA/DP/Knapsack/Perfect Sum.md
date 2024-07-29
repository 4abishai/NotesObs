Given an array **arr** of non-negative integers and an integer **sum**, the task is to count all subsets of the given array with a sum equal to a given **sum**.

**Note:** Answer can be very large, so, output answer modulo **109+7**.

**Constraints:**  
1 ≤ N * sum ≤ 10^6
0 ≤ arr[i] ≤ 10^6

### Memoization
```cpp
class Solution {
public:
    int mod = 1e9 + 7;
    
    int perfectSumUtil(int arr[], int n, int sum, vector<vector<int>>& memo) {
        // Base Cases   
        
        if(n == 0 && sum == 0) return 1; // Empty subset sums to zero
        if(n == 0) return 0; // No elements left, and sum is not zero
        
        // Check if already computed
        if (memo[n][sum] != -1) return memo[n][sum];
        
        // If the last element is greater than the sum, ignore it
        if (arr[n-1] > sum)
            memo[n][sum] = perfectSumUtil(arr, n-1, sum, memo) % mod;
        else
            memo[n][sum] = (perfectSumUtil(arr, n-1, sum, memo) % mod + perfectSumUtil(arr, n-1, sum - arr[n-1], memo) % mod) % mod;
        
        return memo[n][sum];
    }

    int perfectSum(int arr[], int n, int sum) {
        // Create a memoization table and initialize with -1
        vector<vector<int>> memo(n+1, vector<int>(sum+1, -1));
        return perfectSumUtil(arr, n, sum, memo);
    }
};
```

### Tabulation
```cpp
class Solution{

	public:
	int perfectSum(int arr[], int n, int sum)
	{
         vector<vector<int>> dp(n + 1, vector<int> (sum + 1, 0));
        
        dp[0][0] = 1;
        
        int mod = (int)1e9+7;
        
        // The inner loop iterates over all possible subset sums from 0 to sum.
        // As arr[j] = 0 is possible (see constraint)
        for(int i = 1; i <= n; i++) {
            for(int j = 0; j <= sum; j++) {
                if(arr[i - 1] <= j)
                    dp[i][j] = (
	                    (dp[i-1][j-arr[i-1]] % mod)
                        + (dp[i-1][j] % mod)) % mod;
                
                else
                    dp[i][j] = dp[i-1][j] % mod;
            }
        }
        
        return dp[n][sum];
	}
};
```