广度优先搜索BFS
---
搜索思路：层层遍历，类似于二叉树的层序遍历，BFS中层数可以理解为距离起点的最短距离。

用途：遍历所有顶点、遍历两点之间最短路径。

遍历所有顶点：使用队列存储顶点，时间复杂度O(V+E)，空间复杂第O(V)，V为顶点数，E为边数。

遍历最短路径：使用队列存储路径，第一次遇到终点即为最短路径，时间复杂度O(V+E)，时间复杂度O(V)。

找出有向无环图(DAG)中起点到终点所有有可能的路径
---
[Leetcode #797](https://leetcode.cn/problems/all-paths-from-source-to-target/)

使用队列装载路径。首先遍历队列中的路径（其实情况下有一条初始路径，只包括起点），若路径的终点为end，就将路径push进ans里，否则就找这个路径终点的所有下一个节点，并将其连接后重新放进队列尾部。当队列为空时，表示所有路径都已遍历完成。
```cpp
class Solution {
public:
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
        vector<vector<int>> ans;
        vector<int> path;
        int start = 0;
        int end = graph.size() - 1;
        queue<vector<int>> que;
        path.push_back(start);
        que.push(path);
        while (!que.empty())
        {
            int n = que.size();
            for (int i = 0; i < n; ++i)
            {
                vector<int> temp = que.front();
                que.pop();
                int lastnode = temp.back();
                if (lastnode == end)
                {
                    ans.push_back(temp);
                    continue;
                }
                for (int j = 0; j < graph[lastnode].size(); ++j)
                {
                    vector<int> road = temp;
                    road.push_back(graph[lastnode][j]);
                    que.push(road);
                }
            }
        }
        return ans;
    }
};
```
