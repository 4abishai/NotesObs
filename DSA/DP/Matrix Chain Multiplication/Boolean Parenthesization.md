Given a boolean expression **s** of length **n** with following symbols.  
Symbols  
    'T' ---> true  
    'F' ---> false  
and following operators filled between symbols  
Operators  
    &   ---> boolean AND  
    |   ---> boolean OR  
    ^   ---> boolean XOR  
Count the number of ways we can parenthesize the expression so that the value of expression evaluates to true.

**Note:** The answer can be large, so return it with **modulo 1003**
### TOP DOWN APPROACH
#### Recursive
```cpp
class Solution{
private:
    int solve(int i, int j, bool reqBool, string s) {
        if (i > j)
            return 0;
        
       if (i == j) {
            if (reqBool) 
                return s[i] == 'T';
            else
                return s[i] == 'F';
        }
        
        int ans = 0;
        
        for (int k = i + 1; k <= j - 1; k += 2) {
            int Lt = solve(i, k - 1, true, s);
            int Lf = solve(i, k - 1, false, s);
            int Rt = solve(k + 1, j, true, s); 
            int Rf = solve(k + 1, j, false, s);
            
            if (s[k] == '^') {
                if (reqBool)
                    ans += Lt * Rf + Lf * Rt;
                else
                    ans += Lt * Rt + Lf * Rf;
            }
            
            if (s[k] == '&') {
                if (reqBool)
                    ans += Lt * Rt;
                else
                    ans += Lt * Rf + Lf * Rt + Lf * Rf;
            }
            
            if (s[k] == '|') {
                if (reqBool)
                    ans += Lt * Rt + Lt * Rf + Lf * Rt;
                else
                    ans += Lf * Rf;
            }
        }
        return ans;
    }
public:
    int countWays(int n, string s){
        return solve(0, n - 1, true, s);
    }
};
```

#### Memoization
```cpp
class Solution {
private:
    vector<vector<vector<int>>> dp;
    const int MOD = 1003;
    
    int solve(int i, int j, bool reqBool, string& s) {
        if (i > j)
            return 0;
        
        if (i == j) {
            if (reqBool) 
                return s[i] == 'T';
            else
                return s[i] == 'F';
        }
        
        if (dp[i][j][reqBool] != -1)
            return dp[i][j][reqBool];
        
        int ans = 0;
        
        for (int k = i + 1; k <= j - 1; k += 2) {
            int Lt = solve(i, k - 1, true, s);
            int Lf = solve(i, k - 1, false, s);
            int Rt = solve(k + 1, j, true, s); 
            int Rf = solve(k + 1, j, false, s);
            
            if (s[k] == '^') {
                if (reqBool)
                    ans = (ans + (  Lt * Rf +   Lf * Rt) % MOD) % MOD;
                else
                    ans = (ans + (  Lt * Rt +   Lf * Rf) % MOD) % MOD;
            } else if (s[k] == '&') {
                if (reqBool)
                    ans = (ans +   Lt * Rt % MOD) % MOD;
                else
                    ans = (ans + (  Lt * Rf +   Lf * Rt +   Lf * Rf) % MOD) % MOD;
            } else if (s[k] == '|') {
                if (reqBool)
                    ans = (ans + (  Lt * Rt +   Lt * Rf +   Lf * Rt) % MOD) % MOD;
                else
                    ans = (ans +   Lf * Rf % MOD) % MOD;
            }
        }
        
        return dp[i][j][reqBool] = ans;
    }
public:
    int countWays(int n, string s) {
        dp.resize(n, vector<vector<int>>(n, vector<int>(2, -1)));
        return solve(0, n - 1, true, s);
    }
};
```