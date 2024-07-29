Given a list of **accounts** of size **n** where each element **accounts [ i ]** is a list of strings, where the first element **account [ i ][ 0 ]**  is a **name**, and the rest of the elements are **emails** representing emails of the account.  
Geek wants you to merge these accounts. Two accounts belong to the same person if there is some common email to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts have the same name.  
After merging the accounts, return the accounts in the following format: The first element of each account is the name, and the rest of the elements are emails **in** **sorted order**.

**Note**: Accounts themselves can be returned in **any order**.

```cpp
class DisjointSet {
public:
    vector<int> size, parent;
 
    DisjointSet(int n) {
        size.resize(n + 1, 1);
        parent.resize(n + 1);
        for (int i = 0; i < n + 1; i++)
            parent[i] = i;
    }

    // Find ultimate parent
    int findUPar(int node) {
        if (node == parent[node])
            return node;
        else
            return parent[node] = findUPar(parent[node]);
    }

    void unionBySize(int u, int v) {
        int up_u = findUPar(u);
        int up_v = findUPar(v);

        if (up_u == up_v)
            return;

        // We attach the smaller tree to the taller tree
        if (size[up_u] < size[up_v]) {
            parent[up_u] = up_v;
            size[up_v] += size[up_u];
        } else {
            parent[up_v] = up_u;
            size[up_u] += size[up_v];
        }
    }
};


class Solution {
public:
    vector<vector<string>> accountsMerge(vector<vector<string>> &accounts) {
        int n_accs = accounts.size();
        DisjointSet ds(n_accs);
        
        unordered_map<string, int> map;
        
        for (int i = 0; i < n_accs; i++) {
            for (int j = 1; j < accounts[i].size(); j++) {  
                string mail = accounts[i][j];  
                
                // If mail is not present
                if (map.find(mail) == map.end())
                    map[mail] = i;
                // If mail is already present
                else
                    ds.unionBySize(map[mail], i);
            }
        }
        
        vector<vector<string>> mailList(n_accs); 
        
        for (auto e : map) {
            string mail = e.first;
            int acc = ds.findUPar(e.second);
            
            mailList[acc].push_back(mail);
        }
        
        vector<vector<string>> ans;
        
        for (int i = 0; i < n_accs; i++) { 
            if (mailList[i].size() == 0) continue;  
            
            sort(mailList[i].begin(), mailList[i].end());
            
            vector<string> temp;
            // Push name first
            temp.push_back(accounts[i][0]);
            
            for (auto mail : mailList[i])
                temp.push_back(mail);
                
            ans.push_back(temp);
        }
        
        return ans;
    }
};
```