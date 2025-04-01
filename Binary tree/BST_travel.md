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
---
实现二叉搜索树迭代器，按中序遍历BST，hasNext函数返回是否有下一个节点，next函数返回下一个节点的val。

使用队列存储中序遍历的BST，需要有一个虚拟的队列头，调用hasNext时判断队列大小是否大于1，调用next时首先pop出第一个节点，此时front就是需要返回的节点。
```cpp
class BSTIterator {
public:
    queue<TreeNode*> que;
    BSTIterator(TreeNode* root) {
        TreeNode* head = new TreeNode(0);
        que.push(head);
        helper(root);
    }
    void helper(TreeNode* root){
        if (!root)
            return;
        helper(root->left);
        que.push(root);
        helper(root->right);
    }
    int next() {
        que.pop();
        if (que.front() == nullptr)
            return 0;
        int ans = que.front()->val;
        return ans;
    }
    
    bool hasNext() {
        if (que.size() > 1)
            return true;
        return false;
    }
};
```
找寻二叉搜索树的最近公共祖先。

利用二叉搜索树的性质，若两节点的值都大于或小于根节点值，则将当前节点的右子树或左子树作为根节点递归，否则，当前节点就是最近公共祖先，直接返回。
```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (p->val > root->val && q->val > root->val)
            return lowestCommonAncestor(root->right, p, q);
        else if (p->val < root->val && q->val < root->val)
            return lowestCommonAncestor(root->left, p, q);
        else
            return root;

    }
};
```
