
Given an array of integers `nums` and an integer `k`, return _the total number of subarrays whose sum equals to_ `k`.

A sub-array is a contiguous **non-empty** sequence of elements within an array.

**Example:**

**Input:** arr = [1,1,1], k = 2
**Output:** 2

## Recursion

1. Express in form of (`index`, `target`) and write the base cases
	- In the entire array does there exists a `target`

2. Explore possibilities of that index
	- Arr[index] -> part of the sub-sequence
	- Arr[index] -> NOT part of the sub-sequence

3. return T / F

### Base Cases

```cpp
if (target == 0) 
	return true;
if (index == 0)
	return (arr[0] == target);
```

### Take and Not Take
```cpp
bool notTake = fun(index - 1, target);
bool take = false;
if (target >= arr[index]) // target has been taken into consideration
	take = fun(index - 1, target - arr[index]);
```

```cpp
return notTake | take;
```

```cpp
bool fun(int ind, int target,  vector<int> &arr){

    if (target == 0)
        return true;
    
    if (ind == 0)
        return (arr[0] == target);

    bool notTake = fun(ind-1, target, arr);

    bool take = false;
    if (arr[ind] <= target)
        take = fun(ind-1, target - arr[ind], arr);

    return notTake | take;

}

bool subsetSumToK(int n, int k, vector<int> &arr) {
    
    return fun(n-1, k, arr);
}
```
### Recursion Tree
![[Pasted image 20240307225351.png]]

## Memoization

**Changing:** `index`, `target`

**Constraints:** `index <= 10^3`, `target <= 10^3`

**Array:** `int dp [10^3 + 1][10^3 + 1]`

```cpp
if (target == 0) 
	return dp[index][target] = true;
	
if (index == 0)
	return  dp[index][target] = (arr[0] == target);
	
if (dp[index][target] != -1)
	return dp[index][target];
```

```cpp
bool fun(int ind, int target,  vector<int> &arr, vector<vector<int>> &dp){

    if (target == 0)
        return dp[ind][target] = true;
    
    if (ind == 0)
        return dp[ind][target] = (arr[0] == target);

    if (dp[ind][target] != -1)
        return dp[ind][target];

    bool notTake = fun(ind-1, target, arr, dp);

    bool take = false;
    if (arr[ind] <= target)
        take = fun(ind-1, target - arr[ind], arr, dp);

    return dp[ind][target] = notTake | take;

}

bool subsetSumToK(int n, int k, vector<int> &arr) {
    
    vector<vector<int>> dp(n, vector<int> (k + 1, -1)); 
    return fun(n-1, k, arr, dp);
}
```

## Tabulation

**Changing:** `index`, `target`

**Constraints:** `index <= 10^3`, `target <= 10^3`

**Array:** `bool dp [10^3 + 1][10^3 + 1]`

```cpp
if (target == 0) 
	return true;
	
if (index == 0)
	return (arr[0] == target);
	
if (dp[index][target] != -1)
	return dp[index][target];
```

```cpp
bool subsetSumToK(int n, int k, vector<int> &arr) {

    vector<vector<bool>> dp(n, vector<bool>(k + 1));

    for (int ind = 0; ind < n; ind++)
        dp[ind][0] = true;

    if(arr[0]  <= k)
        dp[0][arr[0]] = true;

    for (int i = 1; i < n; i++){
        for (int j = 1; j <= k; j++){

        bool notTake = dp[i-1][j];

        bool take = false;
        
        if (arr[i] <= j)
	        take = dp[i-1][j- arr[i]];
        
        dp[i][j] = take | notTake;
        }
    }
    return dp[n-1][k];
}
```

## Space Optimisation

We can space optimise if we have a state as `i - 1`  
- `cur` row will use state of `prev` row.

Here,
- In the base case `for (int ind = 0; ind < n; ind++) dp[ind][0] = true;` 
  every first index of row is marked as `true`.
	- Hence, `prev[0] = cur[0] = true;`

- `dp[0][arr[0]] = true;` -> `prev[arr[0]]`

-  `dp[index - 1][target]` -> `prev[target]`

-  `dp[index][target]` -> `cur[target]`

-  After processing each row, 
	 `prev = cur;`

```cpp
bool subsetSumToK(int n, int k, vector<int> &arr) {

    vector<bool> prev(k + 1), cur(k + 1);

    prev[0] = cur[0] = true;    

    if(arr[0]  <= k)
        prev[arr[0]] = true;

    for (int i = 1; i < n; i++){
        for (int j = 1; j <= k; j++){

        bool notTake = prev[j];

        bool take = false;
        
        if (arr[i] <= j)
            take = prev[j- arr[i]];
        
        cur[j] = take | notTake;
        }
        
        prev = cur;
    }

    return prev[k];
}
```



## Dry Run - Tabulation
## Dry Run - Space Optimization


