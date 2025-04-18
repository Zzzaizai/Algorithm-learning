cpp中可以用vector实现最大最小堆。默认实现最大堆，最小堆可以在最大堆的基础上将数值取反。
```cpp
#include <vector>
#include <algorithm>

int main(){
  vector<int> heap;
  make_heap(heap.begin(), heap.end());            // 最大堆
  make_heap(v.begin(), v.end(), greater<int>());  // 最小堆
  
   // 插入一个新元素
  heap.push_back(60);
  push_heap(heap.begin(), heap.end());

  // 弹出最大值
  pop_heap(heap.begin(), heap.end());  // 把最大值放最后
  heap.pop_back(); // 真正删除最大值

  // 最终排序
  sort_heap(heap.begin(), heap.end());

  return 0;
}
```
