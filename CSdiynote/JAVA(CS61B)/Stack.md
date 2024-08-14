`Stack` 是 Java 中的一种数据结构类，位于 `java.util` 包中。它实现了后进先出（LIFO, Last-In-First-Out）的顺序，也就是说最后一个被添加到堆栈中的元素是第一个被移除的元素。

### 1. Stack 类的基本方法
`Stack` 类继承自 `Vector` 类，并提供了一些用于堆栈操作的方法：

- **`push(E item)`**: 将元素推入堆栈顶部。这个方法将元素添加到 `Stack` 的顶部，并返回该元素。
  
- **`pop()`**: 移除并返回堆栈顶部的元素。如果堆栈为空，这个方法会抛出 `EmptyStackException`。

- **`peek()`**: 返回堆栈顶部的元素，但不移除它。与 `pop()` 不同的是，`peek()` 只是查看而不移除。

- **`empty()`**: 检查堆栈是否为空。如果堆栈为空，则返回 `true`，否则返回 `false`。

- **`search(Object o)`**: 返回对象在堆栈中的位置。该方法从堆栈的顶部开始搜索，返回 1 表示对象位于栈顶，-1 表示对象不在栈中。

### 2. 使用示例
```java
import java.util.Stack;

public class StackExample {
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();

        // 向栈中压入元素
        stack.push(1);
        stack.push(2);
        stack.push(3);

        // 查看栈顶元素
        System.out.println("Peek: " + stack.peek());  // 输出: Peek: 3

        // 弹出栈顶元素
        System.out.println("Pop: " + stack.pop());    // 输出: Pop: 3

        // 查看栈是否为空
        System.out.println("Is Empty: " + stack.empty());  // 输出: Is Empty: false

        // 搜索元素的位置
        System.out.println("Position of 1: " + stack.search(1));  // 输出: Position of 1: 2
    }
}
```

### 3. 注意事项
虽然 `Stack` 类可以方便地用于处理简单的堆栈操作，但由于它继承自 `Vector` 类，在并发编程中可能会有一些性能问题。此外，Java 推荐使用 `Deque` 接口及其实现类（如 `ArrayDeque`）来代替 `Stack`，因为 `Deque` 提供了更丰富的功能，并且可以更好地支持并发操作。