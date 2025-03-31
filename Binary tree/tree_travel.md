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
---
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
---
找寻二叉树两节点最近公共祖先。

首先层序遍历，将每个节点与其父节点存入map中，root的父节点为nullptr。

之后遍历其中一个节点至root，将该路径上的节点存入set中。

最后遍历另一个节点至root，若遍历过程中发现节点存在set中，则该节点就是最近公共祖先。
```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root)
            return nullptr;
        unordered_map<TreeNode*, TreeNode*> parent;
        parent[root] = nullptr;
        queue<TreeNode*> que;
        que.push(root);
        while (!que.empty())
        {
            int n = que.size();
            for (int i = 0; i < n; ++i)
            {
                TreeNode* node = que.front();
                que.pop();
                if (node->left)
                {
                    que.push(node->left);
                    parent[node->left] = node;
                }
                if (node->right)
                {
                    que.push(node->right);
                    parent[node->right] = node;
                }
            }
            if (parent.find(q) != parent.end() && parent.find(p) != parent.end())
                break;
        }
        unordered_set<TreeNode*> set;
        set.insert(q);
        TreeNode* cur = q;
        while (parent[cur] != nullptr)
        {
            cur = parent[cur];
            set.insert(cur);
        }
        cur = p;
        while (set.find(cur) == set.end())
            cur = parent[cur];
        return cur;
    }
};
```
---
二叉树的序列化与反序列化。

serialize函数将二叉树保存为字符串格式的数据，采用层序遍历，每个节点之间用逗号分隔，空节点用井号表示，但空节点的左右孩子无法遍历，也无法表示，故在字符串中省略。

deserialize函数将上述的字符串反序列化为二叉树，与层序遍历相同，使用队列存储当前层的节点，而后分别向左右孩子赋值，并push左右孩子进队列。循环的条件为队列不为空（没有循环到最后一层）且指针还没指向字符串的最后一位，故该循环也不会遍历空节点的左右孩子，与序列化函数相对应。
```cpp
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if (!root)
            return "";
        queue<TreeNode*> que;
        string data;
        que.push(root);
        data += to_string(root->val);
        data += ',';
        while (!que.empty())
        {
            int n = que.size();
            for (int i = 0; i < n; ++i)
            {
                TreeNode* node = que.front();
                que.pop();
                if (node->left)
                {
                    que.push(node->left);
                    data += to_string(node->left->val);
                }
                else
                    data += '#';
                data += ',';
                if (node->right)
                {
                    que.push(node->right);
                    data += to_string(node->right->val);
                }
                else
                    data += '#';   
                data += ',';
            }
        }
        data.pop_back();
        return data;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if (data.empty())
            return nullptr;
        vector<string> nodes;
        string token;
        istringstream t(data);
        while (getline(t, token, ','))
        {
            nodes.push_back(token);
        }
        TreeNode* root = new TreeNode(stoi(nodes[0]));
        queue<TreeNode*> que;
        que.push(root);
        int i = 1;
        while (!que.empty() && i < nodes.size())
        {
            TreeNode* parent = que.front();
            que.pop();
            if (nodes[i] != "#")
            {
                parent->left = new TreeNode(stoi(nodes[i]));
                que.push(parent->left);
            }
            if (nodes[i + 1] != "#")
            {
                parent->right = new TreeNode(stoi(nodes[i + 1]));
                que.push(parent->right);
            }
            i += 2;
        }
        return root;
    }
};
```
