Given a string **str**, a partitioning of the string is a palindrome partitioning if every sub-string of the partition is a palindrome. Determine the fewest cuts needed for palindrome partitioning of the given string.

#### Not optimized

```cpp
class Solution{
private: 
    bool isPalindrome(int i, int j, string& str) {
        while (i < j) {
            if (str[i++] != str[j--])
                return false;
        }
        return true;
    }
    int solve(string& str, int i, int j, vector<vector<int>>& dp) {
        if (dp[i][j] != -1)
            return dp[i][j];
            
        if (i >= j)
            return dp[i][j] = 0;


        if (isPalindrome(i, j, str))
            return dp[i][j] = 0;
            
        
        int mn = INT_MAX;
        
        for(int k = i; k <= j - 1; k++) {
            int left = solve(str, i, k, dp);
            int right = solve(str, k + 1, j, dp);
            int cuts = left + right + 1; // cost: 1
            
            mn = min(mn, cuts);
        }
        
        return dp[i][j] = mn;
        
    }
public:
    int palindromicPartition(string str)
    {
        int N = str.length();
        vector<vector<int>> dp(N, vector<int> (N, -1));
        
        return solve(str, 0, N - 1, dp);
    }
};
```

#### Optimized

```cpp
class Solution{
private:

    bool isPalindrome(int i, int j, string& str) {
        while (i < j) {
            if (str[i++] != str[j--])
                return false;
        }
        return true;
    }
    int solve(string& str, int i, int j, vector<vector<int>>& dp) {
        if (dp[i][j] != -1)
            return dp[i][j];
            
        if (i >= j || isPalindrome(i, j, str))
            return dp[i][j] = 0;
            
        
        int minCuts = INT_MAX;
        
        for(int k = i; k <= j - 1; k++) {
         if (isPalindrome(i, k, str)) {
                int cuts = 1 + solve(str, k + 1, j, dp);
                minCuts = min(minCuts, cuts);
            }
        }
        
        return dp[i][j] = minCuts;
        
    }    
public:
    int palindromicPartition(string str)
    {
        int N = str.length();
        vector<vector<int>> dp(N, vector<int> (N, -1));
        
        return solve(str, 0, N - 1, dp);
    }
};
```