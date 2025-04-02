N叉树为二叉树的延申，遍历方法同样有前中后序和层序。

前序遍历先访问根节点，然后依次访问N叉树的子节点。中序遍历没有标准定义。后序遍历先依次访问N叉树的子节点，再访问根节点。层序遍历与二叉树相同。

N叉树的定义
---
子节点用向量存储，其余与二叉树相同。
```cpp
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
```
N叉树的前序遍历
---
```cpp
class Solution {
public:
    vector<int> preorder(Node* root) {
        vector<int> ans;
        pre(ans, root);
        return ans;
    }
    void pre(vector<int>& ans, Node* root){
        if (!root)
            return;
        ans.push_back(root->val);
        for (int i = 0; i < root->children.size(); ++i)
            pre(ans, root->children[i]);
    }
};
```
N叉树的后序遍历
---
```cpp
class Solution {
public:
    vector<int> postorder(Node* root) {
        vector<int> ans;
        post(ans, root);
        return ans;
    }
    void post(vector<int>& ans, Node* root){
        if (!root)
            return;
        for (int i = 0; i < root->children.size(); ++i)
            post(ans, root->children[i]);
        ans.push_back(root->val);
    }
};
```
N叉树的层序遍历
---
```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>> ans;
        if (!root)
            return ans;
        queue<Node*> que;
        que.push(root);
        while (!que.empty())
        {
            int n = que.size();
            vector<int> anstmp;
            for (int i = 0; i < n; ++i)
            {
                Node* tmp = que.front();
                que.pop();
                anstmp.push_back(tmp->val);
                for (int j = 0; j < tmp->children.size(); ++j)
                    que.push(tmp->children[j]);
            }
            ans.push_back(anstmp);
        }
        return ans;
    }
};
```
