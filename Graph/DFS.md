找出有向无环图(DAG)中起点到终点所有有可能的路径
---
[Leetcode #797](https://leetcode.cn/problems/all-paths-from-source-to-target/)

int graph[i][j]存图，表示节点i有指向graph[i][j]的有向边。

采用深度优先搜索，递归函数需要传入图、起始节点与结束节点，其中，起始节点随着递归更改。

使用全局变量path存储单个路径，递归中止条件为起始节点等于中止节点，此时将单个路径push进结果中。在for循环里处理以起始节点为起点的路径，首先在path中加入下一个节点，然后以下一个节点为起点进行递归，由于图为DAG，递归结束后的回溯只需将栈顶元素pop出即可。
```cpp
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
        int start = 0;                  // 起点
        int end = graph.size() - 1;     // 终点
        path.push_back(start);
        dfs(graph, start, end);
        return ans;
    }
    void dfs(vector<vector<int>>& graph, int start, int end){
        if (start == end)
        {
            ans.push_back(path);
            return;
        }
        for (int i = 0; i < graph[start].size(); ++i)
        {
            path.push_back(graph[start][i]);
            dfs(graph, graph[start][i], end);
            path.pop_back();            // 回溯
        }
    }
};
```
