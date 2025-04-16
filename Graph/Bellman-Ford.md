算法介绍
---
对于有负权重的有向图，不能使用Dijkstra算法解决，但可使用Bellman-Ford算法计算单源最短路径。也可使用动态规划算法，DP[k][u]=min(DP[k][u], DP[k-1][v]+w(u, v))。

正权环：环的边所有权重相加为正，负权环为负。

定理一：在一个有N个顶点的无环有向图中，两点之间的最短路径最多经过N-1条边；在一个有N个顶点的非负权环图中，两点之间的最短路径最多经过N−1条边。

定理二：负权环没有最短路径。

算法步骤
---
在动态规划的基础上，Bellman-Ford算法每次只取前一行（到达每个顶点最多经过i-1条边的最短路程）计算当前行（到达每个顶点最多经过i条边的最短路程），减少了空间复杂度。

更进一步提升，对所有边遍历N-1次，找寻添加该边后，起点到每个点最短路径，当该次遍历与上次遍历结果相同时即可跳出循环，减少了时间复杂度，但无法保证每次循环都是最多经历i条边。

即对所有的边进行了N-1次松弛操作。

Bellman-Ford算法不能用于包含负权环的图，但可自动检测是否有负权环，即在N-1次循环后进行第N次循环，若路径长度仍减小，则包含负权环。

时间复杂度O(V*E)，空间复杂度O(V)，V为顶点数，E为边数。

但是，当每次循环遍历的边顺序不同时，每次循环结束的结果也不同，可以采用基于队列优化的Bellman-Ford算法，也成为SPFA（Shortest Path Faster Algorithm）。

SPFA
---
使用一个队列，每次遍历队列头顶点相邻的边，若更新最短路径，则将该边指向的顶点添加到队列中，同时记录顶点是否添加到队列中，不可重复添加。当队列为空，则代表已找到起点到所有顶点的最短路径。

K站中转内最便宜的航班
---
[Leetcode #787](https://leetcode.cn/problems/cheapest-flights-within-k-stops/)

n个城市通过航班相连，flight[i]=[fromi, toi, pricei]表示从fromi去往toi花费pricei，找到一条最多经过k站中转的路线使得从src到dst最便宜，返回价格，若不存在返回-1。

运用基础的Bellman-Ford算法，用前一行计算当前行，将最多经过k个顶点等价为最多经过k+1条边，故应循环k+1次。

首先初始化距离向量dist，将起点的dist设为0，其他均为无穷大。

然后进行k+1次循环，每次循环中遍历所有的边flights。当src到当前边起点的最短距离为无穷大时，src到当前边终点的最短距离不变；当src到当前边起点的最短距离为有限值时，src到当前边终点的最短距离由松弛操作决定。

最后当src到dst费用为无穷大则返回-1，否则返回该值。
```cpp
class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        vector<int> dist(n, INT_MAX);
        dist[src] = 0;
        for (int i = 0; i <= k; ++i)
        {
            vector<int> temp = dist;
            for (auto &flight : flights)
            {
                int from = flight[0];
                int to = flight[1];
                int price = flight[2];
                if (dist[from] != INT_MAX)
                {
                    temp[to] = min(temp[to], dist[from] + price);
                }
            }
            dist = temp;
        }
        return dist[dst] == INT_MAX ? -1 : dist[dst];
    }
};
```

