Given an array **arr**, partition it into two subsets(possibly empty) such that each element must belong to only one subset. Let the sum of the elements of these two subsets be **S1** and **S2**. Given a difference **d**, count the number of partitions in which **S1** is greater than or equal to **S2** and the difference between **S1** and **S2** is equal to **d**. Since the answer may be large return it **modulo 10^9 + 7**.

**Constraint:**  
1 <= n <= 500  
0 <= d  <= 25000  
0 <= arr[i] <= 50

![[Pasted image 20240610211924.png]]

```cpp
class Solution {
  public:
    int countPartitions(int n, int d, vector<int>& arr) {
        
        int mod = (int)1e9+7;
        int sum = accumulate(arr.begin(), arr.end(), 0);
        
        // If (d + sum) is odd, there is no possible partition
        if ((d + sum) % 2 != 0) {
            return 0;
        }

        // Sum of one partition
        int target = (d + sum) / 2;
        
        vector<vector<int>> dp(n + 1, vector<int> (target + 1, 0));
        
        // Base case: As arr[j] = 0 is possible (0 <= arr[i] <= 50)
        dp[0][0] = 1;
        
        for(int i = 1; i <= n; i++) {
            for(int j = 0; j <= target; j++) {
                if(arr[i - 1] <= j)
                    dp[i][j] = (
	                    (dp[i-1][j-arr[i-1]] % mod)
                        + (dp[i-1][j] % mod)) % mod;
                
                else
                    dp[i][j] = dp[i-1][j] % mod;
            }
        }
        
        return dp[n][target];
    }
};
```