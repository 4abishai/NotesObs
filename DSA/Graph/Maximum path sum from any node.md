Given a binary tree, the task is to find the maximum path sum. The path may start and end at any node in the tree.

```cpp
class Solution {
private:
    int solve(Node* root, int& res) {
        if (!root) return 0;
        
        int l = solve(root->left, res);
        int r = solve(root->right, res);
        
        int height = root->data + max(l, r);
        int maxSinglePath = max(root->data, height);
        int maxTop = max(maxSinglePath, root->data + l + r);
        
        res = max(res, maxTop);
        
        return maxSinglePath;
    }
  public:
    //Function to return maximum path sum from any node in a tree.
    int findMaxSum(Node* root)
    {
        int res = 0;
        
        solve(root, res);
        return res;
    }
};
```