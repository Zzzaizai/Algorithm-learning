单源最短路径SSSP
---
在BFS与DFS中，可解决无权图的最短路径问题，但在有权图中，可以通过SSSP解决。

基本算法有两个：Dijkstra和Bellman-Ford。Dijkstra只能解决加权有向图中权重为非负数的单源最短路径问题，Bellman-Ford可以解决加权有向图中权重包含负数的单源最短路径问题。
