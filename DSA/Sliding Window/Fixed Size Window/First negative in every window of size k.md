Given an array **A[]** of size **N** and a positive integer **K**, find the first negative integer for each and every window(contiguous subarray) of size **K**.

#Sliding-Window

```python
def printFirstNegativeInteger(A, N, k):
    # code here
    i = j = 0
    ans = []
    queue = []
    
    while j < N:
        if A[j] < 0:
            queue.append(A[j])
        
        if j - i + 1 < k:
            j += 1
            
        elif j - i + 1 == k:
            # Window operation -- add first neg elem
            if not queue:
                ans.append(0)
            else:
                ans.append(queue[0])
            
            # If first element in the window 
            if queue and queue[0] == A[i]:
                queue.pop(0)
            
            # Slide the window
            i += 1
            j += 1
    
    return ans
```