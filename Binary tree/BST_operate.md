BST搜索，返回与val相同值的节点及其子树。

在循环中，使用if-elif而不是if-if，否则，若满足第一条if时，ans赋值为其子节点，若该子节点为nullptr，会导致接下来的if语句访问ans->val时出错。
```cpp
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        TreeNode* ans = root;
        while (ans != nullptr)
        {
            if (ans->val > val)
                ans = ans->left;
            else if (ans->val < val)
                ans = ans->right;
            else
                return ans;
        }
        return nullptr;
    }
};
```
---
BST插入，最简单的算法就是将待插入的节点作为叶子插入到树中。

首先判断root是否为空，若是直接返回val节点。

循环内，先判断当前节点是否有空的子节点，且val如果在空节点处满足BST，则直接插入，跳出循环即可。若不满足这两个条件，按照BST的规则移动至下一个节点。

在循环中，前一步的判断子节点若为空但不满足BST，后一步移动节点就不会移动至该空节点；前一步判断子节点若为空且满足BST，则直接break不执行后一步。这可以避免空指针访问节点。
```cpp
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        TreeNode* tmp = root;
        TreeNode* ans = new TreeNode(val);
        if (tmp == nullptr)
            return ans;
        while (tmp != nullptr)
        {
            if (tmp->left == nullptr && tmp->val > val)
            {
                tmp->left = ans;
                break;
            }
            if (tmp->right == nullptr && tmp->val < val)
            {
                tmp->right = ans;
                break;
            }
            if (tmp->val > val)
                tmp = tmp->left;
            else if (tmp->val < val)
                tmp = tmp->right;
        }
        return root;
    }
};
```
---
删除BST节点，最简单的算法为：

1. 若待删除节点为叶，则直接删除。

2. 若待删除的节点有一个子树，则用子树代替该节点。

3. 若待删除的节点有两个子树，将其左子树放在该节点中序遍历后继节点的左子树上，再用该节点的右孩子替换该节点即可。

首先递归找到待删除节点，然后判断是否符合第一二种情况。

若是第三种情况，则遍历找到待删除节点中序遍历的后继节点tmp，将待删除节点的左子树连接到tmp左子树上，最后用待删除节点的右孩子覆盖。

```cpp
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (!root)
            return nullptr;
        if (root->val > key)
            root->left = deleteNode(root->left, key);
        else if (root->val < key)
            root->right = deleteNode(root->right, key);
        else
        {
            if (root->left == nullptr && root->right == nullptr)
                return nullptr;
            if (root->left == nullptr)
                return root->right;
            if (root->right == nullptr)
                return root->left;
            TreeNode* tmp = root->right;
            while (tmp->left != nullptr)
                tmp = tmp->left;
            TreeNode* leftnode = root->left;
            tmp->left = leftnode;
            root = root->right;
        }
        return root;
    }
};
```
