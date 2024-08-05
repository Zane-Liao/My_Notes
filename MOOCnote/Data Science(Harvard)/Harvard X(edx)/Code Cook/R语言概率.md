#### 第 1 节：离散概率
#### 第 2 节：连续概率
#### 第 3 节：随机变量、抽样模型和中心极限定理
#### 第 4 节：大空头

---
### 1.  离散概率
- 我们使用符号
$$
Pr(A)
$$
表示事件的概率𝐴发生。
	离散概率是指随机变量取有限或可数无限个值的情况。常见的离散概率分布包括二项分布和泊松分布。离散随机变量是只能取特定值的随机变量。比如，掷骰子的结果只能是1到6之间的整数。
- 蒙特卡洛，计算机提供了一种实际执行上述简单随机实验的方法：从装有三颗蓝色珠子和两颗红色珠子的袋子中随机挑选一颗珠子。随机数生成器使我们能够模拟随机挑选的过程。
一个例子是`sample`R 中的函数。我们在下面的代码中演示了它的用法。首先，我们使用该函数`rep`生成 urn：
```r
beads <- rep(c("red", "blue"), times = c(2,3))
beads
#> [1] "red"  "red"  "blue" "blue" "blue"

# 然后`sample`随机挑选一颗珠子：
sample(beads, 1)
#> [1] "red"
```
这行代码产生一个随机结果。我们想无限次重复这个实验，但不可能无限重复。相反，我们重复实验的次数足够多，使结果实际上等同于无限重复。**这是_蒙特卡洛_模拟**的一个例子。为了执行第一个蒙特卡罗模拟，我们使用函数`replicate`，该函数允许我们重复执行相同任务任意次。在这里，我们重复随机事件$$
B = 10000
$$
次
```r
B <- 10000
events <- replicate(B, sample(beads, 1))

# 现在我们可以验证我们的定义是否与蒙特卡洛模拟近似一致。我们用它`table`来查看分布：
tab <- table(events)
tab
#> events
#> blue  red 
#> 6027 3973

# `prop.table`给出了比例：
prop.table(tab)
#> events
#>  blue   red 
#> 0.603 0.397
# 这是一个简单且不太实用的例子，因为我们可以轻松地用数学方法计算概率。当很难或不可能用数学方法计算精确概率时，蒙特卡罗模拟就很有用。在深入研究更复杂的例子之前，我们先用一些简单的例子来演示 R 中可用的计算工具。
```
- 设置随机种子
```r
# 我们使用了随机数生成器。这意味着呈现的许多结果可能会因偶然因素而发生变化，表明本书的静态版本显示的结果可能与您按照呈现的代码获得的结果不同。这实际上是可以的，因为结果是随机的并且会随时间而变化。但是，如果您想确保每次运行的结果一致，可以将 R 的随机数生成种子设置为特定数字。上面我们将其设置为 1986。我们希望避免每次都使用相同的种子。选择种子的一种流行方法是年 - 月 - 日。例如，我们在 2018 年 12 月 20 日选择了 1986 年：2018−12−20=1986
set.seed(1986) 
```
- 有无替换
```r
# 该函数`sample`有一个参数，允许我们从瓮中挑选多个元素。但是，默认情况下，这种选择是_无放回的_：选择一颗珠子后，它不会放回袋子中。注意当我们要求随机选择五颗珠子时会发生什么：
sample(beads, 5)
#> [1] "red"  "blue" "blue" "blue" "red"
sample(beads, 5)
#> [1] "red"  "red"  "blue" "blue" "blue"
sample(beads, 5)
#> [1] "blue" "red"  "blue" "red"  "blue"

# 这会导致重新排列，始终包含三个蓝色珠子和两个红色珠子。如果我们要求选择六个珠子，则会出错：
sample(beads, 6)
# Error in sample.int(length(x), size, replace, prob) : cannot take a sample larger than the population when 'replace = FALSE'

# 但是，`sample`可以直接使用该函数，而无需使用`replicate`，在相同条件下重复相同的实验，即从 5 个珠子中连续挑选 1 个。为此，我们采用_放回_抽样法：选择珠子后将其放回瓮中。我们可以通过将默认为 的参数`sample`更改为 来执行此操作：`replace``FALSE``replace = TRUE`
events <- sample(beads, B, replace = TRUE)
prop.table(table(events))
#> events
#>  blue   red 
#> 0.602 0.398
```
- 独立性， 如果一个事件的结果不影响另一个事件，我们称两个事件为独立事件。经典的例子是抛硬币。
 - 每次我们抛出一枚公平硬币，无论之前的结果如何，出现正面的概率都是 1/2。当我们从有放回的瓮中取出珠子时也是如此。在上面的例子中，无论之前的结果如何，出现红色的概率都是 0.40。
 - 许多非独立事件的例子都来自纸牌游戏。当我们发第一张牌时，得到 K 的概率是 1/13，因为有十三种可能性：Ace、2、3、…、十、杰克、皇后、国王和 A。现在，如果我们将第一张牌发成国王，并且不将其放回牌堆中，则第二张牌是国王的概率会降低，因为只剩下三张国王。概率是 51 分之 3。
 - 因此，这些事件不是**独立的**：第一个结果影响下一个结果。要了解非独立事件的极端情况，请考虑随机**不**重复地抽取五颗珠子的例子：
```r
x <- sample(beads, 5)

# 如果你必须猜测第一个珠子的颜色，你会预测是蓝色，因为蓝色有 60% 的概率。但是，如果我向你展示最后四个结果：
x[2:5]
#> [1] "blue" "blue" "blue" "red"
# 剩下的唯一一颗珠子就是红色的。事件不是独立的，所以概率会发生变化。
```
- 条件概率，当事件不独立时，_条件概率_很有用。我们已经演示了条件概率的一个例子：假设第一张牌是 K，我们计算了第二张牌是 K 的概率。在概率中，我们使用以下符号：
$$
Pr(Card\;2\;is\;a\;king \mid Card\;1\;is\;a\;king) = \frac{3}{51}
$$

我们使用∣作为“假设”或“条件”的简写。当两个事件发生时，比如𝐴和B，是独立的，我们有：
$$
Pr(A \mid B) = Pr(A)
$$

这是用数学的方式表达的：B发生的概率不会影响𝐴正在发生。事实上，这可以被认为是独立性的数学定义。
- 乘法和加法法则
如果我们想确定两个事件的概率，比如𝐴和B发生，我们可以使用乘法规则：
$$
Pr(A\;and\;B) = Pr(A)Pr(B\mid A)
$$
让我们以二十一点为例。在二十一点中，您会得到两张随机牌。在您看到自己拥有的牌后，您可以要求更多。目标是比庄家更接近 21 点，但不超过。人头牌值 10 分，A 牌值 11 分或 1 分（您选择）。
在二十一点游戏中，为了计算通过抽取一张 A 然后再抽一张人头牌获得 21 点的概率，我们计算第一张牌是 A 的概率，然后将其乘以抽取一张人头牌或 10 的概率（假设第一张牌是 A 的话）：1/13×16/51≈0.025

乘法规则也适用于两个以上的事件。我们可以使用归纳法来扩展更多事件：
$$
Pr(A\;and\;B\;and\;C) = Pr(A)Pr(B\mid A)Pr(C\mid A\;and\;B)
$$
- 独立条件下的乘法规则,当处理独立事件时，乘法规则变得更简单：
$$
Pr(A\;and\;B\;and\;C) = Pr(A)Pr(B)Pr(C)
$$
然而，在使用此版本的乘法规则之前，我们必须非常小心，因为当事件实际上并不独立时，假设独立性可能会导致非常不同且不正确的概率计算。
举个例子，假设有一个法庭案件，其中嫌疑人被描述为有胡子和胡须。被告有胡子和胡须，控方请来一位“专家”作证说，1/10 的男人有胡须，1/5 的男人有胡子。因此，使用乘法规则，我们得出结论，只有1/10×1/5或 0.02 两者兼有。
但是，要像这样乘法，我们需要假设独立性！假设一个男人有胡子的条件概率（以他有胡须为条件）为 0.95。那么，正确的计算概率要高得多：1/10×95/100=0.095。
乘法规则还为我们提供了计算条件概率的一般公式：
$$
Pr(B\mid A) = \frac{Pr(A\;and\;B)}{Pr(A)}
$$
- 添加规则，加法规则告诉我们：
$$
Pr(A\;or\;B) = Pr(A)+Pr(B)-Pr(A\;and\;B)
$$
这条规则很直观；考虑一下维恩图。如果我们简单地添加概率，我们会计算两次交集，所以我们需要减去一个实例。
![][https://rafalab.dfci.harvard.edu/dsbook-part-2/prob/discrete-probability_files/figure-html/venn-diagram-addition-rule-1.png]
- 组合和排列
在我们的第一个例子中，我们想象了一个有五颗珠子的瓮。提醒一下，为了计算单次抽奖的概率分布，我们只需列出所有可能性，总共 5 种。随后，对于每个事件，我们计算出这些可能性中有多少与该事件相关。选择蓝色珠子的概率为 3/5，因为五种可能结果中有三种是蓝色的。
对于更复杂的情况，计算并不那么简单。例如，如果我不重复抽五张牌，我得到的都是相同花色的牌，这在扑克中被称为“同花”，那么这种概率是多少？在离散概率课程中，您将学习如何进行这些计算的理论。在这里，我们重点介绍如何使用 R 代码来计算答案。
首先，让我们构造一副牌。为此，我们将使用`expand.grid`and`paste`函数。我们使用`paste`通过连接较小的字符串来创建字符串。为此，我们获取牌的数字和花色，并创建牌名，如下所示：
```r
number <- "Three"
suit <- "Hearts"
paste(number, suit)
#> [1] "Three Hearts"

# `paste`也适用于对向量按元素执行操作：
paste(letters[1:5], as.character(1:5))
#> [1] "a 1" "b 2" "c 3" "d 4" "e 5"

# 该函数`expand.grid`为我们提供了两个向量的所有条目组合。例如，如果您有蓝色和黑色的裤子以及白色、灰色和格子衬衫，则所有组合都是：
expand.grid(pants = c("blue", "black"), shirt = c("white", "grey", "plaid"))
#>   pants shirt
#> 1  blue white
#> 2 black white
#> 3  blue  grey
#> 4 black  grey
#> 5  blue plaid
#> 6 black plaid

# 以下是我们如何生成一副牌：
suits <- c("Diamonds", "Clubs", "Hearts", "Spades")
numbers <- c("Ace", "Deuce", "Three", "Four", "Five", "Six", "Seven", 
             "Eight", "Nine", "Ten", "Jack", "Queen", "King")
deck <- expand.grid(number = numbers, suit = suits)
deck <- paste(deck$number, deck$suit)

# 构建好牌组后，我们可以通过计算满足条件的可能结果的比例来再次检查第一张牌是国王的概率是否为 1/13：
kings <- paste("King", suits)
mean(deck %in% kings)
#> [1] 0.0769
```
现在，假设第一张牌是 K，那么第二张牌是 K 的条件概率是多少？之前，我们推断，如果已经从牌堆中抽出一张 K，剩下 51 张牌，那么这个概率就是 3/51。让我们通过列出所有可能的结果来确认一下。
为此，我们可以使用**gtools**`permutations`包中的函数。对于任何大小为 的列表，此函数都会计算我们在选择项目时可以获得的所有不同组合。以下是我们可以从包含 的列表中选择两个数字的所有方法：n r 1 ,2, 3
```r
library(gtools)
permutations(3, 2)
#>      [,1] [,2]
#> [1,]    1    2
#> [2,]    1    3
#> [3,]    2    1
#> [4,]    2    3
#> [5,]    3    1
#> [6,]    3    2
```
请注意，这里的顺序很重要：3,1 与 1,3 不同。另外，请注意 (1,1)、(2,2) 和 (3,3) 不会出现，因为一旦我们选出一个数字，它就不能再出现。
或者，我们可以添加一个向量。如果您想从所有可能的电话号码中随机取出五个七位数字的电话号码（无重复），您可以输入：
```r
all_phone_numbers <- permutations(10, 7, v = 0:9)
n <- nrow(all_phone_numbers)
index <- sample(n, 5)
all_phone_numbers[index,]
#>      [,1] [,2] [,3] [,4] [,5] [,6] [,7]
#> [1,]    1    3    8    0    6    7    5
#> [2,]    2    9    1    6    4    8    0
#> [3,]    5    1    6    0    9    8    2
#> [4,]    7    4    6    0    2    8    1
#> [5,]    4    6    5    9    2    8    0
```
它不使用默认的数字 1 到 10，而是使用我们提供的`v`数字：0 到 9。
为了计算在顺序很重要的情况下选择两张牌的所有可能方式，我们输入：
```r
hands <- permutations(52, 2, v = deck)

# 这是一个有两列和 2652 行的矩阵。通过矩阵，我们可以像这样获得第一张和第二张卡片：
first_card <- hands[,1]
second_card <- hands[,2]

# 现在，第一手牌是国王的情况可以按如下方式计算：
kings <- paste("King", suits)
sum(first_card %in% kings)
#> [1] 204

# 为了得到条件概率，我们计算其中第二张牌是国王的比例：
sum(first_card %in% kings & second_card %in% kings) / 
  sum(first_card %in% kings)
#> [1] 0.0588

# 正如我们已经推导的那样，它恰好是 3/51。请注意，上面的代码相当于：
mean(first_card %in% kings & second_card %in% kings) / 
  mean(first_card %in% kings)
#> [1] 0.0588
```
它使用`mean`而不是`sum`并且是 R 版本的：
$$
\frac{Pr(A\;and\;B)}{Pr(A)}
$$
如果顺序无关紧要怎么办？例如，在二十一点中，如果您在第一轮抽牌中获得一张 A 和一张人头牌，这被称为_Natural 21_，您自动获胜。如果我们想计算这种情况发生的概率，我们会列举组合_，_而不是排列，因为顺序无关紧要。
```r
combinations(3,2)
#>      [,1] [,2]
#> [1,]    1    2
#> [2,]    1    3
#> [3,]    2    3
# 第二行的结果不包含 (2,1)，因为 (1,2) 已经被枚举过了。 (3,1) 和 (3,2) 也是如此。

# 因此，要计算二十一点中自然 21 的概率，我们可以这样做：
aces <- paste("Ace", suits)

facecard <- c("King", "Queen", "Jack", "Ten")
facecard <- expand.grid(number = facecard, suit = suits)
facecard <- paste(facecard$number, facecard$suit)

hands <- combinations(52, 2, v = deck)
mean(hands[,1] %in% aces & hands[,2] %in% facecard)
#> [1] 0.0483

# 在最后一行，我们假设 Ace 先出现。这个假设是基于我们对`combination`枚举可能性的了解而做出的，它会首先列出这种情况。但是，为了安全起见，我们可以这样写并得出相同的答案：
mean((hands[,1] %in% aces & hands[,2] %in% facecard) |
       (hands[,2] %in% aces & hands[,1] %in% facecard))
#> [1] 0.0483
```
- 蒙特卡罗实例
`combinations`我们可以使用蒙特卡洛来估计这个概率，而不是使用来推断出 Natural 21 的确切概率。在这种情况下，我们反复抽两张牌，并记录我们获得了多少张 21。我们可以使用函数 sample 来不重复地抽两张牌：
```r
hand <- sample(deck, 2)
hand
#> [1] "Queen Clubs"  "Seven Spades"

# 然后检查一张牌是 A 还是另一张是人头牌或 10。接下来，当我们提到人头_牌_时，我们会包括 10。现在，我们需要检查两种可能性：
(hand[1] %in% aces & hand[2] %in% facecard) | 
  (hand[2] %in% aces & hand[1] %in% facecard)
#> [1] FALSE

# 如果我们重复 10,000 次，我们就能非常准确地近似得到 Natural 21 的概率。让我们首先编写一个函数，该函数画一只手，如果得到 21，则返回 TRUE。该函数不需要任何参数，因为它使用全局环境中定义的对象。
blackjack <- function(){
   hand <- sample(deck, 2)
  (hand[1] %in% aces & hand[2] %in% facecard) | 
    (hand[2] %in% aces & hand[1] %in% facecard)
}

# 这里，我们必须检查两种可能性：先是 Ace 还是后是 Ace，因为我们没有使用函数。如果我们得到 21，`combinations`函数将返回，否则：`TRUE``FALSE`
blackjack()
#> [1] FALSE

# 现在我们可以玩这个游戏，比如说 10,000 次：
B <- 10000
results <- replicate(B, blackjack())
mean(results)
#> [1] 0.0475
```
- 蒙提霍尔问题
20 世纪 70 年代，有一档游戏节目叫《我们做个交易》，主持人是蒙提·霍尔。在游戏的某个时刻，参赛者被要求从三扇门中选择一扇。其中一扇门后面有奖品，而其他门后面有一只山羊，表示参赛者输了。在参赛者选好一扇门后，在揭晓所选门是否有奖品之前，蒙提·霍尔会打开剩余两扇门中的一扇，并向参赛者透露那扇门后面没有奖品。然后，他会问：“你想换门吗？”你会怎么做？
我们可以用概率来证明，如果你坚持原来的门选择，你赢得奖品的几率仍然是 1/3。但是，如果你换到另一扇门，你获胜的几率就会翻倍到 2/3！这似乎有悖常理。许多人错误地认为两种机会都是 1/2，因为你是在两个选项之间进行选择。下面，我们使用蒙特卡洛模拟来查看哪种策略更好。请注意，出于教学目的，此代码的编写比应有的要长。
我们先从坚持策略开始：
```r
B <- 10000
monty_hall <- function(strategy){
  doors <- as.character(1:3)
  prize <- sample(c("car", "goat", "goat"))
  prize_door <- doors[prize == "car"]
  my_pick  <- sample(doors, 1)
  show <- sample(doors[!doors %in% c(my_pick, prize_door)],1)
  stick <- my_pick
  stick == prize_door
  switch <- doors[!doors %in% c(my_pick, show)]
  choice <- ifelse(strategy == "stick", stick, switch)
  choice == prize_door
}
stick <- replicate(B, monty_hall("stick"))
mean(stick)
#> [1] 0.342
switch <- replicate(B, monty_hall("switch"))
mean(switch)
#> [1] 0.668
```

在编写代码时，我们发现，当我们坚持最初的选择时，以 和 开头的行`my_pick`对`show`最后的逻辑操作没有影响。由此，我们应该意识到，正如我们最初考虑的那样，机会是 1/3。当我们切换时，蒙特卡洛估计证实了 2/3 的计算。这有助于我们获得一些见解，表明我们正在移除一扇门，`show`这扇门在我们的选择中绝对不是赢家。我们还看到，除非我们在第一次选择时就选对了，否则我们获胜的概率是 1 - 1/3 = 2/3。
- 生日问题
假设你在一个有 50 人的教室里。如果我们假设这是随机选择的 50 人组，那么至少有两个人生日相同的概率是多少？虽然这有点高级，但我们可以通过数学推导得出这个结论。我们稍后会这样做。在这里，我们使用蒙特卡洛模拟。为简单起见，我们假设没有人出生在 2 月 29 日，这不会显著改变答案。
首先，请注意生日可以表示为 1 至 365 之间的数字，因此可以通过以下方式获取 50 个生日的样本：
```r
n <- 50
bdays <- sample(1:365, n, replace = TRUE)

# 要检查在这 50 个人中是否有至少两个人的生日相同，我们可以使用该`duplicated`函数，`TRUE`只要向量的元素重复，该函数就会返回。以下是一个例子：
duplicated(c(1, 2, 3, 1, 4, 3, 5))
#> [1] FALSE FALSE FALSE  TRUE FALSE  TRUE FALSE

# 第二次出现 1 和 3 时，我们得到`TRUE`。因此，要检查两个生日是否相同，我们只需使用`any`and`duplicated`函数，如下所示：
any(duplicated(bdays))
#> [1] TRUE

#在这种情况下，我们看到它确实发生了；至少有两个人的生日是同一天。为了估计群体中生日相同的概率，我们通过反复抽样 50 个生日来重复此实验：
B <- 10000
same_birthday <- function(n){
  bdays <- sample(1:365, n, replace = TRUE)
  any(duplicated(bdays))
}
results <- replicate(B, same_birthday(50))
mean(results)
#> [1] 0.969
```
人们往往会低估这些概率。要直观地了解为什么概率这么高，请考虑当群体规模接近 365 时会发生什么。在这个阶段，我们用完了所有天数，概率为 1。
假设我们想利用这些知识与朋友们打赌，赌一组人中两个人生日相同的可能性。当群组规模达到多大时，概率会大于 50%？大于 75%？
让我们创建一个查找表。我们可以快速创建一个函数来计算任何组大小：
```r
compute_prob <- function(n, B = 10000){
  results <- replicate(B, same_birthday(n))
  mean(results)
}

# n <- seq(1,60)
prob <- sapply(n, compute_prob)

# 现在，我们可以绘制一个图表，估计在一组人数为𝑛：
library(tidyverse)
prob <- sapply(n, compute_prob)
qplot(n, prob)
```
![][https://rafalab.dfci.harvard.edu/dsbook-part-2/prob/discrete-probability_files/figure-html/birthday-problem-mc-probabilities-1.png]
现在，让我们计算精确的概率，而不是依赖蒙特卡罗近似值。我们不仅可以使用数学获得精确的答案，而且由于我们不必进行实验，因此计算速度要快得多。
为了简化计算，我们不再计算发生的概率，而是计算不发生的概率。为此，我们使用乘法规则。
让我们从第一个人开始。第 1 个人拥有唯一生日的概率为 1。假设第 1 个人已经占用了一天，那么第 2 个人拥有唯一生日的概率为 364/365。随后，假设前两个人的生日都是唯一的，那么第 3 个人还有 363 天可供选择。这种模式继续下去，我们发现所有 50 个人拥有唯一生日的概率为：
$$
1\times\frac{364}{365}\times\frac{363}{365}...\frac{365 - n + 1}{365}
$$
我们可以编写一个函数对任意数字执行此操作：

```r
exact_prob <- function(n){
  prob_unique <- seq(365, 365 - n + 1)/365 
  1 - prod( prob_unique)
}
eprob <- sapply(n, exact_prob)
qplot(n, prob) + geom_line(aes(n, eprob), col = "red")
```
![][https://rafalab.dfci.harvard.edu/dsbook-part-2/prob/discrete-probability_files/figure-html/birthday-problem-exact-probabilities-1.png]
该图显示蒙特卡罗模拟对准确概率提供了非常好的估计。如果我们无法计算准确概率，我们仍然可以准确地估计它们。
 - 无限性的实际应用
 这里描述的理论需要无限地重复实验。实际上，我们无法做到这一点。在上面的例子中，我们使用乙=10，000蒙特卡罗实验，得出准确的估计值。这个数字越大，估计值就越准确，直到近似值好到你的计算机无法分辨出差异。然而，在更复杂的计算中，10,000 次可能远远不够。此外，对于某些计算，10,000 次实验可能在计算上不可行。
在实际情况下，我们事先并不知道答案是什么，所以我们不知道蒙特卡洛估计是否准确。我们知道乙，近似值就越好。但我们需要它有多大呢？这实际上是一个具有挑战性的问题，回答它通常需要高级理论统计培训。
一种实用的方法是检查估计的稳定性。以下示例说明了一组 25 人的生日问题。
```r
B <- 10^seq(1, 5, len = 100)
compute_prob <- function(B, n = 25){
  same_day <- replicate(B, same_birthday(n))
  mean(same_day)
}
prob <- sapply(B, compute_prob)
plot(log10(B), prob)
```
![][https://rafalab.dfci.harvard.edu/dsbook-part-2/prob/discrete-probability_files/figure-html/monte-carlo-convergence-1.png]
在此图中，我们可以看到值开始稳定在 1000 左右。请注意，在这种情况下已知的确切概率是 0.5686997。

---

### 2. 连续概率
- 累积分布函数
我们以成年男学生的身高为例：
```r
library(tidyverse)
library(dslabs)
x <- heights %>% filter(sex == "Male") %>% pull(height)

# 并将经验累积分布函数 (eCDF) 定义为
F <- function(a) mean(x <= a)
```
对于任何值 ，它给出列表中小于或等于 的`a`值的比例。x a
让我们将 eCDF 与概率联系起来，问：如果我随机挑选一名男学生，他身高超过 70.5 英寸的概率是多少？由于每个学生被选中的概率相同，所以这个问题的答案相当于身高超过 70.5 英寸的学生的比例。使用 eCDF，我们可以通过输入以下内容获得答案：
```r
1 - F(70)
#> [1] 0.377
```
累积分布函数（CDF，Cumulative Distribution Function）描述了随机变量 XXX 小于或等于某个值 xxx 的概率。具体来说，对于随机变量 X，CDF 定义为:
$$
F(x) = Pr(X \leq x)
$$一旦定义了 CDF，我们就可以用它来计算任何值子集的概率。例如，学生身高介于 和 之间的概率`a`是`b`：
$$
Pr(a < X \leq b) = F(b) - F(a)
$$
- 概率密度函数
在实践中非常有用的一个数学结果是，对于大多数 CDF，我们可以定义一个函数，称之为𝐹（𝑋），这使我们能够使用微积分构建 CDF，如下所示：
$$
F(b) - F(a) = \int_{a}^{b} f(x)dx
$$
  
𝐹（𝑋）被称为_概率密度函数_。直觉是，即使对于连续结果，我们也可以定义具有正概率的微小间隔，这些间隔几乎和点一样小。如果我们将这些间隔的大小视为矩形的底边，则概率密度函数𝐹确定矩形的高度，使得这些矩形面积的总和近似于概率𝐹（𝑏）−𝐹（𝐴）该和可以写成 Reimann 和，并用积分来近似：
![][https://rafalab.dfci.harvard.edu/dsbook-part-2/prob/continuous-probability_files/figure-html/unnamed-chunk-4-1.png]
这种连续分布的一个例子是正态分布。概率密度函数由以下公式给出：
$$
f(x) = e^-\frac{1}{2}(\frac{s}{s-m})^2
$$
正态分布的累积分布由一个数学公式定义，在 R 中可以使用函数获得。如果`pnorm`一个随机量的概率分布定义为：`m``s`
```r
F(a) = pnorm(a, m, s)

# 这很有用，因为如果我们愿意使用正态近似，比如说，身高，我们不需要整个数据集来回答这样的问题：随机选择的学生身高超过 70 英寸的概率是多少？我们只需要平均身高和标准差：
m <- mean(x)
s <- sd(x)
1 - pnorm(70.5, m, s)
#> [1] 0.371
```
...[连续概率](https://rafalab.dfci.harvard.edu/dsbook-part-2/prob/continuous-probability.html)
[随机变量](https://rafalab.dfci.harvard.edu/dsbook-part-2/prob/random-variables-sampling-models-clt.html)
# 补充
- ## 期望值和标准误差
期望值和标准误差是统计学中的两个重要概念，用于描述数据的中心趋势和分散程度。

### 期望值（Expected Value）

1. **定义**
   - 期望值是描述一个随机变量在大量重复实验中的平均结果。它可以被看作是该随机变量的“中心”。
   - 对于离散随机变量 \( X \) ，其期望值 \( E(X) \) 定义为：
     $$
     E(X) = \sum_{i} x_i \cdot P(X = x_i)
     $$
   - 对于连续随机变量 \( X \) ，其期望值定义为：
     $$
     E(X) = \int_{-\infty}^{\infty} x \cdot f(x) \, dx
     $$
   - 其中， (\ x_i \) 是变量的可能取值， \( \P(\X = \x_i) ) 是 ( X ) 取 ( x_i ) 的概率， ( f(x) ) 是概率密度函数。

2. **例子**
   - 如果你掷一个六面骰子，骰子的期望值是：
     $$
     E(X) = \frac{1 + 2 + 3 + 4 + 5 + 6}{6} = 3.5
     $$
   - 这意味着，如果你掷很多次骰子，结果的平均值会接近 3.5。
   - 计算单次期望值
$$\mu = a \cdot p + b \cdot (1 - p)$$
### 标准误差（Standard Error）

1. **定义**
   - 标准误差是描述样本统计量（如样本均值）围绕总体统计量（如总体均值）波动的程度。它反映了统计量的精确度。
   - 对于样本均值的标准误差 $$( SE(\bar{X}))$$ ，定义为：
     $$
     SE(\bar{X}) = \frac{\sigma}{\sqrt{N}}
     $$
   - 其中， \( \sigma \) 是总体标准差， \( N \) 是样本量。
   $$\sigma = \left| b - a \right| \cdot \sqrt{p \cdot (1 - p)}$$

2. **估计标准误差**
   - 如果总体标准差未知，可以使用样本标准差 \( s \) 来估计标准误差：
     $$
     SE(\bar{X}) = \frac{s}{\sqrt{N}}
     $$

3. **例子**
   - 如果你有一个样本，样本量 \( N = 100 \)，样本标准差 \( s = 10 \)：
     $$
     SE(\bar{X}) = \frac{10}{\sqrt{100}} = \frac{10}{10} = 1
     $$
   - 这意味着样本均值的标准误差是 1，表明样本均值估计总体均值的精确度。
$$\text{SE}_{\text{sum}} = \sqrt{次数} \cdot \sigma$$
### 总结

- **期望值** 是随机变量的平均值，表示其中心趋势。
- **标准误差** 是样本统计量的标准偏差，反映其估计总体参数的精确度。

理解这两个概念可以帮助你更好地分析数据，并理解数据的特性和估计的可靠性。

----
## 计算期望值和标准误差

- 为了更好地理解如何在不同的数据集中计算期望值和标准误差，我们可以将其分解为几个步骤。假设我们有一个数据集，其中包含了每个事件的概率和收益值，我们将通过以下步骤计算期望值和标准误差。

### 示例数据集

假设我们有一个数据集，表示事件发生的概率和对应的收益值：

| Event | Probability | Payout |
|-------|-------------|--------|
| A     | 0.2         | 5      |
| B     | 0.5         | 2      |
| C     | 0.3         | -1     |

### R代码实现

我们将计算期望值和标准误差：

1. **期望值计算**：使用加权平均方法。
2. **标准误差计算**：计算方差，然后取其平方根，并乘以事件数的平方根。

以下是完整的R代码：

```r
# 示例数据集
data <- data.frame(
  Event = c("A", "B", "C"),
  Probability = c(0.2, 0.5, 0.3),
  Payout = c(5, 2, -1)
)

# 计算期望值
EV <- sum(data$Probability * data$Payout)

# 计算方差
variance <- sum(data$Probability * (data$Payout - EV)^2)

# 计算标准误差
n <- 500 # 假设我们要进行500次实验
SE <- sqrt(n * variance)

# 打印结果
cat("期望值是：", EV, "\n")
cat("500次实验的标准误差是：", SE, "\n")
```

### 代码解释

1. **数据集定义**：
   ```r
   data <- data.frame(
     Event = c("A", "B", "C"),
     Probability = c(0.2, 0.5, 0.3),
     Payout = c(5, 2, -1)
   )
   ```
   我们定义了一个包含事件、概率和收益的数据框。

2. **期望值计算**：
   ```r
   EV <- sum(data$Probability * data$Payout)
   ```
   我们使用加权平均法计算期望值。

3. **方差计算**：
   ```r
   variance <- sum(data$Probability * (data$Payout - EV)^2)
   ```
   计算收益值的方差。

4. **标准误差计算**：
   ```r
   n <- 500 # 假设我们要进行500次实验
   SE <- sqrt(n * variance)
   ```
   计算标准误差，这里假设我们进行500次实验。

5. **打印结果**：
   ```r
   cat("期望值是：", EV, "\n")
   cat("500次实验的标准误差是：", SE, "\n")
   ```
   打印计算出的期望值和标准误差。

通过这种方法，我们可以计算任何数据集中的期望值和标准误差，只要我们知道每个事件的概率和对应的收益值。这种方法适用于各种概率分布和收益情况。

----


- ## 离散概率，连续概率，随机变量
在数据科学中，离散概率和连续概率是描述随机变量行为的重要概念。让我们深入了解这些概念，并举一些 R 语言的示例。

### 离散概率
离散概率是指随机变量取有限或可数无限个值的情况。常见的离散概率分布包括二项分布和泊松分布。

#### 离散随机变量
离散随机变量是只能取特定值的随机变量。比如，掷骰子的结果只能是1到6之间的整数。

#### 示例：二项分布
假设我们有一个重复 n 次的独立伯努利试验，每次试验成功的概率是 p。X 是成功的次数，它服从二项分布。

```r
# 二项分布
n <- 10  # 试验次数
p <- 0.5 # 成功概率

# 生成随机数
set.seed(123)
binom_sample <- rbinom(1000, n, p)

# 绘制直方图
hist(binom_sample, breaks = n+1, main = "Binomial Distribution", xlab = "Number of Successes", col = "blue")
```

#### 示例：泊松分布
泊松分布通常用于描述在固定时间内发生特定事件的次数。

```r
# 泊松分布
lambda <- 3  # 平均每单位时间的事件数

# 生成随机数
set.seed(123)
pois_sample <- rpois(1000, lambda)

# 绘制直方图
hist(pois_sample, breaks = max(pois_sample)-min(pois_sample)+1, main = "Poisson Distribution", xlab = "Number of Events", col = "green")
```

### 连续概率
连续概率是指随机变量可以取任意实数值的情况。常见的连续概率分布包括正态分布和指数分布。

#### 连续随机变量
连续随机变量是可以取任意实数值的随机变量。比如，人的身高是一个连续随机变量。

#### 示例：正态分布
正态分布是一种非常重要的连续分布，通常用于描述许多自然现象。

```r
# 正态分布
mu <- 0    # 均值
sigma <- 1 # 标准差

# 生成随机数
set.seed(123)
norm_sample <- rnorm(1000, mean = mu, sd = sigma)

# 绘制直方图
hist(norm_sample, breaks = 30, probability = TRUE, main = "Normal Distribution", xlab = "Value", col = "purple")

# 添加正态分布曲线
curve(dnorm(x, mean = mu, sd = sigma), col = "darkblue", lwd = 2, add = TRUE)
```

#### 示例：指数分布
指数分布常用于描述事件发生的时间间隔。

```r
# 指数分布
lambda <- 1  # 事件发生率

# 生成随机数
set.seed(123)
exp_sample <- rexp(1000, rate = lambda)

# 绘制直方图
hist(exp_sample, breaks = 30, probability = TRUE, main = "Exponential Distribution", xlab = "Time", col = "orange")

# 添加指数分布曲线
curve(dexp(x, rate = lambda), col = "red", lwd = 2, add = TRUE)
```

### 随机变量
随机变量是一个取决于随机现象的数值变量。它可以是离散的（只能取某些特定值）或连续的（可以取任何值）。

- 离散随机变量示例：掷硬币次数中的正面次数。
- 连续随机变量示例：某人等待公交车的时间。

通过这些示例，你可以看到如何在 R 中生成不同分布的随机数，并绘制其直方图以可视化这些分布。

