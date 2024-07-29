## Pseudocode

```perl
function fractional_knapsack(W, items):
    # Step 1: Calculate the value-to-weight ratio for each item
    for each item in items:
        item.ratio = item.value / item.weight
    
    # Step 2: Sort items based on value-to-weight ratio in descending order
    sort items by item.ratio in descending order
    
    # Step 3: Initialize total value and current weight
    total_value = 0
    current_weight = 0
    
    # Step 4: Iterate through the sorted list of items
    for each item in items:
        if current_weight + item.weight <= W:
            # Add the entire item
            current_weight = current_weight + item.weight
            total_value = total_value + item.value
        else:
            # Add fractional part of the item
            remaining_weight = W - current_weight
            total_value = total_value + item.ratio * remaining_weight
            break  # Knapsack is full
    
    # Step 5: Return the total value of items in the knapsack
    return total_value
```

### Question
Given weights and values of **n** items, we need to put these items in a knapsack of capacity **w** to get the **maximum** total value in the knapsack.  
**Note:** Unlike 0/1 knapsack, you are **allowed** to break the item here.

#### Answer
```cpp
/*struct Item{
    int value;
    int weight;
}; */

class SortByR {
    public:
    // Overloaded function call operator () to sort pairs by their second value.
    bool operator()(pair<Item, double>& a, pair<Item, double>& b) {
        return a.second > b.second;
    }
};

class Solution {
  public:
    // Function to get the maximum total value in the knapsack.
    double fractionalKnapsack(int w, Item arr[], int n) {
        // Your code here
        
        // Vector to store pairs of items and their value-to-weight ratios.
        vector<pair<Item, double>> vec;
        
        // value-to-weight ratio for each item and storing it in the vector.
        for(int i = 0; i < n; i++) {
            pair<Item, double> p = make_pair(arr[i], 
	            static_cast<double>(arr[i].value) / arr[i].weight);
            vec.push_back(p);
        } 
        
        // Sorting the vector of pairs based on value-to-weight ratios.
        sort(vec.begin(), vec.end(), SortByR());
       
        // Variables to keep track of total value and current weight.
        double totalVal = 0;
        int curWt = 0;
       
        // Iterating through sorted vector and adding items to the knapsack.
        for(auto item : vec) {
            if (curWt + item.first.weight <= w) {
                // Item fits completely: add its value and update.
                curWt += item.first.weight;
                totalVal += item.first.value;
            }
            else {
                // Item doesn't fit completely: add its fractional value.
                int remainingWt = w - curWt;
                totalVal += item.second * remainingWt;
                break;
            }
        }
       
        // Return the total value of items in the knapsack.
        return totalVal;
    }
};

```