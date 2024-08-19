Trie（发音为“try”）是一种树形数据结构，主要用于高效地存储和检索字符串，特别是在需要快速查找前缀匹配的场景中。它也被称为前缀树或字典树。

### Trie的基本结构
- **节点（Node）**：Trie的每个节点代表一个字符。每个节点可以有多个子节点，分别代表不同的字符。
- **根节点（Root）**：Trie的根节点通常是空的，作为整个树的起始点。
- **边（Edge）**：从一个节点到另一个节点的连接代表从一个字符到下一个字符的转换。
- **终止标志（End of Word）**：在Trie中，一个单词的最后一个字符对应的节点通常会有一个标志（如`isEnd`），表示这个节点是一个完整单词的结尾。

### Trie的作用和用途

1. **快速前缀查询**：
   Trie最主要的作用是快速前缀匹配。给定一个前缀，可以在Trie中迅速找到以这个前缀开头的所有单词。因为每个节点对应一个字符，因此查找前缀的时间复杂度与前缀的长度成线性关系。

   - **示例**：如果Trie中存储了“cat”、“car”、“cap”，并且你输入前缀“ca”，Trie会迅速找到所有以“ca”开头的单词。

2. **单词查找与验证**：
   Trie可以用来验证某个单词是否存在于一个已知的字典或词库中。因为Trie按照字符逐层构建，每层都可以快速决定是否有必要继续查找，查找速度非常快。

   - **示例**：想要查找单词“cat”是否在字典中，Trie会依次查找“c”、“a”、“t”三个字符，如果都存在并且“t”是一个单词的终止标志，则单词存在。

3. **自动补全**：
   许多搜索引擎和文本输入法使用Trie来实现自动补全功能。当用户输入一部分字符时，Trie可以快速找到所有以这些字符为前缀的单词，并将其作为建议返回给用户。

   - **示例**：用户输入“aut”，系统会建议“auto”、“automatic”、“automobile”等以“aut”开头的单词。

4. **最长公共前缀**：
   Trie可以用来找到一组字符串的最长公共前缀。通过将所有字符串插入Trie，然后从根节点出发，遍历到每个节点只拥有一个子节点的路径即可找到最长公共前缀。

   - **示例**：给定字符串“flower”、“flow”、“flight”，它们的最长公共前缀是“fl”。

5. **字典序排序**：
   因为Trie的结构与字典序相吻合，从根节点开始的深度优先遍历可以产生所有单词的字典序排列。

   - **示例**：如果Trie中存储了“bat”、“ball”、“cat”，遍历结果会按字典顺序输出：“ball”、“bat”、“cat”。

### Trie的优缺点

- **优点**：
  - 高效的前缀查找，适用于需要大量前缀匹配操作的应用。
  - 插入和查找操作的时间复杂度为O(m)，其中m是字符串的长度。

- **缺点**：
  - 存储空间较大，因为每个字符都需要独立的节点。
  - 实现和维护相对复杂。

-------------

```
collect():
    Create an empty list of results x
    For character c in root.next.keys():
        Call colHelp(c, x, root.next.get(c))
    Return x

colHelp(String s, List<String> x, Node n):
    if n.isKey:
        x.add(s)
    For character c in n.next.keys():
        Call colHelp(s + c, x, n.next.get(c))
```

```
keysWithPrefix(String s):
    Find the end of the prefix, alpha
    Create an empty list x
    For character in alpha.next.keys():
        Call colHelp("sa" + c, x, alpha.next.get(c))
    Return x
```

这些代码片段是实现Trie（前缀树）中一些重要功能的递归方法。Trie是一种树形数据结构，常用于存储字符串集，特别适合前缀查找操作。以下是对这些函数的解释：

### 1. `collect()`函数
这个函数的作用是从根节点开始，收集Trie中的所有字符串。

- **步骤**：
  1. 创建一个空列表`x`，用于存储结果。
  2. 遍历根节点的所有子节点`c`。
  3. 对于每个子节点`c`，调用`colHelp(c, x, root.next.get(c))`，其中`root.next.get(c)`获取子节点的引用。
  4. 最终返回列表`x`，其中包含了Trie中所有的字符串。

### 2. `colHelp(String s, List<String> x, Node n)`函数
这个辅助函数是`collect()`的核心递归部分，用于深度遍历Trie并收集完整的字符串。

- **参数**：
  - `s`: 当前构建的字符串。
  - `x`: 用于存储结果的列表。
  - `n`: 当前处理的节点。

- **步骤**：
  1. 如果当前节点`n`代表一个完整的字符串（即`n.isKey`为`true`），则将字符串`s`添加到列表`x`中。
  2. 遍历节点`n`的所有子节点`c`，对每个子节点递归调用`colHelp(s + c, x, n.next.get(c))`，其中`s + c`表示将当前字符添加到字符串中。

### 3. `keysWithPrefix(String s)`函数
这个函数的作用是查找以给定前缀` s `开头的所有字符串。

- **步骤**：
  1. 首先在Trie中找到前缀` s `对应的末尾节点`alpha`。
  2. 创建一个空列表`x`，用于存储结果。
  3. 遍历`alpha`节点的所有子节点`c`，调用`colHelp("sa" + c, x, alpha.next.get(c))`，其中`"sa"`是前缀`s`加上当前字符`c`。
  4. 最终返回列表`x`，其中包含了所有以` s `为前缀的字符串。

### 整体功能
- `collect()`：从根节点开始，收集Trie中的所有字符串。
- `colHelp()`：递归遍历Trie树的帮助函数，收集从当前节点开始的所有字符串。
- `keysWithPrefix(String s)`：查找并返回所有以特定前缀` s `开头的字符串。

这些函数通过递归遍历Trie树来实现字符串的收集和查找，利用Trie的结构特性有效地进行前缀查询。

| Data Structure        | Key Type   | Name Lookup Time | Get(x) Time |
| --------------------- | ---------- | ---------------- | ----------- |
| Balanced BST          | Comparable | Θ(log N)         | Θ(log N)    |
| RSC Hash Table        | Hashable   | Θ(1) †           | Θ(1) ∗ †    |
| Data Indexed Array    | Chars      | Θ(1)             | Θ(1)        |
| Tries (BST, HT, DICM) | Strings    | Θ(1)             | Θ(1)        |
**RSC Hash Table**：

- 加入了脚注†，解释哈希表在没有冲突且哈希函数良好的情况下能达到Θ(1)的时间复杂度。
- 加入了脚注∗，指出如果发生大量哈希冲突，最坏情况下时间复杂度会降到Θ(N)。
