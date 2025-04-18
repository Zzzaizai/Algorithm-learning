堆排序
---
堆排序指的是利用堆对一组无序元素排序。

最大堆排序步骤（最小堆类似）：
1. 所有元素化成最大堆。
2. 取出并删除堆顶元素，存储至有序数据集T中。
3. 此时，堆会调整为新的最大堆。
4. 重复取出堆顶元素放入T中，直至堆空。

时间复杂度O(NlogN)，空间复杂度O(N)。

Top K
---
Top K指的是获取最大的K个元素或最小的K个元素。

Top K大元素步骤（Top K小类似）：
1. 创建一个最大堆。
2. 所有元素加入最大堆中。
3. 边删除边遍历，将堆顶元素删除，并保存到T中。
4. 重复上一个步骤，直到去除K个元素。

时间复杂度O(KlogN)，空间复杂度O(N)。

另一种解法求Top K大（Top K小类似）：
1. 创建大小为K的最小堆。
2. 依次将元素添加到最小堆中。
3. 当堆满时，将当前元素与堆顶元素对比，如果当前元素小于堆顶元素，则放弃该元素；如果当前元素大于堆顶元素，则删除堆顶元素，将当前元素加入最小堆。
4. 重复上述步骤，直到遍历完所有元素。
5. 此时最小堆中的元素为Top K大元素。

时间复杂度O(NlogK)，空间复杂度O(K)。

The Kth
---
The Kth指的是利用堆获取第K大或第K小的元素。

The Kth大元素步骤（The Kth小类似）：
1. 创建一个最大堆。
2. 将所有元素都加到最大堆中。
3. 边删除边遍历，将堆顶元素删除。
4. 重复上一步K次，获取第K大元素。

时间复杂度O(KlogN)，空间复杂度O(N)。

另一种解法求The Kth大（The Kth小类似）：
1. 创建大小为K的最小堆。
2. 依次将元素添加到最小堆。
3. 当堆满时，将当前元素与堆顶元素比较，若当前元素小于堆顶元素，则放弃该元素；若当前元素大于堆顶元素，则删除堆顶元素，将当前元素加入最小堆。
4. 重复上述步骤，直到所有元素遍历完。
5. 此时堆顶元素为第K大元素。

时间复杂度O(NlogK)，空间复杂度O(K)。

数组中第K个最大元素
---
[Leetcode #215](https://leetcode.cn/problems/kth-largest-element-in-an-array/)

数组nums和整数k，注意需要找排序后的第k个最大元素，而不是第k个不同元素。

使用大小为k的最小堆查找第k大元素。

```cpp
int findKthLargest(vector<int>& nums, int k) {
    vector<int> heap(k);
    for (int i = 0; i < k; ++i)
        heap[i] = nums[i];
    make_heap(heap.begin(), heap.end(), greater<int>());
    for (int i = k; i < nums.size(); ++i)
    {
        if (nums[i] >= heap.front())
        {
            pop_heap(heap.begin(), heap.end(), greater<int>());
            heap.pop_back();
            heap.push_back(nums[i]);
            push_heap(heap.begin(), heap.end(), greater<int>());
        }
    }
    return heap.front();
}
```

数据流中的第K大元素
---
[Leetcode #703](https://leetcode.cn/problems/kth-largest-element-in-a-stream/)

使用优先队列构建大小为k的最小堆，当堆中元素数量不足k时，add函数直接添加val到堆中，并维护最小堆；当堆中元素数量大于k时，若val大于堆顶，则添加进堆并且弹出堆顶，维护最小堆。

这样，每次添加元素后，最小堆的堆顶元素就是第K大的元素。
```cpp
class KthLargest {
public:
    priority_queue<int, vector<int>, greater<int>> heap;
    int kth;
    KthLargest(int k, vector<int>& nums) {
        kth = k;
        for (int num : nums)
            int temp = add(num);
    }
    
    int add(int val) {
        if (kth > heap.size())
            heap.push(val);
        else
        {
            if (val > heap.top())
            {
                heap.pop();
                heap.push(val);
            }
        }
        return heap.top();
    }
};
```

前K个高频元素
---
[Leetcond #347](https://leetcode.cn/problems/top-k-frequent-elements)

返回整数数组nums中出现频率前k高的元素，可按任意顺序。

使用优先队列构造最小堆，堆中的元素类型为[频率，数字]，此时最小堆会按照频率大小排序，当频率相同时按照数字大小排序。

然后使用哈希表存储各数字出现的频率，并依次插入最小堆中，维护最小堆。

最后最小堆中的所有元素就是出现频率前k高的元素，将对应的数字存入向量中返回即可。
```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        using PII = pair<int, int>;
        priority_queue<PII, vector<PII>, greater<PII>> heap;
        unordered_map<int, int> map;
        for (int num : nums)
            map[num]++;
        for (auto &it : map)
        {
            if (heap.size() < k)
                heap.emplace(it.second, it.first);  // 频率，数字
            else
            {
                if (heap.top().first < it.second)
                {
                    heap.pop();
                    heap.emplace(it.second, it.first);
                }
            }
        }
        vector<int> ans;
        while (!heap.empty())
        {
            ans.push_back(heap.top().second);
            heap.pop();
        }
        return ans;
    }
};
```
