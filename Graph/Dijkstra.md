算法步骤
---
以起点为中心，逐步向外扩展，并更新到达其他顶点的最短路径，最后查表得到达某点的最短路径。运用了贪心的思想，每一步都选择最小权重的路径。

贪心算法只保证每一步做最优的选择，但无法保证最终的结果是最优的。但由于算法在每一步中都选择了距起点最短的点为下一步的起始位置，因而也保证了全局的结果是最优的。

Dijkstra算法只能用于非负权重的有向图，因其每次选择的都是距离起点最短的点为下一步开始的位置，即选择的路径一直都是最短路径，若出现负权，就无法保证加上下一条边后，该路径仍是最短的，故不能用于负权图中。

时间复杂度：斐波那契堆实现O(E+VlogV)，空间复杂度O(V)，V为顶点数，E为边数。

网络延迟时间
---
[Leetcode #743](https://leetcode.cn/problems/network-delay-time/)

有n个节点，times[i] = (ui, vi, wi)列表表示源节点，目标节点以及时间，从k节点发出信号，求多久后所有节点都能收到信号，若有的节点收不到则返回-1。

首先将题目所给的列表格式的图转化为邻接矩阵格式mat，并初始化其余数据为无穷大，无穷大为int最大值的一半，因为后续需要两个无穷大相加。定义两节点之间最短距离数组dist，并初始化位置k处的最短距离为0，其余为无穷大。定义判断是否访问过的数组sign，初始化为false。

对每个节点进行遍历。首先找到距离起点最短的节点x，并把sign[x]置true，然后再对dist数组遍历，更新dist值，更新原则为：该节点距离起点的最短距离=min{当前值，x节点距离起点的距离+x到该节点的距离}，若距离不存在则为无穷大。

最后返回dist中的最大值，若最大值为无穷大，则返回-1。
```cpp
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        if (n == 1)
            return 0;
        // 初始化邻接矩阵
        int inf = INT_MAX / 2;
        vector<vector<int>> mat(n, vector<int>(n, inf));
        for (auto t : times)
        {
            int x = t[0] - 1;
            int y = t[1] - 1;;
            mat[x][y] = t[2];
        }

        vector<int> dist(n, inf);
        dist[k - 1] = 0;
        vector<bool> sign(n, false);

        for (int i = 0; i < n; ++i)
        {
            int x = -1;
            for (int y = 0; y < n; ++y)     // 找到距离当前节点最短的节点，作为下一个起点x
            {
                if (!sign[y] && (x == -1 || dist[y] < dist[x]))
                    x = y;
            }
            sign[x] = true;
            for (int y = 0; y < n; ++y)     // 更新所有的最短路径dist
                dist[y] = min(dist[y], dist[x] + mat[x][y]);
        }

        int ans = *max_element(dist.begin(), dist.end());
        return ans == inf ? -1 : ans;
    }
};
```
