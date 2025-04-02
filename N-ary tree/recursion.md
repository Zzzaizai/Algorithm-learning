求N叉树的最大深度
---
与二叉树类似，采用递归的思路求解，每递归一次子树，深度加1。

在二叉树中，可直接返回max(1 + maxDpeth(root->left), 1 + maxDepth(root->right))，但在N叉树中，需要先定义一个最大值max = 1，然后遍历当前节点的子节点，并更新max，最后返回max。

```cpp
class Solution {
public:
    int maxDepth(Node* root) {
        if (!root)
            return 0;
        int n = root->children.size();
        int max = 1;
        for (int i = 0; i < n; ++i)
        {
            int temp = 1 + maxDepth(root->children[i]);
            if (temp > max)
                max = temp;
        }
        return max;
    }
};
```
