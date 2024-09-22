作为一份设计文档，它包含了实现原理和实现方法，还有将其他数据结构与此实现结构作为比较（运行时间）。


get()查找 `HashMap` 通过key的hashcode()找到对应的index，然后遍历该位置的链表或树，找到匹配的key并返回对应的value。
put()插入 `HashMap` 通过key的hashcode()找到对应的index，然后查看该index是否已经有元素。如果没有，直接插入；如果有碰撞，会将新的键值对插入到对应链表或树中。
keySet() 这里的 `keySet()` 方法声明是用于返回一个包含当前 `HashMap` 所有键的 `Set` 视图。你可能需要实现它而不是直接返回 `null`

--------

其目的是构建一个自定义的哈希表类，类似于 Java 内置的 `HashMap`，并通过特定规则优化性能和空间使用。

### 总结要点：

1. **构造函数**：
    
    - `MyHashMap()`：无参构造，使用默认初始大小（`16`）和负载因子（`0.75`）。
    - `MyHashMap(int initialSize)`：指定初始桶数的构造函数，负载因子仍为默认值。
    - `MyHashMap(int initialSize, double loadFactor)`：同时指定初始桶数和负载因子。
2. **负载因子**：
    
    - 当负载因子（`loadFactor = N/M`，即元素数量与桶数量的比值）超过指定值时，需要扩容。扩容时应成倍增加哈希表大小，而不是线性增长。
3. **冲突处理**：
    
    - 使用**单独链表法（Separate Chaining）**处理冲突，允许多个元素存储在同一个桶中，通过链表或类似数据结构（如 `ArrayList` 或 `HashSet`）来处理哈希碰撞。
4. **存储桶类型**：
    
    - 必须使用 `ArrayList`、`LinkedList` 或 `HashSet` 来存储桶中的元素，并且只能使用 `Collection` 接口中的方法（如 `add`、`remove` 和 `iterator`）。
5. **恒定摊销时间**：
    
    - 所有操作应当是恒定的摊销时间，假设 `hashCode` 能均匀分布。
    - 注意：`hashCode` 可以返回负值，代码应当处理这个问题。
6. **键的重复处理**：
    
    - 如果插入了重复的键，`MyHashMap` 应该更新该键对应的值。

### 设计的目的：

- **效率**：通过适当的负载因子控制和指数扩容机制，确保哈希表在使用过程中保持高效，避免过多冲突影响查找速度。
- **灵活性**：支持自定义初始桶数和负载因子，用户可以根据需求调整 `MyHashMap` 的空间和时间性能。
- **正确性**：通过冲突处理、键值更新和扩容机制，确保哈希表在各种操作下能正确存储和更新数据。

---------

HashMap Representation with Key-Value Pairs:

+----------------------+     +--------------------------+     +--------------------------+
|      Bucket 0        | --> |       Node               | --> |       Node               |
|                      |     |   [key1, value1]         |     |   [key5, value5]         |
|                      |     +--------------------------+     +--------------------------+
|                      |
+----------------------+
|      Bucket 1        | --> null
+----------------------+
|      Bucket 2        | --> |       Node               | --> null
|                      |     |   [key2, value2]         |
+----------------------+     +--------------------------+
|      Bucket 3        | --> null
+----------------------+
|      Bucket 4        | --> |       Node               | --> |       Node               |
|                      |     |   [key3, value3]         |     |   [key6, value6]         |
|                      |     +--------------------------+     +--------------------------+
+----------------------+
|      Bucket 5        | --> |       Node               | --> null
|                      |     |   [key4, value4]         |
+----------------------+     +--------------------------+

--------

这段文字描述了对 `ComplexOomage` 类的 `hashCode` 方法进行评估和测试的需求。总结如下：

### 需求
1. **理解 `ComplexOomage` 类的结构**：
   - `ComplexOomage` 是一个更复杂的类，与简单的 `Oomage` 类不同，它包含一个整数列表而不是三个固定的颜色值（`red`、`green` 和 `blue`）。
   - 整数列表的长度和内容可以变化，且每个整数都在 0 到 255 之间。

2. **不修改 `ComplexOomage` 类**：
   - 在这次任务中，不需要修改 `ComplexOomage` 类或其 `hashCode` 方法。任务的重点在于测试和评估其 `hashCode` 方法的表现。

3. **测试 `hashCode` 方法的缺陷**：
   - 编写测试用例，专门查找 `ComplexOomage` 的 `hashCode` 函数中潜在的缺陷，特别是在对象分布不均的问题上。
   - 使用 `randomComplexOomage` 方法生成随机的 `ComplexOomage` 对象，并通过可视化工具和测试用例检查 `hashCode` 的分布情况。

4. **评估 `hashCode` 的分布性能**：
   - 通过 `testRandomOomagesHashCodeSpread` 测试和可视化工具检查随机生成的 `ComplexOomage` 对象的 `hashCode` 分布情况。
   - 对于随机对象，`hashCode` 的表现可能是良好的（通过测试）。

5. **针对特殊情况编写测试（testWithDeadlyParams）**：
   - 编写 `testWithDeadlyParams` 测试，设计一组特定的 `ComplexOomage` 对象，使得它们在 `hashCode` 的计算中表现出明显的分布不均问题，从而导致哈希冲突和性能问题。
   - 该测试要求利用二进制表示整数的知识来构造测试数据，以暴露 `hashCode` 方法的缺陷。

6. **评估测试结果**：
   - 运行 `testWithDeadlyParams`，如果 `ComplexOomage` 的 `hashCode` 方法在特定数据上分布不均匀并导致测试失败，说明该方法存在问题。

7. **修复（可选）**：
   - 如果找到了 `hashCode` 方法的缺陷，可以尝试修改 `ComplexOomage` 的 `hashCode` 实现，使 `testWithDeadlyParams` 通过测试。
   - 需要考虑是否还存在其他可能导致 `hashCode` 分布不良的参数组合。

### 目标
- 找出 `ComplexOomage` 的 `hashCode` 方法在特殊情况下可能出现的分布不均问题。
- 编写针对这些问题的测试用例，验证 `hashCode` 方法是否能够有效地将对象分布在哈希表中。
- 最终确认 `ComplexOomage` 类的 `hashCode` 方法在所有情况下都能表现良好，或者指出其缺陷并进行改进。