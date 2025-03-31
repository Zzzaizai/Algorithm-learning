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
根据中序排列与后序排列构建二叉树

后序排列中最后一个数为根节点，倒数第二个数为根节点的右孩子，倒数第三个数为根节点的右孩子的右孩子……以此类推。需定义一个全局指针，指向后序排列的最后一个数（当前节点的值）并一步步前移，先构造根节点，然后递归构造右孩子，最后递归构造左孩子。

中序排列中，根节点的左半为左子树的中序排列，右半为右子树的中序排列，故应在递归过程中传入中序排列的开始与结束位置，进而求得下一个左子树与右子树的中序排列……以此类推。

当传入的中序排列起始位置大于中止位置时，说明该节点为空，直接返回nullptr。
```cpp
class Solution {
public:
    int post_right;
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        int n = inorder.size();
        post_right = n - 1;
        TreeNode* root = build(0, n - 1, inorder, postorder);
        return root;
    }
    TreeNode* build(int in_left, int in_right, vector<int>& inorder, vector<int>& postorder){
        if (in_left > in_right)
            return nullptr;
        int rootval = postorder[post_right];
        post_right--;
        int i;
        for (i = in_left; i < in_right; ++i)
            if (inorder[i] == rootval)
                break;
        TreeNode* root = new TreeNode(rootval);
        root->right = build(i + 1, in_right, inorder, postorder);
        root->left = build(in_left, i - 1, inorder, postorder);
        return root;
    }
};
```
---
根据前序排列与中序排列构造二叉树。

原理与上面相同，前序排列中第一个数为根节点的值，指针应从preorder的开头一个个遍历。

再将中序排列中左右子树分开即可。递归时应先递归左子树，最后递归右子树。
```cpp
class Solution {
public:
    int pre_left;
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = inorder.size();
        pre_left = 0;
        TreeNode* root = build(0, n - 1, preorder, inorder);
        return root;
    }
    TreeNode* build(int in_left, int in_right, vector<int>& preorder, vector<int>& inorder){
        if (in_left > in_right)
            return nullptr;
        int rootval = preorder[pre_left];
        pre_left++;
        int i;
        for (i = in_left; i < in_right; ++i)
            if (inorder[i] == rootval)
                break;
        TreeNode* root = new TreeNode(rootval);
        root->left = build(in_left, i - 1, preorder, inorder);
        root->right = build(i + 1, in_right, preorder, inorder);
        return root;
    }
};
```
---
将每一个节点中的next指向其右边相邻的节点。

层序遍历，使用队列遍历每一层，遍历个数为队列的节点数，在遍历时队列会push进下一层的节点，所以应提前求出队列的size，当节点是这一层最后一个节点时，next指向nullptr，不是的话先弹出，再指向队列的front。

装入下一层节点时，一定要先判断node->left和node->right是否存在，不然会装入nullptr，导致下一次循环时调用node->next报错。

无论是否为满二叉树，均可达到要求。
```cpp
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;
    Node() : val(0), left(NULL), right(NULL), next(NULL) {}
    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}
    Node(int _val, Node* _left, Node* _right, Node* _next) : val(_val), left(_left), right(_right), next(_next) {}
};
class Solution {
public:
    Node* connect(Node* root) {
        if (!root)
            return nullptr;
        queue<Node*> que;
        que.push(root);
        root->next = nullptr;
        while (!que.empty())
        {
            int n = que.size();
            for (int i = 0; i < n; ++i)
            {
                Node* node = que.front();
                que.pop();
                if (i == n - 1)
                    node->next = nullptr;
                else
                    node->next = que.front();
                if (node->left)
                    que.push(node->left);
                if (node->right)
                    que.push(node->right);
            }
        }
        return root;
    }
};
```
