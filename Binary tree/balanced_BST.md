普通的二叉树在最坏的情况下可以退化成一条链，对于有N个节点的二叉树，遍历的时间复杂度为O(logN)~O(N)。

对于平衡二叉搜索树，每个节点的两个子树高度相差不超过1，遍历的时间复杂度固定为O(logN)，对操作性能有提升。

常见的平衡二叉树：红黑树、AVL树，伸展树、树堆等。

平衡二叉搜索树通常应用与Set和Map中，cpp中的set是由平衡二叉搜索树实现的，unordered_map是由哈希实现的，但当哈希键过多时，通常使用平衡二叉搜索树将查找的时间复杂度从O(N)降为O(logN)。

判断是否为平衡二叉树
---

注意平衡二叉树的定义为每个节点的两个子树高度相差不超过1。

而treeHeight == log2(nodesNum) + 1并不能判断二叉树是否平衡，其反应的是总体平衡性。

运用递归的方法，判断每个节点的左右子树是否满足深度差小于等于1，子树的深度也使用递归的方法计算。
```cpp
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        if (!root)
            return true;
        if (abs(findheight(root->left) - findheight(root->right)) > 1)
            return false;
        return isBalanced(root->left) && isBalanced(root->right);
    }
    int findheight(TreeNode* root){
        if (!root)
            return 0;
        return max(1 + findheight(root->left), 1 + findheight(root->right));
    }
};
```
有序列表转平衡二叉搜索树
---
使用递归算法，每次递归列表的一半为相应子树的列表，使用辅助函数，每次传入原数组与相应的区间，当右区间小于左区间时，递归结束。
```cpp
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        int n = nums.size();
        TreeNode* root = helper(nums, 0, n - 1);
        return root;
    }
    TreeNode* helper(vector<int>& nums, int left, int right){
        if (right < left)
            return nullptr;
        int mid = (right + left + 1) / 2;
        TreeNode* root = new TreeNode(nums[mid]);
        root->left = helper(nums, left, mid - 1);
        root->right = helper(nums, mid + 1, right);
        return root;
    }
};
```
