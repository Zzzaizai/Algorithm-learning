定义
---
堆并不是优先队列，优先队列是一种抽象的数据类型，而堆是一种数据结构，堆是实现优先队列的一种方式。

堆可以使优先队列的插入删除操作在O(logN)的时间内完成。

完全二叉树且每个节点的值都大于等于或小于等于其孩子节点值的树称为堆。分别对应最大堆和最小堆。完全二叉树要求除了最后一层外，节点是满的，最后一层节点从左向右排布。区别于平衡二叉树和二叉搜索树。

插入、删除及堆的实现
---
堆的插入过程是指在堆中插入一个节点，主要分为两步，首先使插入节点后仍满足完全二叉树，之后需交换节点以满足每个节点的值大于等于或小于等于其孩子节点的值。

堆的删除过程指的是删除堆顶元素，主要分为两部，首先删除堆顶元素并将最后一个元素移至堆顶，之后需交换节点以满足每个节点的值大于等于或小于等于其孩子节点的值。

可以使用数组模拟完全二叉树以实现最大最小堆。数组中index=0不存储节点，index=1为根节点，节点index的父节点为index/2，节点index的左孩子为2*index，右孩子为2*index+1，当index>n/2时表示叶子节点。
```cpp
class MaxHeap {
private:
    vector<int> heap;

    // 向上调整
    void siftUp(int index) {
        while (index > 1) {
            int parent = index / 2;
            if (heap[index] > heap[parent]) {
                swap(heap[index], heap[parent]);
                index = parent;
            } else break;
        }
    }

    // 向下调整
    void siftDown(int index) {
        int size = heap[0];
        while (2 * index <= size) {
            int left = 2 * index;
            int right = 2 * index + 1;
            int largest = index;

            if (left <= size && heap[left] > heap[largest]) largest = left;
            if (right <= size && heap[right] > heap[largest]) largest = right;

            if (largest != index) {
                swap(heap[index], heap[largest]);
                index = largest;
            } else break;
        }
    }

public:
    MaxHeap() {
        heap.push_back(0); // heap[0] 表示当前元素个数，初始化为0
    }

    // 插入元素
    void insert(int val) {
        heap.push_back(val);
        heap[0]++; // 元素个数 +1
        siftUp(heap[0]);
    }

    // 获取最大值（堆顶）
    int getMax() {
        if (heap[0] == 0) throw runtime_error("Heap is empty");
        return heap[1];
    }

    // 删除最大值
    void extractMax() {
        if (heap[0] == 0) throw runtime_error("Heap is empty");
        heap[1] = heap[heap[0]]; // 最后一个元素提到堆顶
        heap.pop_back();         // 删除最后元素
        heap[0]--;               // 元素个数 -1
        siftDown(1);             // 从根向下调整
    }

    // 打印堆内容（跳过heap[0]）
    void printHeap() {
        cout << "堆内容: ";
        for (int i = 1; i <= heap[0]; i++) {
            cout << heap[i] << " ";
        }
        cout << endl;
    }

    // 返回当前堆大小
    int size() {
        return heap[0];
    }
};

int main() {
    MaxHeap h;

    h.insert(45);
    h.insert(20);
    h.insert(35);
    h.insert(50);
    h.insert(40);

    h.printHeap();
    cout << "当前最大值: " << h.getMax() << endl;

    h.extractMax();
    cout << "删除最大值后: ";
    h.printHeap();

    return 0;
}

```
