Given two distinct words **startWord** and **targetWord**, and a list denoting **wordList** of unique words of equal lengths. Find all shortest transformation sequence(s) from startWord to targetWord. You can return them in any order possible.  
Keep the following conditions in mind:

- A word can only consist of lowercase characters.
- Only one letter can be changed in each transformation.
- Each transformed word must exist in the wordList including the targetWord.
- startWord may or may not be part of the wordList.
- Return an empty list if there is no such transformation sequence.
![[Pasted image 20240707131200.png]]
![[Pasted image 20240707131209.png]]

```cpp
class Solution {
public:
    vector<vector<string>> findSequences(string beginWord, string endWord, 
    vector<string>& wordList) 
    {
        unordered_set<string> st(wordList.begin(), wordList.end());
        queue<vector<string>> q;
        vector<string> lvl_words;
        
        q.push({beginWord});
        lvl_words.push_back(beginWord);
        
        int lvl = 0;
        
        vector<vector<string>> ans;
        
        while(!q.empty()) {
            vector<string> v = q.front();
            q.pop();
            
            // Erase words that have been added
            // on the previous level
            if (v.size() > lvl) {
                ++lvl;
                for (auto e : lvl_words) {
                    st.erase(e);
                }
            }
            
            if (endWord == v.back()) {
                // For the first sequence
                if (!ans.size())
                    ans.push_back(v);
                
                else if (ans[0].size() == v.size())
                    ans.push_back(v);
                    
            }
            
            string word = v.back();
            for (int i = 0; i < word.size(); i++) {
                char original = word[i];
                for (char c = 'a'; c <= 'z'; c++) {
                    // replace i with new char
                    word[i] = c;
                    if (st.find(word) != st.end()) {
                        v.push_back(word);
                        q.push(v);
                        lvl_words.push_back(word);
                        v.pop_back();
                    }
                }
                // restore the orginal char
                // before moving to next char in the same word
                word[i] = original;
            }
            
        }
        return ans;
        
    }
};
```