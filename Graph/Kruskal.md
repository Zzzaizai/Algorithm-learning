Kruskal算法步骤
---
Kruskal算法用于求解加权无向图的最小生成树。

首先将所有边从小到大排序，然后依次加入到最小生成树中，若形成环则跳过该边，直到加入了N-1条边位置，N为节点个数。

贪心思想，每次选择不形成环的最小边，

时间复杂度O(ElogE)，E为边数，空间复杂度O(V)，V为顶点数。

连接所有点的最小费用
---
[Leetcode #1584](https://leetcode.cn/problems/min-cost-to-connect-all-points/)

points表示一些点，其两点之间的费用为两点的曼哈顿距离，求所有点连接起来的最小费用。

其本质为求加权无向图的最小生成树，采用Kruskal算法，维护边。

首先遍历所有点，求出所有可能的边及其费用，存入road中，并对其按费用排序。road[i][0]和road[i][1]表示第i条边连接的两个节点，road[i][2]表示该边的费用。

之后按费用从小到大依次将边插入，若插入新边会导致子图中出现环（用isCircle函数与并查集进行判断），则不插入。

最小生成树的边数总是为节点数减1，因此边数达到该数即可返回总费用，不必遍历后序边。
```cpp
class Solution {
public:
    vector<int> parent; // 并查集父节点数组
    int minCostConnectPoints(vector<vector<int>>& points) {
        vector<vector<int>> road;       // 起点、终点、权重组成的向量
        int n = points.size();
        if (n == 1)
            return 0;
        // 初始化并查集
        parent.resize(n);
        for (int i = 0; i < n; ++i) {
            parent[i] = i;
        }

        for (int i = 0; i < n; ++i)
        {
            for (int j = i + 1; j < n; ++j)
            {
                vector<int> temp(3);
                temp[0] = i;
                temp[1] = j;
                temp[2] = abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1]);
                road.push_back(temp);
            }
        }
        sort(road.begin(), road.end(), [](const vector<int>& a, const vector<int>& b){return a[2] < b[2];});
        int ans = 0;
        int roadNum = 0;
        for (int i = 0; i < road.size(); ++i)
        {
            if (isCircle(pair<int, int>(road[i][0], road[i][1])))
                continue;
            else
            {
                ans += road[i][2];
                roadNum++;
            }
            if (roadNum == n - 1)
                break;
        }
        return ans;
    }
    bool isCircle(pair<int, int> p){
        int rootU = find(p.first);
        int rootV = find(p.second);
        if (rootU == rootV) {
            return true; // 同一集合，会形成环
        }
        parent[rootV] = rootU; // 合并集合
        return false;
    }
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }
};
```
