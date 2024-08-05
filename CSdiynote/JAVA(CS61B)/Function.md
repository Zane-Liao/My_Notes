`System.arraycopy()` 是 Java 标准库中用于数组复制的一个方法。它提供了一种高效的方法来复制一个数组的部分或全部内容到另一个数组中。该方法是本地方法，通常比手动复制数组元素更快。

### 方法签名
```java
public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)
```
### 参数
- `src`：源数组。
- `srcPos`：源数组中的起始位置。
- `dest`：目标数组。
- `destPos`：目标数组中的起始位置。
- `length`：要复制的元素数目。
