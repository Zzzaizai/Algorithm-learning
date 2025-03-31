二叉搜索树BST：每个节点值大于等于左子树所有值，小于等于右子树所有值。

中序遍历可得递增序列.

检查二叉树是否符合二叉搜索树。每次递归都需确定当前节点值的最大最小值，递归左子树时，最小值不变，最大值为当前节点值；递归右子树时，最大值不变，最小值为当前节点值。

假设一直遍历节点的左孩子，且遍历过程均满足二叉搜索树条件，那么valid函数中最大值会一直为当前节点值，且在一直变小。在递归过程中，区间总是在变小的。

```cpp
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if (!root)
            return true;
        return valid(root, LONG_MIN, LONG_MAX);
    }
    bool valid(TreeNode* root, long min, long max){
        if (!root)
            return true;
        if (root->val <= min || root->val >= max)
            return false;
        return valid(root->left, min, root->val) && valid(root->right, root->val, max);
    }
};
```
