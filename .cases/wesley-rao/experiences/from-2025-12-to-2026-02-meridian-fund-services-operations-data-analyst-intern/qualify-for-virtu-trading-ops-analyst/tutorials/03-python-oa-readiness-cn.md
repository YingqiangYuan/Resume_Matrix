# 教程 03：Python 面试/OA 就绪（惯用法 + 从 C++ 迁移 + 限时刷题）

> 对应 POC-03，🟡 Important。
>
> 这篇是给你（Wesley）专门写的。你的算法功底已经在 C++ 里练扎实了：hash table、priority queue、binary search、部分成交匹配，你都手写过。所以这篇教程**不是教你算法**，而是教你把这套已经在脑子里的直觉，**平移到 Python 上，并且在限时 OA 的压力下不卡语法**。
>
> 换句话说：你已经知道"该用哈希表"，我们要做的是让你在看到题的那一刻，手指自动敲出 Python 里最地道、最短、最不容易写错的那一版，而不是把 C++ 的写法一比一翻译过来（那样又慢又容易踩坑）。

---

## 0. 先说清楚这篇要解决的问题

Virtu 这个 Trading Ops Analyst 岗位，招聘第一关是 HackerRank 在线编程测试（Python），后面的技术电面也会考 Python。你的风险不在"想不出算法"，而在两个地方：

1. **语法手感**：C++ 选手切到 Python，最常见的不是不会，而是"每写三行要停下来想一下 Python 这里怎么写"，限时场景下这点迟疑会累积成致命的时间损耗。
2. **惯用法（idiom）**：Python 有一整套"地道写法"，用了就短、就快、就不容易错；不用就等于用 Python 写 C++，又长又容易在边界上翻车。面试官一眼能看出你是不是"真的会 Python"。

所以这篇的主线只有一条：**每个知识点都给你三栏，"你在 C++ 里会怎么写" → "Python 地道写法" → "为什么 Python 这样更快更不易错"**。你顺着自己已有的直觉读，会非常快。

读完你应该能做到：

- HackerRank 中等题，用 Python 限时做对，不卡语法。
- 把你 C++ 撮合系统里的"分批成交聚合"模块，用 Python 重写并跑通。
- 讲清楚 defaultdict / Counter / 推导式等惯用法各自该在什么时候用。
- 理解 as-of 聚合口径（按"截至某日已成交量"聚合，不假设订单一定被填满）。

建议你边读边在本地开一个 `python3` 交互式解释器（终端敲 `python3` 回车），每个例子都亲手敲一遍跑出来。手感是敲出来的，不是读出来的。

---

## 1. 基础心智模型：Python 和 C++ 到底哪里不一样

在进入具体惯用法之前，先建立几个"总纲"级别的差异认知。这几条想清楚了，后面很多细节你自己就能推出来。

### 1.1 变量没有类型声明，容器是异构的

C++ 里你写：

```cpp
std::vector<int> v = {1, 2, 3};
std::unordered_map<std::string, int> m;
int x = 5;
```

Python 里对应：

```python
v = [1, 2, 3]
m = {}
x = 5
```

没有 `int`、没有 `std::vector<>`、没有尖括号泛型。变量名只是一个"贴在对象上的标签"。这带来两个直接后果：

- **写得快**：你不用打类型，OA 里省下大量击键。
- **要小心**：Python 不会在编译期帮你抓类型错误，很多 bug 要到运行时才炸。所以 Python 选手更依赖"边界测试"和"打印中间结果"来自查。

### 1.2 缩进就是代码块（没有大括号）

C++ 用 `{}` 界定作用域，Python 用**缩进**。一个 `if` 或 `for` 后面跟冒号 `:`，下一行缩进（惯例是 4 个空格）就是它的代码块。

```python
for i in range(3):
    print(i)          # 这行属于 for
print("done")         # 这行不属于 for，缩进退回来了
```

> 💡 **C++ 选手最容易踩的 Python 坑**：混用 Tab 和空格。C++ 里无所谓，Python 里混用会直接报 `TabError` 或产生诡异的缩进逻辑。解决办法：把编辑器设成"Tab 转 4 空格"，一劳永逸。HackerRank 网页编辑器默认就是空格，一般没事，但你本地练习时务必统一。

### 1.3 一切皆对象，赋值是"绑定引用"

这条是 C++ 选手最容易翻车的地方，单独拎出来在第 9 节"陷阱"里细讲。这里先记住一句话：**Python 里 `b = a` 对于 list/dict 这类可变对象，`b` 和 `a` 指向的是同一个东西，不是拷贝。** C++ 里 `vector<int> b = a;` 是深拷贝，Python 里不是。这个差异会咬你，记住它。

### 1.4 输入输出：OA 的第一道坎

HackerRank 通常从标准输入读数据。C++ 你习惯 `cin >> x`，Python 用 `input()`：

```python
n = int(input())                      # 读一行，转成整数
arr = list(map(int, input().split())) # 读一行 "1 2 3 4"，切成整数列表
name = input().strip()                # 读一行字符串，去掉首尾空白
```

逐行拆解 `list(map(int, input().split()))`（从里往外读，这是读 Python 表达式的通用技巧）：

- `input()` 读进来一整行字符串，比如 `"1 2 3 4"`。
- `.split()` 按空白切开，得到列表 `["1", "2", "3", "4"]`（注意还是字符串）。
- `map(int, ...)` 把 `int` 这个函数逐个作用到每个元素上，得到 `1, 2, 3, 4`（但此时是一个惰性的 map 对象，还没真正展开）。
- `list(...)` 把它实体化成列表 `[1, 2, 3, 4]`。

> ✅ **限时小练习 1**：读入一行 N 个整数，输出它们的和。
>
> **参考解**：
> ```python
> arr = list(map(int, input().split()))
> print(sum(arr))
> ```
> `sum()` 是内置函数，直接对列表求和。C++ 里你要 `accumulate(v.begin(), v.end(), 0)`，Python 一个 `sum()` 搞定。这就是"惯用法省时间"的第一个直观例子。

---

## 2. 推导式（comprehension）：Python 的头号生产力工具

这是你从 C++ 迁过来后，收益最大的一个惯用法。务必练到形成肌肉记忆。

### 2.1 list 推导式

**场景**：把一个列表里每个元素平方。

C++ 你会写：

```cpp
std::vector<int> out;
for (int x : v) {
    out.push_back(x * x);
}
```

Python 地道写法：

```python
out = [x * x for x in v]
```

**为什么更快更不易错**：一行就说清了"我要一个新列表，元素是每个 x 的平方"。没有 `push_back`，没有提前声明空容器，没有忘记初始化的风险。读法是"从 `for` 开始读"：`for x in v`（对 v 里每个 x）→ `x * x`（放进去的是 x 的平方）。

**加过滤条件**：只要偶数的平方。

```python
out = [x * x for x in v if x % 2 == 0]
```

对应 C++ 是循环里再套一个 `if`。Python 把这个 `if` 直接挂在推导式尾巴上。

### 2.2 dict 推导式

**场景**：建一个"值 → 下标"的映射。

C++：

```cpp
std::unordered_map<int, int> idx;
for (int i = 0; i < v.size(); i++) {
    idx[v[i]] = i;
}
```

Python：

```python
idx = {v[i]: i for i in range(len(v))}
```

或者更地道，配合 `enumerate`（下一节讲）：

```python
idx = {val: i for i, val in enumerate(v)}
```

### 2.3 set 推导式

```python
seen = {x % 10 for x in v}   # 所有元素的个位数，自动去重
```

大括号里没有冒号就是 set，有冒号就是 dict。空的 `{}` 是 dict 不是 set，空 set 要写 `set()`，这是个小坑，记一下。

> 💡 **C++ 选手最容易踩的 Python 坑**：不要滥用嵌套推导式。两层还能读，三层就成了"只写不读"的密码。限时场景下，如果一个推导式你自己写完要盯着看两秒才确定对不对，那就拆成普通 for 循环，可读性优先。惯用法是为了帮你，不是为了炫技。

> ✅ **限时小练习 2**：给一个字符串列表 `words`，返回所有长度大于 3 的单词，全部转大写。
>
> **参考解**：
> ```python
> out = [w.upper() for w in words if len(w) > 3]
> ```

---

## 3. enumerate 和 zip：告别手动索引

### 3.1 enumerate：要下标又要值

C++ 里遍历带下标：

```cpp
for (int i = 0; i < v.size(); i++) {
    cout << i << ": " << v[i] << endl;
}
```

Python 新手（写得像 C++）会写：

```python
for i in range(len(v)):
    print(i, v[i])
```

这能跑，但不地道。地道写法用 `enumerate`：

```python
for i, val in enumerate(v):
    print(i, val)
```

**为什么更好**：`enumerate` 直接同时给你下标 `i` 和值 `val`，不用每次 `v[i]` 去索引（少一次可能写错下标的机会）。想让下标从 1 开始？`enumerate(v, start=1)`。

### 3.2 zip：并行遍历多个列表

**场景**：两个等长列表，一个价格一个数量，逐对相乘求总额。

C++：

```cpp
double total = 0;
for (int i = 0; i < prices.size(); i++) {
    total += prices[i] * qtys[i];
}
```

Python：

```python
total = sum(p * q for p, q in zip(prices, qtys))
```

`zip(prices, qtys)` 把两个列表"拉链"式配对：`(p0,q0), (p1,q1), ...`。配合生成器表达式（推导式去掉外层方括号，直接喂给 `sum`）一行算完。这个模式在交易数据处理里极其常见，记牢。

> 💡 **坑**：`zip` 以最短的那个列表为准，长的部分会被静默丢弃。如果你需要"以最长为准、短的补默认值"，用 `itertools.zip_longest`。限时时先确认两个列表是不是等长。

---

## 4. collections 模块：你最该背下来的三件套

这一节是 OA 提速的重点。C++ 里你手写 hash table 计数、手写队列，Python 标准库直接给你现成的、还更好用的。

### 4.1 defaultdict：省掉"键不存在"的判断

**场景**：统计每个单词出现次数（其实用 Counter 更好，但先用这个讲清 defaultdict 的价值）。

C++ 里你会写：

```cpp
std::unordered_map<std::string, int> cnt;
for (auto& w : words) {
    cnt[w]++;   // C++ 里 map[key] 不存在会自动插入默认值 0
}
```

注意：C++ 的 `map[key]++` 恰好能工作，因为 `operator[]` 会为不存在的键插入默认构造值（int 是 0）。但 Python 普通 dict **不会**：

```python
cnt = {}
for w in words:
    cnt[w] += 1     # ❌ KeyError！键第一次出现时 cnt[w] 还不存在
```

普通 dict 的笨办法（能跑但啰嗦）：

```python
cnt = {}
for w in words:
    if w not in cnt:
        cnt[w] = 0
    cnt[w] += 1
```

地道写法用 `defaultdict`：

```python
from collections import defaultdict
cnt = defaultdict(int)      # 缺省值工厂是 int，即 0
for w in words:
    cnt[w] += 1             # 键不存在时自动先建为 0，再 +1
```

**为什么更好**：`defaultdict(int)` 里那个 `int` 是一个"工厂函数"，当你访问一个不存在的键时，它自动调用 `int()`（结果是 0）建好这个键。你就不用写那个 `if w not in cnt` 了。

`defaultdict` 最强的用法其实是**分组（group by）**，缺省值工厂用 `list`：

```python
from collections import defaultdict
groups = defaultdict(list)
for order_id, fill in fills:
    groups[order_id].append(fill)   # 键不存在时自动先建成空列表 []
```

这个"按某个键把记录归拢到一起"的模式，就是第 8 节"分批成交聚合"的核心，先在这里眼熟它。

### 4.2 Counter：计数就是一行

上面那个"统计词频"，其实 Python 有专门的类：

```python
from collections import Counter
cnt = Counter(words)          # 直接传入可迭代对象，自动计数
print(cnt["apple"])           # 访问不存在的键返回 0，不报错
print(cnt.most_common(3))     # 出现次数最多的 3 个，返回 [(词, 次数), ...]
```

C++ 里做"找出现次数最多的前 3 个"，你得建 map 计数、再倒进 vector、再 sort、再取前 3。Python 一个 `most_common(3)` 全包了。**限时场景遇到"频率/众数/Top-K 计数"类题，第一反应就是 Counter。**

`Counter` 还支持像多重集合一样做加减：

```python
Counter("aab") + Counter("bcc")   # Counter({'b': 2, 'a': 2, 'c': 2})
```

### 4.3 deque：双端队列（BFS / 滑动窗口用）

C++ 你有 `std::deque` 和 `std::queue`。Python 的 `list` 可以当栈（`append` / `pop` 都是尾部，O(1)），但**当队列用很慢**：`list.pop(0)` 从头部弹是 O(n)，因为要把后面所有元素往前挪。

BFS、需要从头部高效弹出的场景，用 `collections.deque`：

```python
from collections import deque
q = deque()
q.append(x)        # 尾部入队 O(1)
q.popleft()        # 头部出队 O(1)  ← 这是关键，list 做不到 O(1)
q.appendleft(y)    # 头部入队 O(1)
```

> 💡 **C++ 选手最容易踩的 Python 坑**：习惯性用 `list.pop(0)` 当队列。数据量一大就超时（TLE）。凡是"从头部反复弹出"的场景，一律 `deque`。这是 HackerRank 上 C++ 选手最常见的性能坑之一。

---

## 5. 排序：sorted + key/lambda

排序是 OA 高频操作，Python 的排序惯用法非常值得练熟。

### 5.1 sorted 与 list.sort

- `sorted(v)`：返回一个**新的**排好序的列表，原列表不动。
- `v.sort()`：**原地**排序，返回 `None`。

> 💡 **坑**：`x = v.sort()` 是新手经典错误，`sort()` 原地排序、返回 `None`，所以 `x` 会是 `None`。要新列表就用 `sorted`。

### 5.2 key：自定义排序依据

C++ 里你传一个比较函数（comparator），描述"谁排在谁前面"。Python 换了个思路：你传一个 **key 函数**，描述"每个元素拿什么值来比"，Python 按这个值升序排。这个思路差异很重要，你不是在比较两两，而是在给每个元素算一个"排序键"。

**场景**：按字符串长度排序。

C++：

```cpp
std::sort(words.begin(), words.end(),
          [](const string& a, const string& b){ return a.size() < b.size(); });
```

Python：

```python
words.sort(key=len)          # 每个元素用它的长度当排序键
```

`len` 是内置函数，直接当 key 传进去。更复杂的键用 `lambda`（匿名函数）：

**场景**：一批订单 `(order_id, qty, price)`，先按 qty 降序，qty 相同再按 price 升序。

```python
orders.sort(key=lambda o: (-o[1], o[2]))
```

逐点拆解：

- `lambda o: (...)` 定义一个匿名函数，参数是 `o`（每个订单元组），返回一个"排序键元组"。
- `-o[1]`：qty 取负数。Python 排序默认升序，取负就等价于让 qty 降序。
- `o[2]`：price，升序。
- 返回**元组** `(-o[1], o[2])`：Python 比较元组是"逐元素字典序"，先比第一个，相等再比第二个。这正好实现了"先按 qty 降序，qty 相同按 price 升序"的多级排序。

**为什么更好**：C++ 的多级排序你要在 comparator 里写一堆 `if (a.qty != b.qty) return ...; return ...;`，容易漏 case、容易把方向写反。Python 用"返回一个元组"来表达多级排序，声明式、不易错。**"多级排序 = 返回一个元组当 key"**，这个套路请刻进肌肉记忆。

> ✅ **限时小练习 3**：给一批 `(name, score)`，按 score 降序输出名字；score 相同时按 name 字母升序。
>
> **参考解**：
> ```python
> for name, score in sorted(data, key=lambda t: (-t[1], t[0])):
>     print(name)
> ```

---

## 6. 字符串处理：常用招式速查

交易 Ops 的题很多是"解析一行文本"。Python 的字符串处理比 C++ 舒服得多。

### 6.1 常用方法

```python
s = "  BUY,AAPL,100  "
s.strip()                 # 去首尾空白 -> "BUY,AAPL,100"
s.strip().split(",")      # 按逗号切 -> ["BUY", "AAPL", "100"]
"AAPL".lower()            # 转小写 -> "aapl"
"hello".startswith("he")  # True
"file.csv".endswith(".csv")   # True
",".join(["a", "b", "c"]) # 用逗号拼接 -> "a,b,c"
"abc".replace("a", "X")   # -> "Xbc"
```

`split` / `join` 这对是重点。C++ 里手写字符串分割要用 `stringstream` 或者一堆 `find` + `substr`，繁琐易错。Python 一个 `.split(",")` 搞定，拼回去一个 `",".join(...)`。

> 💡 **坑**：`split()` 不带参数时按"任意连续空白"切并自动忽略空串；`split(" ")` 带一个空格参数时按单个空格切，连续空格会切出空字符串 `""`。解析用户输入通常用不带参数的 `split()` 更稳。

### 6.2 f-string：格式化输出

C++ 你可能用 `printf` 或者 `<<` 拼接。Python 用 f-string（字符串前加 `f`，大括号里直接写变量/表达式）：

```python
order_id = 42
qty = 100
print(f"Order {order_id} filled {qty} shares")
# Order 42 filled 100 shares
print(f"price = {3.14159:.2f}")   # 保留 2 位小数 -> price = 3.14
print(f"{qty:>8}")                # 右对齐宽度 8
```

f-string 是现代 Python 输出的默认选择，可读性最好，OA 里直接用它。

### 6.3 字符与数字互转

```python
ord('a')      # 97   字符转 ASCII 码
chr(97)       # 'a'  ASCII 码转字符
'5'.isdigit() # True 判断是否全是数字字符
int('42')     # 42   字符串转整数
str(42)       # '42' 整数转字符串
```

> ✅ **限时小练习 4**：一行日志 `"2026-07-04 BUY AAPL 100"`，解析出日期、方向、代码、数量（数量转成整数）。
>
> **参考解**：
> ```python
> date, side, symbol, qty = "2026-07-04 BUY AAPL 100".split()
> qty = int(qty)
> ```
> `split()` 切成 4 段刚好解包给 4 个变量（这叫"解包赋值"，C++ 里对应 structured bindings，但 Python 更灵活）。

---

## 7. 常见题型模板：看到题就能套

OA 中等题翻来覆去就那几个套路。这里给你 Python 版模板，配合你已有的 C++ 直觉，看到题能直接套骨架。

### 7.1 哈希去重 / 两数之和

**思路**：一次遍历，用 dict/set 记录见过的东西，O(n) 解决。这是你在 C++ 里最熟的套路，Python 版更短。

```python
def two_sum(nums, target):
    seen = {}                      # 值 -> 下标
    for i, x in enumerate(nums):
        if target - x in seen:     # 需要的另一半之前见过吗
            return [seen[target - x], i]
        seen[x] = i                # 记下当前值和下标
    return []
```

`in seen` 对 dict 是查键，平均 O(1)，和 C++ 的 `unordered_map.count()` 一个道理。

### 7.2 双指针

**场景**：已排序数组里找两数之和等于 target。

```python
def two_sum_sorted(nums, target):
    lo, hi = 0, len(nums) - 1      # 一行同时赋两个变量
    while lo < hi:
        s = nums[lo] + nums[hi]
        if s == target:
            return [lo, hi]
        elif s < target:
            lo += 1                # 和太小，左指针右移
        else:
            hi -= 1                # 和太大，右指针左移
    return []
```

和 C++ 逻辑完全一样，只是语法更干净。注意 `lo, hi = 0, len(nums)-1` 这种"同时给多个变量赋值"的写法，很 Python。

### 7.3 滑动窗口

**场景**：最长的没有重复字符的子串长度。

```python
def longest_unique(s):
    seen = set()
    left = 0
    best = 0
    for right, ch in enumerate(s):
        while ch in seen:          # 窗口内已有该字符，收缩左边界
            seen.remove(s[left])
            left += 1
        seen.add(ch)
        best = max(best, right - left + 1)
    return best
```

模板骨架：右指针 `for` 循环扩张，条件不满足时 `while` 收缩左边界，每步更新答案。你在 C++ 里写过的那套，直接搬。

### 7.4 简单 DP

**场景**：爬楼梯，每次 1 或 2 阶，到第 n 阶有多少种走法。

```python
def climb(n):
    if n <= 2:
        return n
    a, b = 1, 2                    # 前两阶的方法数
    for _ in range(3, n + 1):      # 用 _ 表示"这个循环变量我不关心"
        a, b = b, a + b            # 同时更新，右边先整体算完再赋值
    return b
```

**注意 `a, b = b, a + b`**：这是 Python 的"同时赋值"，右边 `(b, a+b)` 会**先整体求值**成一个元组，再一次性拆给左边。所以不需要像 C++ 那样搞个临时变量 `tmp`。这是 Python 写 DP 状态转移的招牌写法。

### 7.5 字符串解析题

**场景**：一批交易记录，每行 `"SIDE,SYMBOL,QTY"`，统计每个 symbol 的净买入量（BUY 加、SELL 减）。

```python
from collections import defaultdict

def net_position(lines):
    net = defaultdict(int)
    for line in lines:
        side, symbol, qty = line.strip().split(",")
        qty = int(qty)
        if side == "BUY":
            net[symbol] += qty
        else:
            net[symbol] -= qty
    return dict(net)
```

这题已经很接近你的实际岗位工作了。下一节我们把它升级成"分批成交聚合"，也就是你 C++ 撮合系统里那个模块的 Python 重写。

---

## 8. 手把手：把"分批成交聚合"从 C++ 翻成 Python

这是这篇教程的重头戏，也是你面试时能直接讲的、和岗位强相关的内容。我们把你 C++ 撮合系统里的"分批成交聚合 + partial fill 匹配"模块，用 Python 重写并跑通，同时把 **as-of 聚合口径**讲透。

### 8.1 业务背景（先把口径讲清楚）

场景是这样的：

- 你有一批**内部订单**（internal orders），比如"买 AAPL 10000 股"。
- 券商（broker）不一定一次给你成交完，而是**分批成交（partial fills）**。买 10000 股可能拆成 4000 + 3000 + 3000 三笔，甚至跨天成交：今天成交 7000，剩下 3000 转到明天。
- 你的对账（reconciliation）工作是：把同一个订单号（order_id）下的所有分批成交**归拢起来求和**，再和内部订单的应成交量比对，找出差异。

关键的**口径（这是最容易在面试里被追问、也最容易做错的点）**：

> **as-of 口径**：当你在"截至某个日期"这个时间点做对账时，一个订单**当天可能只成交了一部分，余量转到后面的日子**。所以你聚合的是"**截至该日期已经成交的量**"，**不能假设订单一定被完全填满（filled）**，也不能拿"订单终态的目标量"去替代"实际已成交量"。

用一句话记：**聚合"已发生的成交"，而不是"计划要成交的量"。** 这个区分在真实交易 Ops 里非常重要，你今天做的对账，反映的是今天为止真实发生了什么，不是这个订单最终会变成什么。

### 8.2 C++ 里你会怎么写（回忆一下你的思路）

你在 C++ 里大概是这样组织的（伪代码，唤起你的记忆）：

```cpp
// 一笔成交
struct Fill {
    int order_id;
    std::string symbol;
    long qty;
    std::string fill_date;   // 成交日期
};

// 按 order_id 聚合截至 as_of_date 的已成交量
std::unordered_map<int, long> aggregate(
        const std::vector<Fill>& fills, const std::string& as_of_date) {
    std::unordered_map<int, long> filled;
    for (const auto& f : fills) {
        if (f.fill_date <= as_of_date) {     // 只算截至该日的
            filled[f.order_id] += f.qty;     // 按订单号累加
        }
    }
    return filled;
}
```

核心三步：**遍历成交 → 按日期过滤（as-of） → 按订单号累加**。Python 版本逻辑完全一样，只是写起来更短。

### 8.3 Python 完整实现（可直接跑）

下面是完整、可运行的 Python 版本。你可以整段贴进一个 `.py` 文件跑，也可以贴进 `python3` 交互式解释器。

```python
from collections import defaultdict


def aggregate_filled(fills, as_of_date):
    """
    按 order_id 聚合"截至 as_of_date 已成交的数量"。

    fills: 列表，每个元素是一个 dict：
        {"order_id": int, "symbol": str, "qty": int, "fill_date": "YYYY-MM-DD"}
    as_of_date: 字符串 "YYYY-MM-DD"，只统计成交日期 <= 这一天的。

    返回: dict，{order_id: 截至该日已成交总量}
    """
    filled = defaultdict(int)               # 键不存在时默认 0，省掉存在性判断
    for f in fills:
        if f["fill_date"] <= as_of_date:    # as-of 过滤：只算已经发生的成交
            filled[f["order_id"]] += f["qty"]   # 按订单号累加已成交量
    return dict(filled)                     # 转回普通 dict 返回，输出更干净


def reconcile(orders, fills, as_of_date):
    """
    把内部订单的"目标量"和"截至 as_of_date 的实际已成交量"比对。

    orders: 列表，每个元素 {"order_id": int, "symbol": str, "target_qty": int}
    返回: 列表，每个元素是一条对账结果，含目标量、已成交量、未成交余量、是否完全成交。
    """
    filled = aggregate_filled(fills, as_of_date)   # 先聚合出已成交量

    report = []
    for o in orders:
        oid = o["order_id"]
        done = filled.get(oid, 0)          # 没有任何成交记录的订单，已成交量按 0 算
        remaining = o["target_qty"] - done # 未成交余量（可能 > 0，表示还没填满）
        report.append({
            "order_id": oid,
            "symbol": o["symbol"],
            "target_qty": o["target_qty"],
            "filled_qty": done,
            "remaining_qty": remaining,
            "fully_filled": remaining == 0,
        })
    return report


# ---------- 造一批测试数据来演示 ----------
if __name__ == "__main__":
    orders = [
        {"order_id": 1, "symbol": "AAPL", "target_qty": 10000},
        {"order_id": 2, "symbol": "MSFT", "target_qty": 5000},
        {"order_id": 3, "symbol": "TSLA", "target_qty": 2000},
    ]

    # 订单 1: 分 3 批，其中一批在 as-of 之后（不该被算进来）
    # 订单 2: 只成交了一部分（partial，余量转天）
    # 订单 3: 完全没有成交记录
    fills = [
        {"order_id": 1, "symbol": "AAPL", "qty": 4000, "fill_date": "2026-07-03"},
        {"order_id": 1, "symbol": "AAPL", "qty": 3000, "fill_date": "2026-07-04"},
        {"order_id": 1, "symbol": "AAPL", "qty": 3000, "fill_date": "2026-07-05"},  # as-of 之后
        {"order_id": 2, "symbol": "MSFT", "qty": 2000, "fill_date": "2026-07-04"},
    ]

    as_of = "2026-07-04"
    for row in reconcile(orders, fills, as_of):
        print(row)
```

跑出来你会看到（`as_of = "2026-07-04"`）：

```
{'order_id': 1, 'symbol': 'AAPL', 'target_qty': 10000, 'filled_qty': 7000, 'remaining_qty': 3000, 'fully_filled': False}
{'order_id': 2, 'symbol': 'MSFT', 'target_qty': 5000, 'filled_qty': 2000, 'remaining_qty': 3000, 'fully_filled': False}
{'order_id': 3, 'symbol': 'TSLA', 'target_qty': 2000, 'filled_qty': 0, 'remaining_qty': 2000, 'fully_filled': False}
```

### 8.4 逐段讲解（重点看 as-of 口径怎么落地）

**订单 1 是理解 as-of 的关键**。它有三批成交：7-03 的 4000、7-04 的 3000、7-05 的 3000。但我们的 as-of 日期是 **7-04**。所以：

- 7-03 的 4000：`"2026-07-03" <= "2026-07-04"` 为真，算进来。
- 7-04 的 3000：`"2026-07-04" <= "2026-07-04"` 为真，算进来。
- 7-05 的 3000：`"2026-07-05" <= "2026-07-04"` 为**假**，**不算**。

于是截至 7-04，订单 1 的已成交量是 4000 + 3000 = **7000**，不是订单最终会到的 10000。`fully_filled` 是 `False`。**这就是 as-of 口径的精髓：你聚合的是"截至这一刻已经发生的"，未来才会成交的那 3000 股此刻还不存在于你的对账里。** 如果你偷懒用 `target_qty` 当已成交量，就会错误地报告"已成交 10000"，这在真实对账里是严重错误。

注意上面比较日期用的是**字符串直接比大小**。为什么可以这么干？因为 `"YYYY-MM-DD"` 这种格式的字典序恰好等于时间先后序（年在前、月次之、日最后，且每段都补零对齐）。这是个很方便的小技巧，但**只在这种严格补零的定长格式下成立**；如果日期格式不规整，就得先用 `datetime.strptime` 转成日期对象再比。

**订单 2 演示 partial fill（部分成交）**：目标 5000，截至 7-04 只成交了 2000，余量 3000。这不是错误，是正常的"还没填满"，可能明天继续成交。所以我们的报告里 `remaining_qty` 是一个有用的信号，而不是把它当异常。

**订单 3 演示"零成交"**：它在 `fills` 里一条记录都没有。看 `reconcile` 里的 `filled.get(oid, 0)`，`dict.get(key, default)` 在键不存在时返回默认值 `0`，而不是报 `KeyError`。这一行专门处理了"这个订单还没有任何成交"的情况。**这是 C++ 选手容易漏的边界**：C++ 里 `map[oid]` 不存在会自动插 0，Python 普通 dict 的 `[]` 会抛异常，所以聚合结果查订单时要用 `.get(oid, 0)` 兜底。

**为什么用 `defaultdict(int)` 做聚合**：回顾 `aggregate_filled` 里的 `filled[f["order_id"]] += f["qty"]`。因为 `filled` 是 `defaultdict(int)`，第一次遇到某个 order_id 时它自动初始化成 0 再累加，我们就不用写"如果这个 key 不存在先设成 0"。这正是第 4.1 节讲的 defaultdict 的价值，在真实代码里的体现。

### 8.5 面试怎么讲这段（把技术翻译成业务语言）

如果面试官让你讲这个模块，建议这样组织（这也是 Trading Ops 岗位看重的"能把技术和业务对上"的能力）：

1. **问题**："一个内部订单常常被券商分成多笔、甚至跨天成交，对账时要把同一订单号下的成交归拢求和，再和目标量比对，找出未成交余量。"
2. **口径**："对账是 as-of 的，我聚合的是截至对账日已经发生的成交量，不假设订单已被填满。跨对账日之后的成交不能计入当日口径，否则会虚报成交进度。"
3. **实现**："用按 order_id 分组累加，日期上做 as-of 过滤，对没有成交记录的订单用默认 0 兜底，避免漏掉零成交订单。"
4. **数据结构选择**："分组聚合用哈希表（Python 的 defaultdict），查找和累加都是均摊 O(1)，整体一次遍历 O(n)。"

这四句话，把你 C++ 的实现经验、Python 的表达能力、和交易业务口径全串起来了，正是这个岗位想听到的。

> ✅ **限时小练习 5**：在上面代码基础上，增加一个函数 `over_filled(orders, fills, as_of_date)`，返回所有"已成交量超过目标量"的订单（这在现实里是异常，可能重复下单或数据错误）。
>
> **参考解**：
> ```python
> def over_filled(orders, fills, as_of_date):
>     filled = aggregate_filled(fills, as_of_date)
>     result = []
>     for o in orders:
>         done = filled.get(o["order_id"], 0)
>         if done > o["target_qty"]:              # 已成交超过目标 = 异常
>             result.append((o["order_id"], done, o["target_qty"]))
>     return result
> ```
> 这就是把"对账"从"找未成交"扩展到"找超成交"，一个函数复用了聚合逻辑。面试里能主动想到"超成交也是要报的异常"，是加分项。

---

## 9. 限时 OA 心态和踩坑

算法你不缺，这一节讲的是"怎么在限时压力下不因为 Python 的坑而丢分"。

### 9.1 读题和边界（比你想的更重要）

- **先读完再动手**：HackerRank 题面常有隐藏约束（数据范围、是否有负数、是否有空输入）。C++ 你可能靠类型和编译器兜底，Python 没有，边界要靠你自己想清楚。
- **先想清楚边界再写**：空列表、单元素、全相同、最大数据量。花 30 秒把这几个 case 在脑子里过一遍，比写完再 debug 快得多。
- **数据范围决定复杂度**：n 到 1e5 一般要 O(n log n) 以内；n 到 1e3 才敢 O(n²)。Python 常数比 C++ 大，同样复杂度更容易 TLE，所以能用惯用法（内置函数、Counter、set）就用，它们底层是 C 实现的，比你手写循环快。

### 9.2 五个 C++ 选手最容易踩的 Python 陷阱

**陷阱 1：可变默认参数**

```python
def add_item(x, bucket=[]):     # ❌ 这个 [] 只在函数定义时创建一次！
    bucket.append(x)
    return bucket

print(add_item(1))   # [1]
print(add_item(2))   # [1, 2]  ← 竟然还留着上次的 1！出乎意料
```

默认参数在**函数定义时求值一次**，之后所有调用共享同一个 list。正确写法是用 `None` 当哨兵：

```python
def add_item(x, bucket=None):
    if bucket is None:
        bucket = []             # 每次调用新建
    bucket.append(x)
    return bucket
```

C++ 没有这个坑（默认参数是值），这是纯 Python 特产，务必记住。

**陷阱 2：整数除法 `/` 和 `//`**

```python
7 / 2     # 3.5   ← / 永远返回浮点数，即使能整除
7 // 2    # 3     ← // 是向下取整的整除
-7 // 2   # -4    ← 注意！向下取整，是 -4 不是 -3
7 % 2     # 1
-7 % 2    # 1     ← Python 的 % 结果符号跟除数走，和 C++ 不同！
```

C++ 里 `/` 对两个 int 是整除，Python 里 `/` 永远给浮点。要整除结果用 `//`。而且 **Python 的 `//` 和 `%` 是向下取整语义**，负数行为和 C++（向零取整）不一样，涉及负数取模时格外小心。

**陷阱 3：深浅拷贝**

```python
a = [[0, 0], [0, 0]]
b = a                    # ❌ b 和 a 是同一个 list，不是拷贝
b[0][0] = 99
print(a)                 # [[99, 0], [0, 0]]  ← a 也变了！

import copy
c = a[:]                 # 浅拷贝：外层是新的，但内层子列表还是共享的
d = copy.deepcopy(a)     # 深拷贝：彻底独立
```

C++ 里 `vector b = a` 是深拷贝，Python 里 `b = a` 只是给同一个对象贴第二个标签。要真正复制，一维列表用 `a[:]` 或 `list(a)`，嵌套结构用 `copy.deepcopy`。**建二维数组时也要小心这个坑**：

```python
grid = [[0] * 3] * 3     # ❌ 三行是同一个列表的三个引用！
grid[0][0] = 1
print(grid)              # [[1,0,0],[1,0,0],[1,0,0]]  ← 三行一起变

grid = [[0] * 3 for _ in range(3)]   # ✅ 用推导式，每行独立创建
```

**陷阱 4：遍历时修改容器**

```python
nums = [1, 2, 3, 4]
for x in nums:
    if x % 2 == 0:
        nums.remove(x)   # ❌ 边遍历边删，会跳过元素或行为诡异
```

正确做法是遍历一个副本，或者用推导式构造新列表：

```python
nums = [x for x in nums if x % 2 != 0]   # ✅ 保留奇数，等价于"删掉偶数"
```

C++ 里 `erase` 迭代器失效你也熟，Python 这里同理，但更推荐直接"构造新列表"这种声明式写法。

**陷阱 5：`is` 与 `==` 混用**

```python
a = [1, 2]
b = [1, 2]
a == b      # True   比较"值相不相等"
a is b      # False  比较"是不是同一个对象"
```

`==` 比值，`is` 比身份（是否同一对象）。判断内容相等一律用 `==`；`is` 只用来和 `None` 比（`if x is None:`）。C++ 里 `==` 和指针比较是分开的语法，Python 这俩长得像但含义完全不同，别混。

### 9.3 限时答题的操作节奏（给你的 checklist）

1. 读题，圈出数据范围和边界约束（30 秒）。
2. 心里过一遍：空输入、单元素、最大规模会不会 TLE。
3. 想清楚用哪个数据结构（哈希/set/deque/排序），先写骨架。
4. 用小例子在脑子里或注释里跑一遍，确认逻辑。
5. 提交前手动想三个边界 case，别只测样例。
6. 卡壳超过 3 分钟就先写个能过部分用例的暴力解，保底拿分，再回头优化。

> 💡 **给你的定制建议**：你的算法不是问题，你的时间会花在"语法迟疑"上。所以考前一周，把本教程第 2 到 7 节的每个惯用法**默写三遍**，把第 8 节的聚合代码**不看答案敲一遍**。目标是把这些写法变成不过脑子的肌肉记忆，这样 OA 时你的脑力可以全部投在算法本身，而不是"这里 Python 怎么写"。

---

## 10. 快速参考卡（考前扫一眼）

| 需求 | C++ 习惯 | Python 惯用法 |
|------|----------|---------------|
| 读一行整数 | `cin >>` 循环 | `list(map(int, input().split()))` |
| 求和 | `accumulate` | `sum(v)` |
| 变换列表 | for + push_back | `[f(x) for x in v]` |
| 过滤列表 | for + if + push_back | `[x for x in v if cond]` |
| 计数/词频 | 手写 map | `Counter(v)` / `.most_common(k)` |
| 分组累加 | `map[key] += val` | `defaultdict(int)` + `d[key] += val` |
| 分组归拢 | map<key, vector> | `defaultdict(list)` + `d[key].append(x)` |
| 队列 | `std::queue` | `deque` + `popleft()` |
| 带下标遍历 | `for(i...)` | `for i, x in enumerate(v)` |
| 并行遍历 | 双下标 | `for a, b in zip(A, B)` |
| 多级排序 | 写 comparator | `key=lambda x: (k1, k2)`（返回元组） |
| 格式化输出 | `printf` / `<<` | `f"{var}"` |
| 字符串切分 | `stringstream` | `s.split(",")` |
| 键不存在兜底 | `map[k]` 自动 0 | `d.get(k, default)` |
| 交换变量 | `tmp` 中转 | `a, b = b, a` |

---

## 11. 你的下一步

1. **今天**：把第 8 节的聚合代码整段敲进本地跑通，改改 as-of 日期，观察输出怎么变。确保你能对着空白编辑器把它默写出来。
2. **本周**：上 HackerRank，做 5 道 Easy 找语法手感，再做 5 道 Medium 卡表计时（每题 20 分钟），重点不是难度而是"用 Python 写不卡"。
3. **电面前**：把第 8.5 节那四句话练到能脱口而出，把第 9.2 节五个陷阱各在解释器里复现一遍，眼见为实记得更牢。

你的底子好，缺的只是 Python 的手感和几个特有的坑。这篇过两三遍，加上几十道题的手感，OA 这关对你就是"稳"，不是"赌"。加油，Wesley。
