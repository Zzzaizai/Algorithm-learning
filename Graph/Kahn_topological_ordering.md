拓扑排序
---
针对有向无环图，对图中所有的顶点按照先后顺序进行的一种线性排序。即若存在顶点u和v，要想到达v，则必须到达u，那么在拓扑排序中，u必须位于v的前面。

拓扑排序中最有名的算法为Kahn算法。

Kahn算法
---
Kahn算法基于BFS，首先计算所有顶点的入度，从入度为0的顶点出发，并将其加入队列。从队列头取元素u，遍历以u为起点的所有边u->v，将v的入度减1，而后将此时入度为0的顶点加入队列中，循环往复，直至所有顶点遍历完。

算法限制：必须满足有向无环图，图中至少有一个顶点入度为0，如果图中所有顶点都有入度，则代表所有顶点都至少有一个前置顶点，那就没有顶点可以作为拓扑排序的起点。

时间复杂度O(V+E)，空间复杂度O(V+E)，V为顶点数，E为边数。

课程表问题
---
[Leetcode #210](https://leetcode.cn/problems/course-schedule-ii/)

共有n门课需要修，prerequisites[i]=[ai, bi]表示选修ai前必需修bi，返回学完所有课程的学习顺序，答案不唯一。

首先将prerequisites列表转化为邻接表，并统计各顶点入度。然后将入度为0的顶点装入队列，每次从队列头取元素cur，先将其装入ans中，判断是否有顶点满足cur->v，若存在就将v的入度减1，并将入度为0的顶点装入队列。一直循环直到队列为空。、

最后需判断ans的长度是否为n，若不是则表示有的课无法修到，返回空数组。
```cpp
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> mat(numCourses);
        vector<int> indegree(numCourses);
        vector<int> ans;
        for (auto &pre : prerequisites)
        {
            int a = pre[0];
            int b = pre[1];
            mat[b].push_back(a);
            indegree[a]++;
        }
        queue<int> que;
        for (int i = 0; i < numCourses; ++i)
        {
            if (indegree[i] == 0)
                que.push(i);
        }
        while (!que.empty())
        {
            int cur = que.front();
            que.pop();
            ans.push_back(cur);
            for (int m : mat[cur])
            {
                indegree[m]--;
                if (indegree[m] == 0)
                    que.push(m);
            }
        }
        if (ans.size() != numCourses)
            return {};
        return ans;
    }
};
```
