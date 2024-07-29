Given two strings **str1** & **str 2** of length **n** & **m** respectively, return the length of their **longest common subsequence.** If there is no common subsequence then, **return 0**. 

> _A subsequence is a sequence that can be derived from the given string by deleting some or no elements without changing the order of the remaining elements. For example, "abe" is a subsequence of "abcde"._

![[Pasted image 20240612171551.png]]
![[Pasted image 20240612171655.png]]

### TOP DOWN APPROACH
### Recursive (not optimized -- will take more time) 

```cpp
class Solution {
  public:
    // Function to find the length of longest common subsequence in two strings.
    int lcs(int n, int m, string str1, string str2) {
        if (n == 0 || m == 0)
            return 0;
        
        if (str1[n - 1] == str2[m - 1])
            return (1 + lcs(n - 1, m - 1, str1, str2));
            
        else
            return max(lcs(n, m - 1, str1, str2), lcs(n - 1, m, str1, str2));
    }
};
```

#### Memoization

```cpp
class Solution {
private:
    vector<vector<int>> dp;
public:
    // Function to find the length of the longest common subsequence in two strings.
    int lcs(int n, int m, string str1, string str2) {
        // Initialize the dp vector with -1
        dp.resize(n + 1, vector<int>(m + 1, -1));
        return lcsUtil(n, m, str1, str2);
    }

    // Helper function for lcs
    int lcsUtil(int n, int m, string& str1, string& str2) {
        
        if (n == 0 || m == 0)
            return 0;
        if (dp[n][m] != -1)
            return dp[n][m];

        if (str1[n - 1] == str2[m - 1])
            dp[n][m] = 1 + lcsUtil(n - 1, m - 1, str1, str2);
        else
            dp[n][m] = max(
	            lcsUtil(n, m - 1, str1, str2), 
	            lcsUtil(n - 1, m, str1, str2));

        return dp[n][m];
    }
};
```