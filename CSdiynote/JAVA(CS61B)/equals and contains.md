在 Java 的 `HashSet` 和类似的集合中，`equals()` 和 `contains()` 方法在对象如何比较和管理方面起着特定的作用。

### `equals()` 方法

- **目的**：`equals()` 方法用于确定两个对象是否被认为是相等的。
- **用法**：当你向 `HashSet` 中添加一个对象时，Java 会使用 `equals()` 方法检查是否已经存在一个与要添加的对象“相等”的对象。
- **自定义**：通过重写 `equals()` 方法，你可以定义对于你的自定义类来说，什么情况下两个实例被认为是相等的。在你的示例中，如果两个 `ColoredNumber` 对象具有相同的 `num` 值，它们就被认为是相等的。

### `contains()` 方法

- **目的**：`contains()` 方法检查集合中是否存在某个特定的对象。
- **用法**：在内部，`contains()` 方法使用 `equals()` 方法将你要查找的对象与集合中的对象进行比较。如果 `equals()` 对集合中的任何对象返回 `true`，则 `contains()` 将返回 `true`，表明该对象确实在集合中。

### 使用你提供的代码举例：

1. **向 `HashSet` 添加对象**：
   ```java
   int N = 20;
   HashSet<ColoredNumber> hs = new HashSet<>();
   for (int i = 0; i < N; i += 1) {
       hs.add(new ColoredNumber(i));
   }
   ```
   - 这里，`20` 个 `ColoredNumber` 实例被添加到 `HashSet` 中。由于每个 `ColoredNumber` 对象都有不同的 `num` 值（从 `0` 到 `19`），每个对象都是唯一的，因此它们都会被添加到集合中。
   
2. **检查是否包含特定对象**：
   ```java
   ColoredNumber cn = new ColoredNumber(5);
   boolean containsFive = hs.contains(cn);
   ```
   - 这段代码检查集合中是否包含一个 `num` 值为 `5` 的 `ColoredNumber` 对象。`contains()` 方法将遍历集合，对于每个对象，它会使用 `equals()` 方法检查 `num` 值是否等于 `5`。
   - 由于你之前添加了一个 `num = 5` 的对象，`contains()` 将返回 `true`。

### 总结

- **`equals()`**：定义两个对象如何被认为是相同的。在 `HashSet` 中，它用于防止重复并查找对象。
- **`contains()`**：检查集合中是否存在特定对象，依赖于 `equals()` 方法来确定匹配项。

如果 `equals()` 方法没有正确实现，`contains()` 方法可能无法按预期工作，导致无法找到应该在集合中的对象，或错误地识别出重复项。

--------------

在 Java 中，对于依赖哈希的集合（如 `HashSet` 或 `HashMap`），`equals()` 方法用于确定两个对象是否被认为是相等的。这对于 `HashSet` 中的 `add()`、`contains()` 和 `remove()` 等操作至关重要。

### `equals()` 方法
`equals()` 方法将当前对象（`this`）与另一个对象（`o`）进行比较，以确定它们是否“相等”。在你的示例中，`equals()` 的实现确保了如果两个 `ColoredNumber` 对象具有相同的 `num` 值，它们就被认为是相等的。

以下是你提供的 `equals()` 方法的解释：

```java
@Override
public boolean equals(Object o) {
   if (o instanceof ColoredNumber otherCn) { // 检查对象是否为 ColoredNumber 实例
       return this.num == otherCn.num; // 比较它们的 num 值
   }
   return false; // 如果不是 ColoredNumber 实例，返回 false
}
```

### `contains()` 方法
`contains()` 方法用于检查一个集合（如 `HashSet`）中是否包含特定的元素。在内部，当调用 `contains()` 时，它会使用 `equals()` 方法将目标元素与集合中已有的元素进行比较。

### 示例分析

给定如下代码：

```java
int N = 20;
HashSet<ColoredNumber> hs = new HashSet<>();
for (int i = 0; i < N; i += 1) {
    hs.add(new ColoredNumber(i));
}
```

1. **添加元素**:  
   当你向 `HashSet` 添加一个 `ColoredNumber` 对象时，`HashSet` 会在内部检查集合中是否已经存在一个等价的对象（基于 `equals()` 方法）。
   - 如果没有，新的对象将被添加。
   - 如果找到了等价的对象（基于 `equals()` 方法），新的对象将不会被添加。

2. **使用 `contains()`**:  
   如果你检查集合中是否包含一个特定 `num` 值的 `ColoredNumber` 对象（如 `hs.contains(new ColoredNumber(someNum));`），`HashSet` 会在内部调用 `equals()` 方法，将新 `ColoredNumber` 的 `num` 值与集合中已有对象的 `num` 值进行比较。

### 重要注意事项
- **哈希码考虑**: 在 `HashSet` 中，`hashCode()` 方法同样重要。通过 `equals()` 方法被认为相等的两个对象也应该具有相同的 `hashCode()`。这是因为 `HashSet` 首先检查哈希码，以快速定位潜在的匹配项，然后使用 `equals()` 确认是否相等。
- 如果没有根据 `equals()` 方法一致地重写 `hashCode()`，你的 `HashSet` 可能会表现得不可靠，可能会允许重复项，或者在使用 `contains()` 时找不到元素。

你想进一步了解如何为 `ColoredNumber` 类重写 `hashCode()` 方法吗？