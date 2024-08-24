Given two strings `s` and `p`, return _an array of all the start indices of_ `p`_'s anagrams in_ `s`. You may return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

#Sliding-Window 

```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        i = j = 0
        char_cnt = Counter(p)

        count = len(char_cnt)
        size = len(s)
        k = len(p)
        ans = []
        
        while j < size:
            
            if s[j] in char_cnt:
                char_cnt[s[j]] -= 1
                if char_cnt[s[j]] == 0:
                    count -= 1
            
            if j - i + 1 < k:
                j += 1
            
            elif j - i + 1 == k:
                if count == 0:
                    ans.append(i)

                if s[i] in char_cnt:
                    if char_cnt[s[i]] == 0:
                        count += 1    
                    char_cnt[s[i]] += 1
                
                # slide the window
                i += 1
                j += 1

        return ans
```