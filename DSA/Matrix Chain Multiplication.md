Given a sequence of matrices, find the most efficient way to multiply these matrices together. The efficient way is the one that involves the least number of multiplications.

The dimensions of the matrices are given in an array **arr[]** of size **N** (such that N = number of matrices + 1) where the **ith** matrix has the dimensions **(arr[i-1] x arr[i])**.

![[Pasted image 20240620211029.png]]
### TOP DOWN APPROACH
#### Recursive
```cpp
class Solution {
private:
    int solve(int arr[], int i, int j) {
        if (i >= j)
            return 0;
        int mn = INT_MAX;
        
        for (int k = i; k <= j - 1; k++) {
            int temp = solve(arr, i, k)
                        + solve(arr, k + 1, j)
                        + arr[i - 1] * arr[k] * arr[j];
            
            if (temp < mn)
                mn = temp;
        }
        
        return mn;
    }
public:
    int matrixMultiplication(int N, int arr[]) {
        int i = 1;
        int j = N - 1;
        return solve(arr, i, j);
    }
};
```

#### Memoization
```cpp
class Solution {
private:

    int solve(int arr[], int i, int j, vector<vector<int>>& dp) {
        if (i >= j)
            return dp[i][j] = 0;
        if (dp[i][j] != -1)
            return dp[i][j];
            
        int mn = INT_MAX;
        
        for (int k = i; k <= j - 1; k++) {
            int temp = solve(arr, i, k, dp)
                        + solve(arr, k + 1, j, dp)
                        + arr[i - 1] * arr[k] * arr[j];
            
             mn = min(mn, temp);
        }
        
        return dp[i][j] = mn;
    }
public:
    int matrixMultiplication(int N, int arr[]) {
        vector<vector<int>> dp(N + 1, vector<int> (N + 1, -1));
        
        return solve(arr, 1, N - 1, dp);
    }
};
```