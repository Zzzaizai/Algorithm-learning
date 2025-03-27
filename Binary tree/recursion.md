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
递归算法，自顶向底遍历找到树的最大深度。

findMaxDepth函数用于寻找当前节点为根节点的树的深度，depth为当前节点的深度，每次寻找都从ans和depth中更新一次最大值，最后查找到叶结束递归，此时ans就是最大深度

边界情况：若输入为null，则ans为初始值0，且不与depth=1共同取最大值，返回0；若输入为一个节点，则在findMaxDepth函数中ans取depth的值，并返回1。
```cpp
class Solution {
public:
    int ans = 0;
    int maxDepth(TreeNode* root) {
        int depth = 1;
        findMaxDepth(root, depth);
        return ans;
    }
    void findMaxDepth(TreeNode* root, int depth){
        if (!root)
            return;
        ans = max(ans, depth);
        findMaxDepth(root->left, depth + 1);
        findMaxDepth(root->right, depth + 1);
    }
};
```
---
递归算法，自顶向底判断二叉树是否对称。

Symmetric函数首先判断传入的两节点root->left和root->right是否轴对称，并继续判断以root->left为根节点的树是否对称于以root->right为根节点的树，此时需判断左节点的右节点与右节点的左节点是否一样，左节点的左节点与右节点的右节点是否一样。

边界条件为root为空时，看作轴对称返回true。

```cpp
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (!root)
            return true;
        return Symmetric(root->left, root->right);
    }
    bool Symmetric(TreeNode* l, TreeNode* r){
        if (l == nullptr && r == nullptr)
            return true;
        else if (l == nullptr || r == nullptr)
            return false;
        if (l->val == r->val)
            return Symmetric(l->left, r->right) && Symmetric(l->right, r->left);
        return false;
    }
};
```
---
递归算法判断是否存在根到叶的一条路径，其val相加为targetSum。

确定边界条件，即root若为nullptr，直接返回false。

确定递归结束条件，即当前节点为叶，且叶的val等于targetSum，返回true。

每次递归都将targetSum减去当前节点的val。
```cpp
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if (!root)
            return false;
        if (root->left == nullptr && root->right == nullptr && targetSum == root->val)
            return true;
        return hasPathSum(root->left, targetSum - root->val) || hasPathSum(root->right, targetSum - root->val);
    }
};
```
