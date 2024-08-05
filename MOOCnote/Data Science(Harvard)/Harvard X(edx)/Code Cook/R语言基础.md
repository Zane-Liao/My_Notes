#### 第 1 节：R 基础知识、函数、数据类型，向量，排序，索引、数据操作、绘图
#### 第 2节：编程基础知识
#### 第3节：tidyverse
#### 第4节： 数据表

---
## 1. R 基础知识、函数、数据类型，向量，排序，索引、数据操作、绘图

- 安装软件包
``` r
install.packages("dslabs") # to install a single package
install.packages(c("tidyverse", "dslabs")) # to install two packages at the
same time installed.packages() # to see the list of all installed packages
```
- 添加数据集
``` r
data(movielens)
```
- 加载包
``` r
library(dslabs)
library(tidyverse)
```
- Code ：求解方程 \(x^2 + x - 1 = 0\)

```r
# assigning values to variables
a <-1
b <-1
c <--1

# solving the quadratic equation
(-b + sqrt(b^2 - 4*a*c))/(2*a)
(-b - sqrt(b^2 - 4*a*c))/(2*a)
```
- 一些常用的内置函数
```r
log(8)
#> [1] 2.08
help("log")
max() # 最大值
min() # 最小值
mean() # 求平均数
sd() # 求方差
str(murders) # 函数 `str` 对于了解有关对象结构的更多信息非常有用
#> 'data.frame': 51 obs. of 5 variables: 
#> $ state : chr "Alabama" "Alaska" "Arizona" "Arkansas" ... 
#> $ abb : chr "AL" "AK" "AZ" "AR" ... 
#> $ region : Factor w/ 4 levels "Northeast","South",..: 2 4 4 2 4 4 1 2 2 
#> 2 ... 
#> $ population: num 4779736 710231 6392017 2915918 37253956 ... 
#> $ total : num 135 19 232 93 1257 ...
head(murders) # 使用函数 `head` 显示前六行数据集
#> [1] 4779736 710231 6392017 2915918 37253956 5029196 3574097 
#> [8] 897934 601723 19687653 9920000 1360301 1567582 12830632 
#> [15] 6483802 3046355 2853118 4339367 4533372 1328361 5773552 
#> [22] 6547629 9883640 5303925 2967297 5988927 989415 1826341 
#> [29] 2700551 1316470 8791894 2059179 19378102 9535483 672591
#> [36] 11536504 3751351 3831074 12702379 1052567 4625364 814180 
#> [43] 6346105 25145561 2763885 625741 8001024 6724540 1852994 
#> [50] 5686986 563626
names(murders) # 快速访问变量名称
#> [1] "state" "abb" "region" "population" "total"
pop <- murders$population # 函数 `length` 告诉您向量中有多少个条目
length(pop)
#> [1] 51
class(pop) # 这个特定的向量是数字
#> [1] "numeric"
i_max <- which.max(murders$total) # `which.max` 为最大值的索引
murders$state[i_max]
#> [1] "California"
```
- 访问器运算符 `$`
``` r
murders$population
#>  [1]  4779736   710231  6392017  2915918 37253956  5029196  3574097
#>  [8]   897934   601723 19687653  9920000  1360301  1567582 12830632
#> [15]  6483802  3046355  2853118  4339367  4533372  1328361  5773552
#> [22]  6547629  9883640  5303925  2967297  5988927   989415  1826341
#> [29]  2700551  1316470  8791894  2059179 19378102  9535483   672591
#> [36] 11536504  3751351  3831074 12702379  1052567  4625364   814180
#> [43]  6346105 25145561  2763885   625741  8001024  6724540  1852994
#> [50]  5686986   563626
```

- 数据框是列表的特例。列表很有用，因为您可以存储不同类型的任意组合。您可以使用 `list` 函数创建一个列表，如下所示：
```r
record <- list(name = "John Doe",
             student_id = 1234,
             grades = c(95, 82, 91, 97, 93),
             final_grade = "A")
```
- 我们可以使用 `matrix` 函数定义一个矩阵。我们需要指定矩阵中的数据以及行数和列数
```r
mat <- matrix(1:12, 4, 3)
mat
#>      [,1] [,2] [,3]
#> [1,]    1    5    9
#> [2,]    2    6   10
#> [3,]    3    7   11
#> [4,]    4    8   12
```
- 我们可以使用函数 `c` 创建向量，它代表连接。我们使用 `c` 按以下方式连接条目
```r
codes <- c(380, 124, 818)
codes
#> [1] 380 124 818
country <- c("italy", "canada", "egypt")
country <- c('italy', 'canada', 'egypt') # 单引号
```
- 有时，命名向量的条目很有用。例如，在定义国家/地区代码向量时，我们可以使用名称来连接两者：
```r
codes <- c(italy = 380, canada = 124, egypt = 818)
codes
#>  italy canada  egypt 
#>    380    124    818
class(codes)
#> [1] "numeric"
names(codes)
#> [1] "italy"  "canada" "egypt"

# 如果使用不带引号的字符串看起来很混乱，请知道您也可以使用引号
codes <- c("italy" = 380, "canada" = 124, "egypt" = 818)
codes
#>  italy canada  egypt 
#>    380    124    818

# 使用 `names` 函数分配名称
codes <- c(380, 124, 818)
country <- c("italy","canada","egypt")
names(codes) <- country
codes
#>  italy canada  egypt 
#>    380    124    818
```
- 创建向量生成序列
```r
seq(1, 10, 2)
#> [1] 1 3 5 7 9
1:10
#>  [1]  1  2  3  4  5  6  7  8  9 10
```
- 我们使用方括号来访问向量的特定元素
```r
codes[2]
#> canada 
#>    124
codes[c(1,3)]
#> italy egypt 
#>   380   818
codes[1:2]
#>  italy canada 
#>    380    124
codes["canada"]
#> canada 
#>    124
codes[c("egypt","italy")]
#> egypt italy 
#>   818   380
```
- 当一个函数试图将一种类型强制为另一种类型并遇到不可能的情况时，它通常会给我们一个警告，并将条目转换为一个称为 `NA` 的特殊值，表示“不可用”.
```r
x <- c("1", "b", "3")
as.numeric(x)
#> Warning: NAs introduced by coercion
#> [1]  1 NA  3
```
- 排序
```r
library(dslabs)
sort(murders$total)
#>  [1]    2    4    5    5    7    8   11   12   12   16   19   21   22
#> [14]   27   32   36   38   53   63   65   67   84   93   93   97   97
#> [27]   99  111  116  118  120  135  142  207  219  232  246  250  286
#> [40]  293  310  321  351  364  376  413  457  517  669  805 1257

# 函数 `order` 返回对输入向量进行排序的索引，而不是对输入向量进行排序
index <- order(x)
x[index]
#> [1]  4 15 31 65 92
x
#> [1] 31  4 15 92 65
order(x)
#> [1] 2 3 1 5 4

# 对于任何给定的向量，它返回一个向量，其中包含输入向量的第一个条目、第二个条目等的排名
x <- c(31, 4, 15, 92, 65)
rank(x)
#> [1] 3 1 2 5 4
```
- 逻辑运算符
```r
TRUE & TRUE
#> [1] TRUE
TRUE & FALSE
#> [1] FALSE
FALSE & FALSE
#> [1] FALSE
west <- murders$region == "West"
safe <- murder_rate <= 1
ind <- safe & west
murders$state[ind]
#> [1] "Hawaii"  "Idaho"   "Oregon"  "Utah"    "Wyoming"

# 将逻辑向量转换为索引而不是保留长逻辑向量是很方便的。函数 `which` 告诉我们逻辑向量的哪些条目是 TRUE。所以我们可以输入：
ind <- which(murders$state == "California")
murder_rate[ind]
#> [1] 3.37

# 使用函数 `match` 。该函数告诉我们第二个向量的哪些索引与第一个向量的每个条目匹配：
ind <- match(c("New York", "Florida", "Texas"), murders$state)
ind
#> [1] 33 10 44
murder_rate[ind]
#> [1] 2.67 3.40 3.20

# 如果我们想要一个逻辑而不是索引来告诉我们第一个向量的每个元素是否在第二个向量中，我们可以使用函数 `%in%` 。假设您不确定波士顿、达科他州和华盛顿州是否是州。你可以这样查找：
c("Boston", "Dakota", "Washington") %in% murders$state
#> [1] FALSE FALSE  TRUE

```
- 基本图
```r
# `plot` 函数可用于制作散点图。这是谋杀总数与人口的关系图。
x <- murders$population / 10^6
y <- murders$total
plot(x, y)
```
![](https://rafalab.dfci.harvard.edu/dsbook-part-1/R/R-basics_files/figure-html/first-plot-1.png)
```r
# 为了避免两次访问变量的快速绘图，我们可以使用 `with` 函数：
with(murders, plot(population, total))
```
- 直方图是数字列表的强大图形摘要，可让您大致了解所拥有的值的类型。我们只需输入以下内容即可制作谋杀率的直方图：
```r
murders$rate <- with(murders, total / population * 100000)
boxplot(rate~region, data = murders)
```
![][https://rafalab.dfci.harvard.edu/dsbook-part-1/R/R-basics_files/figure-html/r-base-hist-1.png]
- 箱线图也将在本书的数据可视化部分中进行描述。它们提供了比直方图更简洁的摘要，但它们更容易与其他箱线图堆叠。例如，在这里我们可以使用它们来比较不同的区域：
```r
murders$rate <- with(murders, total / population * 100000)
boxplot(rate~region, data = murders)
```
![][https://rafalab.dfci.harvard.edu/dsbook-part-1/R/R-basics_files/figure-html/r-base-boxplot-1.png]

---

## 2. 编程基础知识
- 这是一个非常简单的示例，显示了 if-else 语句的一般结构。基本思想是打印 `a` 的倒数，除非 `a` 为 0：
```r
a <- 0

if (a != 0) {
  print(1/a)
} else{
  print("No reciprocal for 0.")
}
#> [1] "No reciprocal for 0."

```
-  一个非常有用的相关函数是 `ifelse` 。该函数采用三个参数：一个逻辑答案和两个可能的答案。如果逻辑为 `TRUE` ，则返回第二个参数中的值，如果为 `FALSE` ，则返回第三个参数中的值。这是一个例子：
```r
a <- 0
ifelse(a > 0, 1/a, NA)
#> [1] **NA**
a <- c(0, 1, 2, -4, 5)
result <- ifelse(a > 0, 1/a, NA)

# 下面是一个示例，说明如何轻松使用此函数将向量中的所有缺失值替换为零：
no_nas <- ifelse(is.na(na_example), 0, na_example) 
sum(is.na(no_nas))
#> [1] 0

# 有用的函数是 `any` 和 `all` 。 `any` 函数采用逻辑向量，如果任何条目为 `TRUE` ，则返回 `TRUE` 。 `all` 函数采用逻辑向量，如果所有条目都是 `TRUE` ，则返回 `TRUE` 。这是一个例子：
z <- c(TRUE, TRUE, FALSE)
any(z)
#> [1] TRUE
all(z)
#> [1] FALSE
```
- 定义函数
```r
avg <- function(x){
  s <- sum(x)
  n <- length(x)
  s/n
}

# 现在 `avg` 是一个计算平均值的函数：
x <- 1:100
identical(mean(x), avg(x))
#> [1] TRUE

# 您定义的函数可以有多个参数和默认值。例如，我们可以定义一个函数，根据用户定义的变量计算算术平均值或几何平均值，如下所示
avg <- function(x, arithmetic = TRUE){
  n <- length(x)
  ifelse(arithmetic, sum(x)/n, prod(x)^(1/n))
}
```
- For Loop
```r
compute_s_n <- function(n) { # 1 + 2 + 3 + ... + n
  sum(1:n)
}
for (i in 1:5) {
  print(i)
}
#> [1] 1
#> [1] 2
#> [1] 3
#> [1] 4
#> [1] 5
m <- 25
s_n <- vector(length = m) # create an empty vector
for (n in 1:m) {
  s_n[n] <- compute_s_n(n)
}

# 现在我们可以创建一个图来搜索模式：
n <- 1:m
plot(n, s_n)
```
![][https://rafalab.dfci.harvard.edu/dsbook-part-1/R/programming-basics_files/figure-html/sum-of-consecutive-squares-1.png]
- 向量化和泛函
```r
x <- 1:10
sqrt(x)
#>  [1] 1.00 1.41 1.73 2.00 2.24 2.45 2.65 2.83 3.00 3.16
y <- 1:10
x*y
#>  [1]   1   4   9  16  25  36  49  64  81 100

# 泛函是帮助我们将相同的函数应用于向量、矩阵、数据框或列表中的每个条目的函数。在这里，我们介绍了对数字向量、逻辑向量和字符向量进行操作的函数： `sapply`
x <- 1:10
sapply(x, sqrt)
#>  [1] 1.00 1.41 1.73 2.00 2.24 2.45 2.65 2.83 3.00 3.16
n <- 1:25
s_n <- sapply(n, compute_s_n)

```

---

- ## 3. tidyverse,使用数据框架, 整洁数据
```r
library(tidyverse)
```
- 我们希望分析所需的所有信息都包含在数据框中,函数 `mutate` 使用约定 `name = values` 将数据帧作为第一个参数，将变量的名称和值作为第二个参数。因此，为了增加谋杀率，我们使用：
```r
murders <- mutate(murders, rate = total/population*100000)
# 我们可以看到添加了新列：
head(murders)
#>        state abb region population total rate
#> 1    Alabama  AL  South    4779736   135 2.82
#> 2     Alaska  AK   West     710231    19 2.68
#> 3    Arizona  AZ   West    6392017   232 3.63
#> 4   Arkansas  AR  South    2915918    93 3.19
#> 5 California  CA   West   37253956  1257 3.37
#> 6   Colorado  CO   West    5029196    65 1.29

# 函数 `mutate` 也可用于转换变量。例如，以下代码对总体变量进行对数变换：
mutate(murders, population = log10(population))

# 通常，我们需要对多个变量应用相同的转换。函数 `across` 方便了操作。例如，如果想要对人口和谋杀总数进行对数变换，我们可以使用：
mutate(murders, across(c(population, total), log10))

# 使用 cross 时，辅助函数会派上用场。一个例子是，如果我们想对所有数值变量应用相同的转换：
mutate(murders, across(where(is.numeric), log10))
```
- 现在假设我们要过滤数据帧以仅显示谋杀率低于 0.71 的条目。为此，我们使用 `filter` 函数，该函数将数据框作为第一个参数，然后将条件语句作为第二个参数。与 `mutate` 一样，我们可以在函数内使用 `murders` 中不带引号的变量名称，它会知道我们指的是列而不是工作区中的对象。
```r
filter(murders, rate <= 0.71)
#>           state abb        region population total  rate
#> 1        Hawaii  HI          West    1360301     7 0.515
#> 2          Iowa  IA North Central    3046355    21 0.689
#> 3 New Hampshire  NH     Northeast    1316470     5 0.380
#> 4  North Dakota  ND North Central     672591     4 0.595
#> 5       Vermont  VT     Northeast     625741     2 0.320
```
- 虽然我们的数据框只有六列，但有些数据框包含数百列。如果我们只想查看其中几个，可以使用 dplyr `select` 函数。在下面的代码中，我们选择三列，将其分配给一个新对象，然后过滤新对象：
```r
new_dataframe <- select(murders, state, region, rate) 
# 第一个参数 `murders` 是一个对象，但 `state` 、 `region` 和 `rate` 是变量名
filter(new_dataframe, rate <= 0.71)
#>           state        region  rate
#> 1        Hawaii          West 0.515
#> 2          Iowa North Central 0.689
#> 3 New Hampshire     Northeast 0.380
#> 4  North Dakota North Central 0.595
#> 5       Vermont     Northeast 0.320
```
- 管道操作
```r
murders |> select(state, region, rate) |> filter(rate <= 0.71)
#>           state        region  rate
#> 1        Hawaii          West 0.515
#> 2          Iowa North Central 0.689
#> 3 New Hampshire     Northeast 0.380
#> 4  North Dakota North Central 0.595
#> 5       Vermont     Northeast 0.320

# 一般来说，管道将管道左侧的结果发送为管道右侧函数的第一个参数。这是一个非常简单的例子：
16 |> sqrt()
#> [1] 4
16 |> sqrt() |> log2() # log2(sqrt(16))
#> [1] 2

# 管道将值发送到第一个参数，因此我们可以定义其他参数，就像第一个参数已经定义一样
16 |> sqrt() |> log(base = 2)
#> [1] 2
murders |> select(state, region, rate) |> filter(rate <= 0.71)
```
- 汇总数据, summarize和group_by函数
```r
library(dplyr)
library(dslabs)
# 以下代码计算女性的平均值和标准差
s <- heights |> 
  filter(sex == "Female") |>
  summarize(average = mean(height), standard_deviation = sd(height))
s
#>   average standard_deviation
#> 1    64.9               3.76

# 美国谋杀率是美国谋杀案总数除以美国总人口，正确的计算是：
us_murder_rate <- murders |>
  summarize(rate = sum(total)/sum(population)*100000)
us_murder_rate
#>   rate
#> 1 3.03

# 假设我们需要同一变量的三个摘要，例如中位数、最小高度和最大高度。我们可以像这样使用 `summarize`
heights |> summarize(median = median(height), min = min(height), max = max(height))
#>   median min  max
#> 1   68.5  50 82.7

# 我们可以使用 `quantile` 函数仅用一行即可获得这三个值： `quantile(x, c(0.5, 0, 1))` 返回中位数（第 50 个百分位数）、最小值（第 0 个百分位数）和最大值（第 100 个百分位数）向量 `x` 。这里我们不能使用 `summarize` 因为它需要每行一个值。相反，我们必须使用 `reframe` 函数：
heights |> reframe(quantiles = quantile(height, c(0.5, 0, 1)))
#>   quantiles
#> 1      68.5
#> 2      50.0
#> 3      82.7

# 如果我们想要每个摘要有一列，如上面的 `summarize` 调用，我们必须定义一个返回数据框的函数，如下所示：
median_min_max <- function(x){
  qs <- quantile(x, c(0.5, 0, 1))
  data.frame(median = qs[1], min = qs[2], max = qs[3])
}
heights |> summarize(median_min_max(height))
#>   median min  max
#> 1   68.5  50 82.7
```
- 使用group_by总结
```r
# 我们可能想分别计算男性和女性身高的平均值和标准差。该`group_by`函数可以帮助我们做到这一点
heights |> group_by(sex)
#> # A tibble: 1,050 × 2
#> # Groups:   sex [2]
#>   sex   height
#>   <fct>  <dbl>
#> 1 Male      75
#> 2 Male      70
#> 3 Male      68
#> 4 Male      74
#> 5 Male      61
#> # ℹ 1,045 more rows

# 特别是**dplyr**`summarize`函数在作用于此对象时会有所不同。从概念上讲，您可以将此表视为多个表，这些表具有相同的列，但不一定具有相同的行数，堆叠在一个对象中。当我们在分组后汇总数据时，会发生以下情况：
heights |> 
  group_by(sex) |>
  summarize(average = mean(height), standard_deviation = sd(height))
#> # A tibble: 2 × 3
#>   sex    average standard_deviation
#>   <fct>    <dbl>              <dbl>
#> 1 Female    64.9               3.76
#> 2 Male      69.3               3.61

# 该`summarize`函数将汇总分别应用于每个组
# 让我们使用上面的定义来计算该国四个地区的中位数、最小值和最大值谋杀率`median_min_max`：
murders |> 
  group_by(region) |>
  summarize(median_min_max(rate))
#> # A tibble: 4 × 4
#>   region        median   min   max
#>   <fct>          <dbl> <dbl> <dbl>
#> 1 Northeast       1.80 0.320  3.60
#> 2 South           3.40 1.46  16.5 
#> 3 North Central   1.97 0.595  5.36
#> 4 West            1.29 0.515  3.63
```
- pull提取变量
```r
class(us_murder_rate)
#> [1] "data.frame"

# 与大多数**dplyr**函数一样，`summarize`总是返回一个数据框,如果我们想将此结果与需要数值的函数一起使用，这可能会有问题。这里我们展示了一个使用管道访问存储在数据中的值的有用技巧：当数据对象通过管道传输时，可以使用函数访问该对象及其列`pull`。要使用一行额外的代码从原始数据表中获取数字，我们可以输入：
us_murder_rate <- murders |> 
  summarize(rate = sum(total)/sum(population)*100000) |>
  pull(rate)

us_murder_rate
#> [1] 3.03
class(us_murder_rate)
#> [1] "numeric"
```
- 排序
```r
# 检查数据集时，按不同列对数据框进行排序通常很方便。我们知道`order`and`sort`函数，但对于对整个数据框进行排序，**dplyr**函数`arrange`很有用。例如，这里我们按人口规模对各州进行排序：
murders |> arrange(population) |> head()
#>                  state abb        region population total   rate
#> 1              Wyoming  WY          West     563626     5  0.887
#> 2 District of Columbia  DC         South     601723    99 16.453
#> 3              Vermont  VT     Northeast     625741     2  0.320
#> 4         North Dakota  ND North Central     672591     4  0.595
#> 5               Alaska  AK          West     710231    19  2.675
#> 6         South Dakota  SD North Central     814180     8  0.983

# `arrange`可以决定按哪一列进行排序。例如，要查看按谋杀率排序的各州，我们将使用`arrange(rate)`,请注意，默认行为是按升序排列。在**dplyr**中，该函数`desc`将向量转换为降序排列。要按谋杀率降序对数据框进行排序，我们可以输入：
murders |> arrange(desc(rate)) 

# 嵌套排序
murders |> 
  arrange(region, rate) |> 
  head()
#>           state abb    region population total  rate
#> 1       Vermont  VT Northeast     625741     2 0.320
#> 2 New Hampshire  NH Northeast    1316470     5 0.380
#> 3         Maine  ME Northeast    1328361    11 0.828
#> 4  Rhode Island  RI Northeast    1052567    16 1.520
#> 5 Massachusetts  MA Northeast    6547629   118 1.802
#> 6      New York  NY Northeast   19378102   517 2.668

# 要查看谋杀率最高的特定数量的观察结果，我们可以使用该`top_n`函数。此函数将数据框作为其第一个参数，将要显示的行数作为第二个参数，将要过滤的变量作为第三个参数。以下是如何查看前 5 行的示例：
murders |> top_n(5, rate)
#>                  state abb        region population total  rate
#> 1 District of Columbia  DC         South     601723    99 16.45
#> 2            Louisiana  LA         South    4533372   351  7.74
#> 3             Maryland  MD         South    5773552   293  5.07
#> 4             Missouri  MO North Central    5988927   321  5.36
#> 5       South Carolina  SC         South    4625364   207  4.48
```
 -  占位符
```r
 # 使用管道的一个优点`|>`是，我们在操作数据框时不必不断命名新对象。管道左侧的对象用作管道右侧函数的第一个参数。但是，如果我们想将其作为参数传递给不是第一个的右侧函数，该怎么办？答案是占位符运算符`_`（对于`%>%`管道，占位符是`.`）。下面是一个将`base`参数传递给`log`函数的简单示例。以下三个是等效的：
log(8, base = 2)
2 |> log(8, base = _)
2 %>% log(8, base = .)
```

---

### 3.数据表
[Table](https://rafalab.dfci.harvard.edu/dsbook-part-1/R/data-table.html)