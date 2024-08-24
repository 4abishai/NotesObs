Given an array of integers **Arr** of size **N** and a number **K**. Return the **maximum sum** of a subarray of size **K**.

**NOTE*:** A subarray is a contiguous part of any given array.

#Sliding-Window

```python
class Solution:
    def maximumSumSubarray(self, K, Arr, N):
        # Initialize pointers and variables
        i = j = 0
        current_sum = 0
        max_sum = float('-inf')
        
        while j < N:
            # Add new element to the window
            current_sum += Arr[j]
            
            # If window size has not been reached yet
            if j - i + 1 < K:
                j += 1
            
            # Window size reached
            elif j - i + 1 == K:
                # Update max_sum if current_sum is greater
                max_sum = max(max_sum, current_sum)
                
                # Remove the first element of the window
                current_sum -= Arr[i]
                
                # Slide the window
                i += 1
                j += 1
        
        return max_sum
```