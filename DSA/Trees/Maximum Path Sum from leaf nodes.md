A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.

The **path sum** of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return _the maximum **path sum** of any **non-empty** path_.

```cpp
class Solution {
private:
    int solve(TreeNode* root, int& res) {
        if (!root) return 0;

        int l = solve(root->left, res);
        int r = solve(root->right, res);

        // Only consider positive contributions
        l = max(l, 0);
        r = max(r, 0);

        int height = max(l, r) + root->val;
        int maxTop = root->val + l + r;

        res = max(res, maxTop);

        return height;
    }
public:
    int maxPathSum(TreeNode* root) {
        int res = INT_MIN;

        solve(root, res);
        return res;
    }
};
```