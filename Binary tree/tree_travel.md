定义二叉树
```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};
```
前中后序遍历均使用递归算法。

前序：root->left->right

中序：left->root->right

后序：left->right->root

层序遍历使用广度优先算法，队列将当前层的节点自左向右push进，将其val依次push进vector，子节点push进队列。原节点需pop出。
```cpp
class Solution {
public:
// 前序遍历
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ans;
        pre(ans, root);
        return ans;
    }
    void pre(vector<int>& ans, TreeNode* root){
        if (root == nullptr)
            return;
        ans.push_back(root->val);
        pre(ans, root->left);
        pre(ans, root->right);
    }
// 中序遍历
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        in(ans, root);
        return ans;
    }
    void in(vector<int>& ans, TreeNode* root){
        if (root == nullptr)
            return;
        in(ans, root->left);
        ans.push_back(root->val);
        in(ans, root->right);        
    }
// 后序遍历
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> ans;
        post(ans, root);
        return ans;
    }
    void post(vector<int>& ans, TreeNode* root){
        if (root == nullptr)
            return;
        post(ans, root->left);
        post(ans, root->right);
        ans.push_back(root->val);
    }
// 层序遍历
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if (!root)
            return ans;
        queue<TreeNode*> que;
        que.push(root);
        while (!que.empty())
        {
            int size = que.size();
            vector<int> temp;
            for (int i = 1; i <= size; ++i)
            {
                TreeNode* node = que.front();
                que.pop();
                temp.push_back(node->val);
                if (node->left)
                    que.push(node->left);
                if (node->right)
                    que.push(node->right);
            }
            ans.push_back(temp);
        }
        return ans;
    }
};
```
