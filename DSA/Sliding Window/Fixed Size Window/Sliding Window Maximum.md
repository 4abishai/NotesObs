You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return _the max sliding window_.

#Sliding-Window 

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        size = len(nums)
        i = j = 0
        heap = []
        ans = []
        
        while j < size:
            heapq.heappush(heap, (-nums[j], j))
            
            if j - i + 1 < k:
                j += 1
            elif j - i + 1 == k:
                # Remove elements not in the current window
                while heap and heap[0][1] < i:
                    heapq.heappop(heap)
                
                # The maximum value in the current window
                ans.append(-heap[0][0])
                
                i += 1
                j += 1

        return ans
```