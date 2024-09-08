在 Java 中，`Collection` 是 Java 集合框架的核心接口之一。`Collection` 接口定义了一组操作方法，主要用来存储和操作一组对象。它是 `java.util` 包中的一部分，继承 `Iterable` 接口。常见的 `Collection` 子接口和实现类如下：

### 1. **List（列表）**
   `List` 是一个有序的集合，允许重复元素，支持通过索引访问元素。常见实现类有：
   - **ArrayList**：基于数组，支持快速的随机访问，适合频繁读取操作。
   - **LinkedList**：基于双向链表，适合插入和删除操作较多的场景。
   - **Vector**：类似于 `ArrayList`，但线程安全，性能较低。
   - **Stack**：继承自 `Vector`，实现了栈结构，遵循 LIFO（后进先出）原则。

### 2. **Set（集合）**
   `Set` 是一个不允许重复元素的集合，不保证元素的顺序。常见实现类有：
   - **HashSet**：基于哈希表实现，不保证元素顺序，插入和查找的时间复杂度为 O(1)。
   - **LinkedHashSet**：基于哈希表和链表，保证元素插入顺序。
   - **TreeSet**：基于红黑树实现，保证元素的排序，插入和查找的时间复杂度为 O(log n)。

### 3. **Queue（队列）**
   `Queue` 是一种遵循 FIFO（先进先出）原则的集合，适合处理队列模型的数据。常见实现类有：
   - **PriorityQueue**：基于堆实现，支持优先级队列，元素按照优先级排序。
   - **ArrayDeque**：双端队列实现，支持队列和栈操作。
   - **LinkedList**：也实现了 `Queue` 接口，适合用作队列。

### 4. **Deque（双端队列）**
   `Deque` 是 `Queue` 的子接口，支持在两端插入和删除元素。常见实现类有：
   - **ArrayDeque**：高效的双端队列实现。
   - **LinkedList**：也实现了 `Deque` 接口。

### 5. **其他 Collection**
   - **EnumSet**：专门为枚举类型设计的 `Set` 实现，所有元素必须是同一枚举类型的常量。
   
### 常见操作方法
所有实现 `Collection` 接口的类都继承了一组常用方法，例如：
   - `add(E element)`: 添加元素。
   - `remove(Object o)`: 移除元素。
   - `contains(Object o)`: 判断集合中是否包含某个元素。
   - `size()`: 返回集合的大小。
   - `isEmpty()`: 判断集合是否为空。
   - `iterator()`: 返回集合的迭代器。

`Collection` 是 Java 集合框架的基础，你可以基于它处理各种类型的数据集合。