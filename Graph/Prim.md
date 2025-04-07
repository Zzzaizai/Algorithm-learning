Prim算法步骤
---
Prim算法用于求解加权无向图的最小生成树。

使用两个集合存储已访问和未访问的顶点，遍历当前子树与未访问顶点之间的所有边，选取权重最小的边。

贪心思想，将已访问的顶点作为一个整体，找其与外界的最短路径。可用切分定理证明。

时间复杂度：普通二叉堆O(ElogV)，斐波那契堆O(E+VlogV)，空间复杂度O(V)，V为顶点数，E为边数。

连接所有点的最小费用
---
[Leetcode #1584](https://leetcode.cn/problems/min-cost-to-connect-all-points/)

使用集合分别存储已访问和未访问的节点，并且将每条路径的费用按升序排列，存储为(顶点1，顶点2，费用)向量。

循环调用findMinRoad函数，找已访问点与未访问点之间最小费用的路径，返回最小费用到达的顶点编号，然后更新该顶点在集合中的状态。

当未访问点为空时，所有顶点均已访问，此时所累积的路径就是最小生成树，返回总费用即可。
```cpp
class Solution {
public:
    unordered_set<int> visited;
    unordered_set<int> unvisited;
    vector<vector<int>> cost;
    int ans = 0;
    int minCostConnectPoints(vector<vector<int>>& points) {
        int n = points.size();
        if (n == 1)
            return 0;
        for (int i = 0; i < n; ++i)
            unvisited.insert(i);
        visited.insert(0);
        unvisited.erase(0);
        for (int i = 0; i < n; ++i)
        {
            for (int j = i + 1; j < n; ++j)
            {
                vector<int> tempCost(3);
                tempCost[0] = i;
                tempCost[1] = j;
                tempCost[2] = abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1]);
                cost.push_back(tempCost);
            }
        }
        sort(cost.begin(), cost.end(), [](const vector<int>& a, const vector<int>& b){return a[2] < b[2];});
        while (!unvisited.empty())
        {
            int temp = findMinRoad();
            unvisited.erase(temp);
            visited.insert(temp);
        }
        return ans;
    }
    int findMinRoad(){
        int n = cost.size();
        for (int i = 0; i < n; ++i)
        {
            int dot1 = cost[i][0];
            int dot2 = cost[i][1];
            int c = cost[i][2];
            if ((visited.find(dot1) != visited.end() && unvisited.find(dot2) != unvisited.end()) || (visited.find(dot2) != visited.end() && unvisited.find(dot1) != unvisited.end()))
            {
                ans += c;
                if (visited.find(dot1) != visited.end())
                    return dot2;
                else
                    return dot1;
            }
        }
        return -1;
    }
};
```
