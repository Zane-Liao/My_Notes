Java 中的 `PriorityQueue` 是一个实现了优先级队列的数据结构，位于 `java.util` 包中。优先级队列与普通队列的不同之处在于，它按照元素的优先级来排序，而不是按照元素进入队列的顺序。一般来说，`PriorityQueue` 是基于堆（heap）结构实现的，通常是一个最小堆或最大堆。

### 1. 基本概念
`PriorityQueue` 在插入和移除元素时，会按照元素的自然顺序（或者使用自定义的比较器）进行排序。默认情况下，它是一个最小堆，即每次 `poll` 或 `peek` 操作返回的都是优先级最高（值最小）的元素。

### 2. 常用方法
- **`add(E e)` / `offer(E e)`**: 将元素添加到队列中。如果队列受容量限制且已满，`add` 会抛出 `IllegalStateException`，而 `offer` 返回 `false`。

- **`poll()`**: 移除并返回队列头部的元素（即优先级最高的元素），如果队列为空，返回 `null`。

- **`peek()`**: 返回队列头部的元素但不移除它，如果队列为空，返回 `null`。

- **`remove(Object o)`**: 从队列中移除指定的元素。

- **`size()`**: 返回队列中的元素个数。

### 3. 使用示例
```java
import java.util.PriorityQueue;

public class PriorityQueueExample {
    public static void main(String[] args) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();

        pq.offer(10);
        pq.offer(5);
        pq.offer(20);
        pq.offer(1);

        // 输出队列元素的顺序，应该是按优先级（从小到大）排序的
        while (!pq.isEmpty()) {
            System.out.println(pq.poll());  // 输出: 1, 5, 10, 20
        }
    }
}
```

### 4. 内部实现
`PriorityQueue` 的内部实现通常基于堆，而在 CS61B 课程中，堆作为优先级队列的基础数据结构有重要的地位。具体来说：

- **最小堆**: 在最小堆中，每个父节点的值都小于或等于其子节点的值。因此，堆顶元素（即根节点）总是最小的。

- **插入操作**: 将新元素添加到堆的末尾，然后通过“上浮”（bubble up）操作将其移动到正确的位置。

- **删除操作**: 移除堆顶元素后，将堆的最后一个元素移到堆顶，再通过“下沉”（sink down）操作将其移到正确的位置。

### 5. 复杂度
`PriorityQueue` 的操作复杂度如下：
- 插入元素（`add`/`offer`）：O(log n)
- 删除最小元素（`poll`）：O(log n)
- 访问最小元素（`peek`）：O(1)

### 6. 实际应用
优先级队列在很多算法和应用中非常有用，比如：
- **Dijkstra算法**：用于找到单源最短路径。
- **A*算法**：用于路径搜索。
- **任务调度**：根据任务的优先级顺序执行任务。

----------


- `leftChild(k)` = k∗2k∗2
- `rightChild(k)` = k∗2+1k∗2+1
- `parent(k)` = k/2k/2

| Methods          | Ordered Array | Bushy BST | Hash Table | Heap    |
| ---------------- | ------------- | --------- | ---------- | ------- |
| `add`            | Θ(N)          | Θ(logN)   | Θ(1)       | Θ(logN) |
| `getSmallest`    | Θ(1)          | Θ(logN)   | Θ(N)       | Θ(1)    |
| `removeSmallest` | Θ(N)          | Θ(logN)   | Θ(N)       | Θ(logN) |

- 堆操作是**摊销**分析，因为数组必须调整大小（没什么大不了的）
- 如果指针存储到最小元素，BST 可以具有恒定时间`getSmallest`
- 基于数组的堆占用的内存大约是使用方法 1A（指向子级的直接指针）表示堆所需内存的 1/3