```r
nrow(data) # 数据集行数
ncol(data) # 数据集列数
```
### 整理数据
当然！让我详细解释一下 `pivot_longer`、`pivot_wider`、`separate` 和 `unite` 是如何操作数据集的。我们将使用你提供的示例数据来演示每个函数的用法和效果。

### 示例数据集
我们从这个数据集开始：
```r
age_group,2015_time,2015_participants,2016_time,2016_participants
20,3:46,54,3:22,62
30,3:50,60,3:43,58
40,4:39,29,3:49,33
50,4:48,10,4:59,14
```

#### 1. `pivot_longer`
`pivot_longer` 将宽格式数据转换为长格式数据。这意味着它将多个列合并成两列，分别是键和值。

**代码示例：**
```r
library(tidyr)

# 读取数据
dat <- tribble(
  ~age_group, ~`2015_time`, ~`2015_participants`, ~`2016_time`, ~`2016_participants`,
  20, "3:46", 54, "3:22", 62,
  30, "3:50", 60, "3:43", 58,
  40, "4:39", 29, "3:49", 33,
  50, "4:48", 10, "4:59", 14
)

# 使用 pivot_longer 将数据转换为长格式
dat_long <- dat %>%
  pivot_longer(cols = -age_group, names_to = "key", values_to = "value")

print(dat_long)
```

**结果：**
```r
# A tibble: 16 x 3
   age_group key                value
       <dbl> <chr>              <chr>
 1        20 2015_time          3:46 
 2        20 2015_participants  54   
 3        20 2016_time          3:22 
 4        20 2016_participants  62   
 5        30 2015_time          3:50 
 6        30 2015_participants  60   
 7        30 2016_time          3:43 
 8        30 2016_participants  58   
 9        40 2015_time          4:39 
10        40 2015_participants  29   
11        40 2016_time          3:49 
12        40 2016_participants  33   
13        50 2015_time          4:48 
14        50 2015_participants  10   
15        50 2016_time          4:59 
16        50 2016_participants  14   
```

#### 2. `pivot_wider`
`pivot_wider` 将长格式数据转换为宽格式数据。这意味着它将键值对转换为单独的列。

**代码示例：**
```r
# 使用 pivot_wider 将数据转换为宽格式
dat_wide <- dat_long %>%
  separate(col = key, into = c("year", "variable"), sep = "_") %>%
  pivot_wider(names_from = variable, values_from = value)

print(dat_wide)
```

**结果：**
```r
# A tibble: 8 x 4
  age_group year  time  participants
      <dbl> <chr> <chr> <chr>       
1        20 2015  3:46  54          
2        20 2016  3:22  62          
3        30 2015  3:50  60          
4        30 2016  3:43  58          
5        40 2015  4:39  29          
6        40 2016  3:49  33          
7        50 2015  4:48  10          
8        50 2016  4:59  14          
```

#### 3. `separate`
`separate` 将一列拆分为多列。例如，我们可以将包含年份和变量名称的列拆分成两个单独的列。

**代码示例：**
```r
# 使用 separate 将 key 列拆分成 year 和 variable 列
dat_separated <- dat_long %>%
  separate(col = key, into = c("year", "variable"), sep = "_")

print(dat_separated)
```

**结果：**
```r
# A tibble: 16 x 4
   age_group year  variable     value
       <dbl> <chr> <chr>        <chr>
 1        20 2015  time         3:46 
 2        20 2015  participants 54   
 3        20 2016  time         3:22 
 4        20 2016  participants 62   
 5        30 2015  time         3:50 
 6        30 2015  participants 60   
 7        30 2016  time         3:43 
 8        30 2016  participants 58   
 9        40 2015  time         4:39 
10        40 2015  participants 29   
11        40 2016  time         3:49 
12        40 2016  participants 33   
13        50 2015  time         4:48 
14        50 2015  participants 10   
15        50 2016  time         4:59 
16        50 2016  participants 14   
```

#### 4. `unite`
`unite` 将多列合并成一列。例如，我们可以将 `year` 和 `variable` 列合并成一个新的列。

**代码示例：**
```r
# 使用 unite 将 year 和 variable 列合并成一个新的列
dat_united <- dat_separated %>%
  unite("key", year, variable, sep = "_")

print(dat_united)
```

**结果：**
```r
# A tibble: 16 x 3
   age_group key                value
       <dbl> <chr>              <chr>
 1        20 2015_time          3:46 
 2        20 2015_participants  54   
 3        20 2016_time          3:22 
 4        20 2016_participants  62   
 5        30 2015_time          3:50 
 6        30 2015_participants  60   
 7        30 2016_time          3:43 
 8        30 2016_participants  58   
 9        40 2015_time          4:39 
10        40 2015_participants  29   
11        40 2016_time          3:49 
12        40 2016_participants  33   
13        50 2015_time          4:48 
14        50 2015_participants  10   
15        50 2016_time          4:59 
16        50 2016_participants  14   
```

### 综合示例
结合所有这些函数，我们可以完整地演示如何将原始数据转换为你所需的格式：

**原始数据转换为整洁数据的完整示例：**
```r
library(tidyverse)

# 读取数据
dat <- tribble(
  ~age_group, ~`2015_time`, ~`2015_participants`, ~`2016_time`, ~`2016_participants`,
  20, "3:46", 54, "3:22", 62,
  30, "3:50", 60, "3:43", 58,
  40, "4:39", 29, "3:49", 33,
  50, "4:48", 10, "4:59", 14
)

# 转换数据为长格式，拆分列，再转换回宽格式
dat_tidy <- dat %>%
  pivot_longer(cols = -age_group, names_to = "key", values_to = "value") %>%
  separate(col = key, into = c("year", "variable"), sep = "_") %>%
  pivot_wider(names_from = variable, values_from = value)

print(dat_tidy)
```

通过这些步骤，我们能够有效地将数据整理为所需的格式，便于后续分析和处理。

-----

- dplyr 包中的 `join` 函数组合两个表，以便匹配的行放在一起。
- `left_join()` 仅保留第一个表中包含信息的行。
- `right_join()` 仅保留第二个表中包含信息的行。
- `inner_join()` 仅保留两个表中都有信息的行。
- `full_join()` 保留两个表中的所有行。
- `semi_join()` 保留第一个表中我们在第二个表中拥有信息的部分。
- `anti_join()` 保留第一个表中第二个表中没有信息的元素。
- 与连接函数不同，绑定函数不会尝试通过变量进行匹配，而只是组合数据集。
- `bind_cols()` 通过使两个对象成为 tibble 中的列来绑定它们。 R 基函数 `cbind()` 绑定列，但创建数据框或矩阵。
- `bind_rows()` 函数类似，但绑定行而不是列。 R 基函数 `rbind()` 绑定行，但生成数据框或矩阵。
- 默认情况下，R 基中的集合运算符作用于向量。如果加载 tidyverse/dplyr，它们也可以在数据帧上工作。
- 您可以使用 `intersect()` 获取向量的交集。这将返回两个集合共有的元素。
- 您可以使用 `union()` 获取向量的并集。这将返回任一集合中的元素。
- 第一个参数和第二个参数之间的设置差异可以通过 `setdiff()` 获得。请注意，该函数不是对称的。
- 函数 `set_equal()` 告诉我们两个集合是否相同，无论元素的顺序如何。

-----

## String处理

| stringr           | Task       | Description 描述                                                                                                                 | Base R                          |
| ----------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------- |
| `str_detect`      | Detect     | Is the pattern in the string?  <br>模式在字符串中吗？                                                                                   | `grepl`                         |
| `str_which`       | Detect     | Returns the index of entries that contain the pattern.  <br>返回包含该模式的条目的索引。                                                     | `grep`                          |
| `str_subset`      | Detect     | Returns the subset of strings that contain the pattern.  <br>返回包含模式的字符串子集。                                                     | `grep` with   和`value = TRUE`   |
| `str_locate`      | Locate     | Returns positions of first occurrence of the pattern in a string.  <br>返回模式在字符串中第一次出现的位置。                                      | `regexpr`                       |
| `str_locate_all`  | Locate     | Returns position of all occurrences of the pattern in a string.  <br>返回字符串中所有出现该模式的位置。                                         | `gregexpr`                      |
| `str_view`        | Locate     | Show the first part of the string that matches the pattern.  <br>显示与模式匹配的字符串的第一部分。                                             |                                 |
| `str_view_all`    | Locate     | Show all the parts of the string that match the pattern.  <br>显示字符串中与模式匹配的所有部分。                                                |                                 |
| `str_extract`     | Extract    | Extract the first part of the string that matches the pattern.  <br>提取与模式匹配的字符串的第一部分。                                          |                                 |
| `str_extract_all` | Extract    | Extract all parts of the string that match the pattern.  <br>提取字符串中与模式匹配的所有部分。                                                 |                                 |
| `str_match`       | Extract    | Extract first part of the string that matches the pattern and the groups defined by the pattern.  <br>提取与模式和模式定义的组匹配的字符串的第一部分。 |                                 |
| `str_match_all`   | Extract    | Extract all parts of the string that match the pattern and the groups defined by the pattern.  <br>提取字符串中与模式以及模式定义的组匹配的所有部分。   |                                 |
| `str_sub`         | Extract    | Extract a substring. 提取一个子字符串。                                                                                                 | `substring`                     |
| `str_split`       | Extract    | Split a string into a list with parts separated by a pattern.  <br>将字符串拆分为列表，其中各部分由模式分隔。                                       | `strsplit`                      |
| `str_split_fixed` | Extract    | Split a string into a matrix with a fixed number of parts separated by a pattern.  <br>将字符串拆分为矩阵，其中固定数量的部分由模式分隔。               | `strsplit` with  `fixed = TRUE` |
| `str_count`       | Describe   | Count number of times a pattern appears in a string.  <br>计算某个模式在字符串中出现的次数。                                                    |                                 |
| `str_length`      | Describe   | Number of character in string.  <br>字符串中的字符数。                                                                                  | `nchar`                         |
| `str_replace`     | Replace    | Replace first part of a string matching a pattern with another.  <br>将与模式匹配的字符串的第一部分替换为另一个部分。                                  |                                 |
| `str_replace_all` | Replace    | Replace all parts of a string matching a pattern with another.  <br>将字符串中与某个模式匹配的所有部分替换为另一个模式。                                 | `gsub`                          |
| `str_to_upper`    | Replace    | Change all characters to upper case.  <br>将所有字符更改为大写。                                                                          | `toupper`                       |
| `str_to_lower`    | Replace    | Change all characters to lower case.  <br>将所有字符更改为小写。                                                                          | `tolower`                       |
| `str_to_title`    | Replace    | Change first character of each word to upper and rest to lower case.  <br>将每个单词的第一个字符更改为大写，其余字符更改为小写。                          |                                 |
| `str_replace_na`  | Replace    | Replace all `NA`s with a new value.  <br>将所有 s 替换为新值。                                                                          |                                 |
| `str_trim`        | Replace    | Remove white space from start and end of string.  <br>删除字符串开头和结尾的空格。                                                           |                                 |
| `str_c`           | Manipulate | Join multiple strings. 连接多个字符串。                                                                                                | `paste0`                        |
| `str_conv`        | Manipulate | Change the encoding of the string.  <br>更改字符串的编码。                                                                              |                                 |
| `str_sort`        | Manipulate | Sort the vector in alphabetical order.  <br>按字母顺序对向量进行排序。                                                                      | `sort`                          |
| `str_order`       | Manipulate | Provide index needed to order the vector in alphabetical order.  <br>提供按字母顺序对向量进行排序所需的索引。                                      | `order`                         |
| `str_trunc`       | Manipulate | Truncate a string to a fixed size.  <br>将字符串截断为固定大小。                                                                           |                                 |
| `str_pad`         | Manipulate | Add white space to string to make it a fixed size.  <br>向字符串添加空格以使其固定大小。                                                       |                                 |
| `str_dup`         | Manipulate | Repeat a string. 重复一个字符串。                                                                                                      | `rep` then   `paste`            |
| `str_wrap`        | Manipulate | Wrap things into formatted paragraphs.  <br>将内容包装成格式化的段落。                                                                      |                                 |
| `str_interp`      | Manipulate | String interpolation. 字符串插值。                                                                                                   | `sprintf`                       |
- 使用 `str_detect()` 函数来确定字符串是否包含某种模式。
- 使用 `str_replace_all()` 函数将一种模式的所有实例替换为另一种模式。要删除模式，请替换为空字符串 ( `""` )。
- `parse_number()` 函数从字符串中删除标点符号并将其转换为数字。
- `mutate_at()` 对指定的列号执行相同的转换。