Given two distinct words **startWord** and **targetWord**, and a list denoting **wordList** of unique words of equal lengths. Find the length of the shortest transformation sequence from startWord to targetWord.  
Keep the following conditions in mind:

- A word can only consist of lowercase characters.
- Only one letter can be changed in each transformation.
- Each transformed word must exist in the wordList including the targetWord.
- startWord may or may not be part of the wordList

```cpp
public:
    int wordLadderLength(string startWord, string targetWord, vector<string>& wordList) {
        queue<pair<string, int>> q;
        q.push({startWord, 1});
        
        unordered_set<string> st(wordList.begin(), wordList.end());
        st.erase(startWord);
        
        while(!q.empty()) {
            string word = q.front().first;
            int steps = q.front().second;
            q.pop();
            
            if (word == targetWord) return steps;
            
            for (int i = 0; i < word.size(); i++) {
                char original = word[i];
                
                for (char c = 'a'; c <= 'z'; c++) {
                    // replace i with new char
                    word[i] = c;
                    if (st.find(word) != st.end()) {
                        st.erase(word);
                        q.push({word, steps + 1});
                    }
                }
                // restore the orginal char
                // before moving to next char in the same word
                word[i] = original;
            }
            
        }
        
        return 0;
    }
};
```