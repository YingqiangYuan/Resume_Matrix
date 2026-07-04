# 教程 01：用 SQL 做对账（PostgreSQL：JOIN / 窗口函数 / T+1 过滤）

> 对应 POC-01，🔴 Core，是整个求职计划的心脏。
> 目标岗位：Virtu Financial 的 Trading Operations Analyst（post-trade 中后台运营）。
> HackerRank OA 考 Python + SQL，SQL 不过直接出局，所以这一篇必须真的读懂、动手敲一遍。

---

## 0. 读前须知 + 环境搭建

### 0.1 这篇教程要把你带到哪

Wesley，先说结论。你现在的状态是：C++ 很硬（写过银行交易处理系统、事件驱动撮合系统），Python 会用 Pandas，但简历上 SQL 是零。这不是你笨，是你一直用「DataFrame 思维」在处理数据，从没被逼着用「数据库思维」。

好消息是：你已经有的直觉几乎可以全部迁移过来。你在 C++ 里用 hash table 做匹配、用 priority queue 排序取 top、用 binary search 找边界，这些操作在 SQL 里都有对应，而且往往一行就写完了。SQL 只是把「怎么做」交给数据库，你只负责说「我要什么」。

学完这一篇，你要能做到：

- 手写 INNER / LEFT / RIGHT / FULL OUTER JOIN，并能说清各自何时用。
- 写 GROUP BY + 聚合函数 + HAVING，把每个账户的风险敞口聚合出来。
- 用窗口函数 ROW_NUMBER() 做排名。
- 用一句 SQL 找出「内部账有、托管行没有」的 break（对账缺口）。
- 说清 WHERE 与 HAVING 的区别、LEFT JOIN 之后 NULL 到底意味着哪边漏记了。
- 把业务规则「T+1 结算」翻译成 WHERE settle_date <= as_of_date 这样的过滤条件。
- 在中等难度 SQL 题上，不查文档、限时做对。

### 0.2 什么是「对账（reconciliation）」，为什么中后台天天在做

先用大白话讲清楚这个业务，不然后面所有 SQL 你都不知道为谁而写。

想象你和室友合租，两个人各自记了一本账，记「这个月谁付了哪些钱」。月底两本账应该对得上：你记「我付了水费 80」，室友那本也应该有「Wesley 付了水费 80」。如果你这本有、室友那本没有，或者两本都有但金额不一样（你记 80、他记 60），那就是对不平了。这个「对不平的地方」，在金融运营里就叫 **break**。

在 Virtu 这种做市商 / 交易公司里，同一笔事实会被记在好几个地方：

- **内部持仓账**：公司自己系统里记「我现在持有 500 股 AAPL」。
- **托管行（custodian）持仓账**：帮公司保管资产的银行记「Virtu 在我这里存了 500 股 AAPL」。
- **内部成交记录** vs **券商 / 交易所确认（confirm）**：我方系统记「我买了 500 股」，对手方 / broker 也回一条「确认你买了 500 股」。
- **现金两侧**：内部现金账 vs 银行现金账。

这几套账**本应完全一致**。对账（reconciliation，业内常简称 recon）就是：每天把两套账拉出来，逐条比对，把对不平的 break 找出来、标出来、按风险排序，交给人去查原因（可能是漏记、录错、时间差、结算还没到）。

**这就是这个岗位每天在干的事，而 SQL 是干这件事的主力工具。** 你现在要学的每一条 JOIN、每一个 GROUP BY，都是为了「找出两套账对不平的地方」。带着这个目标读，一切都有意义。

> 💡 你可能会问：为什么不用 Pandas 干这个？用 `merge` 不也能对账吗？
> 能，而且你现在肯定觉得 Pandas 更顺手。但现实是：这些账躺在数据库里（几百万行），不可能整表拉到内存里 merge；而且团队协作、审计、定时任务全都是围绕 SQL 建的。岗位 JD 明确要 SQL，OA 明确考 SQL，所以这一篇我们**刻意不用 pandas**，就用纯 SQL 把这块硬技能补上。你的 Pandas 直觉会帮你理解，但手要敲 SQL。

### 0.3 用 Docker 起一个本地 PostgreSQL

我们不假设你装过任何数据库。最干净的方式是用 Docker 起一个 PostgreSQL 容器，用完删掉不留垃圾。

**第一步：确认你有 Docker。** 打开终端，敲：

```bash
docker --version
```

如果显示类似 `Docker version 24.x.x`，说明装好了。如果提示 command not found，去 docker.com 下载 Docker Desktop 装上（Mac / Windows 都有图形安装包），装完打开它，等鲸鱼图标不再转圈。

**第二步：拉起一个 PostgreSQL 容器。** 敲下面这一整条：

```bash
docker run --name recon-pg \
  -e POSTGRES_PASSWORD=recon123 \
  -e POSTGRES_DB=recon \
  -p 5432:5432 \
  -d postgres:16
```

逐行解释这条命令在干什么（这对你不陌生，很像配置一个服务进程）：

- `docker run` ：跑一个新容器。
- `--name recon-pg` ：给这个容器起个名字叫 recon-pg，方便以后 stop / start / 删除时点名。
- `-e POSTGRES_PASSWORD=recon123` ：设一个环境变量，数据库超级用户 postgres 的密码设成 recon123（本地练习随便设，别在生产这么干）。
- `-e POSTGRES_DB=recon` ：容器启动时自动建一个叫 recon 的数据库（database），我们所有练习都在里面做。
- `-p 5432:5432` ：把容器里的 5432 端口（PostgreSQL 默认端口）映射到你本机的 5432 端口，这样你才能从本机连进去。
- `-d postgres:16` ：用官方 postgres 镜像第 16 版，`-d` 表示后台运行（detached），不占着你的终端。

第一次运行会下载镜像，等一会儿。跑完敲 `docker ps`，能看到 recon-pg 在 STATUS 列显示 Up，就成功了。

> ⚠️ 常见误区：如果这条报错说 5432 端口被占用（port is already allocated），说明你机器上已经有别的 PostgreSQL 在跑。把上面的 `-p 5432:5432` 改成 `-p 5433:5432`，之后连接时用端口 5433 就行。

**第三步：连进去。** 最简单的方式是直接钻进容器用它自带的 `psql` 命令行客户端：

```bash
docker exec -it recon-pg psql -U postgres -d recon
```

- `docker exec -it recon-pg` ：在正在跑的 recon-pg 容器里，开一个交互式（-it）会话。
- `psql -U postgres -d recon` ：用 postgres 用户连到 recon 数据库。psql 是 PostgreSQL 官方的命令行客户端，就是你敲 SQL 的地方。

连上后提示符会变成 `recon=#`。这就是你的 SQL 命令行了。试着敲一句：

```sql
SELECT 1 + 1;
```

按回车。它应该回你一个 `2`。恭喜，你的第一条 SQL 跑通了。

一些 psql 里好用的元命令（不是 SQL，是 psql 自己的命令，都以反斜杠开头）：

- `\dt` ：列出当前数据库里所有的表（还没建表时是空的）。
- `\d 表名` ：看某张表的结构（有哪些列、什么类型）。
- `\q` ：退出 psql。
- 每条 SQL 语句要以分号 `;` 结尾，psql 才会执行它。忘了分号它会一直等你继续输入。

> 💡 你可能会问：一定要用命令行吗？我看别人都用图形界面。
> 图形工具（比如 DBeaver、TablePlus、pgAdmin）当然可以，连接信息填：host = localhost，port = 5432，database = recon，user = postgres，password = recon123。但我建议你练习期就用 psql 命令行，因为 HackerRank OA 就是一个纯文本框让你敲 SQL，你越习惯「无提示、纯手写」，考试越稳。

用完不想留着？`docker stop recon-pg` 停掉，`docker start recon-pg` 再开起来（数据还在），`docker rm -f recon-pg` 彻底删掉容器（数据也没了，练习环境正好可以随时重来）。

---

## 1. 数据库、表、行、列到底是什么（对照 Excel 和 DataFrame）

在写查询之前，先把几个最基础的名词用你熟悉的东西锚定住。

| SQL 世界 | 你熟悉的对照物 | 一句话解释 |
|---|---|---|
| 数据库 database | 一整个 Excel 工作簿（.xlsx 文件） | 一堆相关的表放在一起 |
| 表 table | 工作簿里的一张 sheet | 一个二维的数据网格 |
| 行 row（也叫记录 record） | sheet 里的一行 | 一条数据，比如「AAPL 持仓 500 股」 |
| 列 column（也叫字段 field） | sheet 里的一列 | 一个属性，比如「股票代码」 |
| 列的类型 data type | Excel 里「这列是文本 / 数字 / 日期」 | 规定这一列只能装什么 |

如果你更习惯 Pandas：一张**表 ≈ 一个 DataFrame**，行 ≈ DataFrame 的行，列 ≈ DataFrame 的列，列的 dtype ≈ SQL 的 data type。区别在于：DataFrame 在内存里、临时的；SQL 表在磁盘上、持久的，而且**建表时就得先声明每列叫什么、是什么类型**，不能像 Pandas 那样随手加列。

**建你的第一张表。** 我们建一张「内部持仓表」，记录公司自己系统里的持仓：

```sql
CREATE TABLE internal_positions (
    account_id   TEXT,
    symbol       TEXT,
    quantity     INTEGER,
    settle_date  DATE
);
```

逐行看：

- `CREATE TABLE internal_positions (...)` ：建一张叫 internal_positions 的表，括号里列出它的列。
- `account_id TEXT` ：一列叫 account_id，类型是 TEXT（字符串，比如 "ACC001"）。
- `symbol TEXT` ：股票代码，字符串，比如 "AAPL"。
- `quantity INTEGER` ：持仓数量，整数（可正可负，负数代表做空 short）。
- `settle_date DATE` ：结算日，日期类型。后面第 8 节会重点用它做 T+1 过滤。

常见的几个类型你先记这些就够用了：`INTEGER`（整数）、`NUMERIC(18,2)`（带小数的精确数字，18 位总长、2 位小数，钱和价格用这个，别用会有精度误差的 FLOAT）、`TEXT`（字符串）、`DATE`（日期）、`TIMESTAMP`（日期 + 时间）、`BOOLEAN`（真 / 假）。

**往表里插几行数据：**

```sql
INSERT INTO internal_positions (account_id, symbol, quantity, settle_date) VALUES
('ACC001', 'AAPL', 500,  '2026-07-03'),
('ACC001', 'MSFT', 300,  '2026-07-03'),
('ACC002', 'TSLA', 100,  '2026-07-04'),
('ACC002', 'AAPL', -200, '2026-07-03');
```

- `INSERT INTO 表名 (列名列表) VALUES (...)` ：往表里插入行。
- 每一组括号是一行。字符串和日期要用单引号 `'...'` 包起来，整数不用。
- 注意 ACC002 的 AAPL 是 -200，负数表示这个账户在 AAPL 上是空头（借来卖出的）。

插完看一眼：`SELECT * FROM internal_positions;` 应该看到你插的 4 行。这就是你第一张有数据的表。

> 💡 你可能会问：为什么钱和数量不用 FLOAT？
> 因为 FLOAT 是二进制浮点，`0.1 + 0.2` 在它眼里不等于 `0.3`。对账最忌讳这种误差，两套账本来一样，因为浮点误差被你判成 break，就闹笑话了。凡是钱、价格、需要精确相等的，一律 `NUMERIC`。这点你在 C++ 写结算系统时应该深有体会。

---

## 2. SELECT / WHERE 基础：把数据「查出来」

SELECT 是 SQL 里你用得最多的一个字。它的意思就是「给我看」。

**最基础的查询：**

```sql
SELECT * FROM internal_positions;
```

- `SELECT *` ：`*` 表示「所有列」。
- `FROM internal_positions` ：从这张表查。

**只要某几列：**

```sql
SELECT symbol, quantity FROM internal_positions;
```

只返回 symbol 和 quantity 两列。这就像 Pandas 的 `df[['symbol', 'quantity']]`。

**用 WHERE 过滤行。** WHERE 是「筛选条件」，只保留满足条件的行，等价于 Pandas 的 `df[df['quantity'] > 0]`：

```sql
SELECT symbol, quantity
FROM internal_positions
WHERE quantity > 0;
```

这只返回多头持仓（数量为正）。示例输出：

| symbol | quantity |
|---|---|
| AAPL | 500 |
| MSFT | 300 |
| TSLA | 100 |

（ACC002 的 AAPL -200 因为不满足 `> 0` 被过滤掉了。）

WHERE 里能用的条件运算符：`=`（等于，注意 SQL 里判断相等是**一个**等号，不是 C++ 的 `==`）、`<> 或 !=`（不等于）、`> < >= <=`、`AND`、`OR`、`NOT`、`BETWEEN a AND b`、`IN (...)`、`LIKE '模式'`（模糊匹配）。

几个例子：

```sql
-- 账户 ACC001 里数量大于 400 的持仓
SELECT * FROM internal_positions
WHERE account_id = 'ACC001' AND quantity > 400;

-- symbol 是 AAPL 或 TSLA 的
SELECT * FROM internal_positions
WHERE symbol IN ('AAPL', 'TSLA');
```

**排序 ORDER BY 和取前几行 LIMIT：**

```sql
SELECT symbol, quantity
FROM internal_positions
ORDER BY quantity DESC
LIMIT 2;
```

- `ORDER BY quantity DESC` ：按 quantity 从大到小排（DESC = descending 降序，ASC = 升序，默认升序）。
- `LIMIT 2` ：只要前 2 行。

> 💡 接回你的 C++ 直觉：`ORDER BY quantity DESC LIMIT 2` 本质就是你手写的「排序后取 top 2」。你在 C++ 里可能用 priority queue 或者部分排序来取 top，SQL 里一句话搞定，数据库内部会挑高效的算法（可能就是堆排序），你不用操心。

一条完整 SELECT 的子句顺序（先背下来，很重要）：

```
SELECT   要哪些列
FROM     从哪张表
WHERE    过滤哪些行
GROUP BY 按什么分组
HAVING   过滤分组后的结果
ORDER BY 怎么排序
LIMIT    取几行
```

写的时候必须按这个顺序。但**数据库执行的顺序**不是这个，大致是 FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT。记住这个执行顺序，第 6 节讲 WHERE 和 HAVING 区别时会用到。

---

## 3. 对账要用到的两张表长什么样

现在正式搭建我们的对账场景。我们要两套账：一套是公司**内部**的持仓（上面已经建好 internal_positions），另一套是**托管行**记录的持仓。理想情况下两套账对每个账户、每个 symbol 记的数量应该一样。我们故意在数据里埋几个 break，好让你学会把它们揪出来。

先把内部表重建一遍（保证数据干净），再建托管行表：

```sql
DROP TABLE IF EXISTS internal_positions;
CREATE TABLE internal_positions (
    account_id   TEXT,
    symbol       TEXT,
    quantity     INTEGER,
    settle_date  DATE
);
INSERT INTO internal_positions (account_id, symbol, quantity, settle_date) VALUES
('ACC001', 'AAPL', 500,  '2026-07-03'),
('ACC001', 'MSFT', 300,  '2026-07-03'),
('ACC002', 'TSLA', 100,  '2026-07-04'),
('ACC002', 'AAPL', -200, '2026-07-03'),
('ACC003', 'NVDA', 150,  '2026-07-03');
```

```sql
DROP TABLE IF EXISTS custodian_positions;
CREATE TABLE custodian_positions (
    account_id   TEXT,
    symbol       TEXT,
    quantity     INTEGER,
    settle_date  DATE
);
INSERT INTO custodian_positions (account_id, symbol, quantity, settle_date) VALUES
('ACC001', 'AAPL', 500,  '2026-07-03'),   -- 一致
('ACC001', 'MSFT', 250,  '2026-07-03'),   -- 数量不一致！内部300，托管行250
('ACC002', 'TSLA', 100,  '2026-07-04'),   -- 一致
('ACC002', 'AAPL', -200, '2026-07-03'),   -- 一致
('ACC004', 'GOOG', 80,   '2026-07-03');   -- 托管行有、内部没有！
```

我先把埋进去的坑给你标出来（后面你要用 SQL 自己把它们找出来，而不是靠这份注释）：

- **ACC001 / MSFT**：内部记 300，托管行记 250。数量对不上，这是一个 break。
- **ACC003 / NVDA**：内部有 150 股，托管行**根本没这条记录**。内部有、托管行没有 → break。
- **ACC004 / GOOG**：托管行有 80 股，内部**根本没这条记录**。托管行有、内部没有 → break。
- 其余（ACC001/AAPL、ACC002/TSLA、ACC002/AAPL）两边一致，不是 break。

两张表的「共同列」是 `(account_id, symbol)` 这一对。对账的本质就是：**按这一对把两张表拼起来，然后看拼起来之后哪里对不上。** 「拼起来」这个动作，就是下一节的主角：JOIN。

> 💡 接回你的 C++ 直觉：这一步你太熟了。在 C++ 里对账你会怎么做？大概是：把内部持仓塞进一个 `hash_map<key, quantity>`，key 用 (account_id, symbol)，然后遍历托管行记录，去 hash_map 里 `find(key)`。找到了就比数量，找不到就是缺口。**JOIN 就是数据库替你做的这个 hash 匹配。** 你已经理解了 JOIN 的核心，只是没学过它的写法。

---

## 4. JOIN 全家桶：把两张表拼起来

JOIN 是这整篇最重要的概念，慢慢读。

**先建立最直白的画面。** 你有两张点名单：一张是「上午到场名单」，一张是「下午到场名单」。JOIN 就是把这两张名单按「学号」拼在一起，让你能同时看到「张三上午在不在、下午在不在」。区别只在于：**拼的时候，只在一张单子上出现的人，要不要留下来。** 这个「要不要留」的策略不同，就分出了 INNER / LEFT / RIGHT / FULL OUTER 四种 JOIN。

我们的两张「点名单」就是 internal_positions（内部）和 custodian_positions（托管行），拼接的「学号」是 `(account_id, symbol)`。

### 4.1 INNER JOIN：只留两边都有的

INNER JOIN = 只保留在**两张表里都能匹配上**的行。类比点名单：只列出「上午和下午都到了」的人。

```sql
SELECT
    i.account_id,
    i.symbol,
    i.quantity AS internal_qty,
    c.quantity AS custodian_qty
FROM internal_positions AS i
INNER JOIN custodian_positions AS c
    ON i.account_id = c.account_id
   AND i.symbol     = c.symbol;
```

逐行拆解：

- `FROM internal_positions AS i` ：从内部表查，给它起个别名 i（alias），后面用 `i.列名` 引用它的列。别名是为了少打字、也为了在两张表有同名列时区分清楚。
- `INNER JOIN custodian_positions AS c` ：把托管行表（别名 c）拼进来。
- `ON i.account_id = c.account_id AND i.symbol = c.symbol` ：**这是 JOIN 的核心，拼接条件**。意思是「当两表的 account_id 相等**并且** symbol 也相等时，认为这两行是同一个东西，拼成一行」。
- `SELECT i.quantity AS internal_qty, c.quantity AS custodian_qty` ：拼成一行后，我把内部数量和托管行数量并排放出来，`AS` 给列改个显示名，好区分谁是谁。

示例输出（只有两边都匹配上的才出现）：

| account_id | symbol | internal_qty | custodian_qty |
|---|---|---|---|
| ACC001 | AAPL | 500 | 500 |
| ACC001 | MSFT | 300 | 250 |
| ACC002 | TSLA | 100 | 100 |
| ACC002 | AAPL | -200 | -200 |

注意结果里：

- ACC003 / NVDA **不见了**（内部有、托管行没有，匹配不上，被 INNER JOIN 剔除）。
- ACC004 / GOOG **也不见了**（托管行有、内部没有，同样被剔除）。

这就暴露了 INNER JOIN 的关键局限：**它会悄悄丢掉「只在一边出现」的行**。而对账最要命的 break 恰恰就是「只在一边出现」的那些！所以，

> ⚠️ 常见误区（这是对账新手第一大坑）：用 INNER JOIN 找 break。你会漏掉整整一类最严重的 break，「一边有、另一边完全没有」的记录。INNER JOIN 只能帮你抓到「两边都有但数量不一致」这一种 break（比如 MSFT 300 vs 250），抓不到「凭空多出来 / 凭空少一条」。记住这个教训，它直接决定了后面为什么必须用 FULL OUTER JOIN。

那 INNER JOIN 什么时候用？当你**只关心两边都存在的记录、想比对它们的差异**时。比如你只想看「双方都确认存在、但数量对不上」的持仓，INNER JOIN 就够了，而且它最快。

### 4.2 LEFT JOIN：保住左表所有行，右表没有的填 NULL

LEFT JOIN（全称 LEFT OUTER JOIN）= **左表的每一行都保留**，右表能匹配上就拼上，匹配不上就在右表那几列填 NULL（空）。

类比点名单：以「上午名单」为准，把每个上午到的人都列出来；如果他下午也到了，就把下午信息补上；如果他下午没到，下午那几列就空着。

```sql
SELECT
    i.account_id,
    i.symbol,
    i.quantity AS internal_qty,
    c.quantity AS custodian_qty
FROM internal_positions AS i
LEFT JOIN custodian_positions AS c
    ON i.account_id = c.account_id
   AND i.symbol     = c.symbol;
```

跟 INNER JOIN 唯一的区别就是把 `INNER` 换成了 `LEFT`。左表是 FROM 后面那张（internal_positions）。

示例输出：

| account_id | symbol | internal_qty | custodian_qty |
|---|---|---|---|
| ACC001 | AAPL | 500 | 500 |
| ACC001 | MSFT | 300 | 250 |
| ACC002 | TSLA | 100 | 100 |
| ACC002 | AAPL | -200 | -200 |
| ACC003 | NVDA | 150 | **NULL** |

看第 5 行：ACC003 / NVDA 内部有 150，托管行没有，所以 custodian_qty 是 **NULL**。这一行 INNER JOIN 会丢掉，LEFT JOIN 保住了。

**这直接给了你找一类 break 的方法：「内部有、托管行没有」= LEFT JOIN 之后 custodian_qty IS NULL。**

```sql
SELECT
    i.account_id,
    i.symbol,
    i.quantity AS internal_qty
FROM internal_positions AS i
LEFT JOIN custodian_positions AS c
    ON i.account_id = c.account_id
   AND i.symbol     = c.symbol
WHERE c.quantity IS NULL;
```

输出：

| account_id | symbol | internal_qty |
|---|---|---|
| ACC003 | NVDA | 150 |

这就是「内部记了、但托管行完全没有」的 break，可能是托管行漏记，也可能是内部误记。这正是任务描述里要求你能写的那句：「用一句 SQL 找出内部有、托管行没有的 break」。核心套路就是 **LEFT JOIN + WHERE 右表关键列 IS NULL**。

> ⚠️ 常见误区：写成 `WHERE c.quantity = NULL`。SQL 里判断「是不是空」不能用 `=`，必须用 `IS NULL` / `IS NOT NULL`。原因下一节（第 5 节）会讲透。现在先记住：跟 NULL 比较永远用 IS，不用 =。

### 4.3 RIGHT JOIN：保住右表所有行

RIGHT JOIN 是 LEFT JOIN 的镜像：**右表每一行都保留**，左表匹配不上就填 NULL。

用它可以抓另一类 break：「托管行有、内部没有」。

```sql
SELECT
    c.account_id,
    c.symbol,
    c.quantity AS custodian_qty
FROM internal_positions AS i
RIGHT JOIN custodian_positions AS c
    ON i.account_id = c.account_id
   AND i.symbol     = c.symbol
WHERE i.quantity IS NULL;
```

输出：

| account_id | symbol | custodian_qty |
|---|---|---|
| ACC004 | GOOG | 80 |

ACC004 / GOOG 托管行有 80、内部没有，被抓出来了。

> 💡 你可能会问：RIGHT JOIN 好像和 LEFT JOIN 就是反过来？
> 对。任何 `A RIGHT JOIN B` 都能写成 `B LEFT JOIN A`。实践中大部分人只写 LEFT JOIN，需要反向时就把两张表位置调一下，读起来更顺（人习惯从左往右读，「以谁为准」放左边最清楚）。RIGHT JOIN 你知道它存在、看得懂就行。

### 4.4 FULL OUTER JOIN：两边所有行都保留，这才是对账的正确姿势

FULL OUTER JOIN = **两张表的所有行都保留**。能匹配上的拼在一起；只在左表的，右表列填 NULL；只在右表的，左表列填 NULL。

类比点名单：把上午名单和下午名单**合起来去重**，列出所有出现过的人，每个人标清「上午在不在、下午在不在」。这样你一眼能看出三类人：两次都在的、只上午在的、只下午在的。

**为什么对账必须用它？** 因为 break 有三种形态，你需要一次性全抓到：

1. 两边都有、但数量不一致（MSFT 300 vs 250）。
2. 内部有、托管行没有（NVDA）。
3. 托管行有、内部没有（GOOG）。

INNER JOIN 只抓得到第 1 种；LEFT JOIN 抓 1、2；RIGHT JOIN 抓 1、3。**只有 FULL OUTER JOIN 一次抓全 1、2、3。** 这就是为什么它是对账的标准姿势。

```sql
SELECT
    COALESCE(i.account_id, c.account_id) AS account_id,
    COALESCE(i.symbol,     c.symbol)     AS symbol,
    i.quantity AS internal_qty,
    c.quantity AS custodian_qty
FROM internal_positions AS i
FULL OUTER JOIN custodian_positions AS c
    ON i.account_id = c.account_id
   AND i.symbol     = c.symbol;
```

这里出现一个新函数 `COALESCE`，先解释它，因为 FULL OUTER JOIN 离不开它。

- `COALESCE(a, b)` ：返回第一个不是 NULL 的值。如果 a 不为空就返回 a，否则返回 b。
- 为什么这里要用它？因为 FULL OUTER JOIN 里，一行可能是「只在内部有」（那 `c.account_id` 是 NULL）或「只在托管行有」（那 `i.account_id` 是 NULL）。我想要一个「不管这行来自哪边，都能拿到账户号」的列，所以用 `COALESCE(i.account_id, c.account_id)`：内部有就用内部的，内部没有就退而用托管行的。symbol 同理。

示例输出：

| account_id | symbol | internal_qty | custodian_qty |
|---|---|---|---|
| ACC001 | AAPL | 500 | 500 |
| ACC001 | MSFT | 300 | 250 |
| ACC002 | TSLA | 100 | 100 |
| ACC002 | AAPL | -200 | -200 |
| ACC003 | NVDA | 150 | **NULL** |
| ACC004 | GOOG | **NULL** | 80 |

看，六行全在。所有 break 都可见了：MSFT 数量不符、NVDA 内部独有、GOOG 托管行独有。

**现在一句 SQL 把所有 break 全找出来**（数量不一致 OR 任一边缺失）：

```sql
SELECT
    COALESCE(i.account_id, c.account_id) AS account_id,
    COALESCE(i.symbol,     c.symbol)     AS symbol,
    i.quantity AS internal_qty,
    c.quantity AS custodian_qty
FROM internal_positions AS i
FULL OUTER JOIN custodian_positions AS c
    ON i.account_id = c.account_id
   AND i.symbol     = c.symbol
WHERE i.quantity IS DISTINCT FROM c.quantity;
```

关键在 `WHERE i.quantity IS DISTINCT FROM c.quantity`。

- `IS DISTINCT FROM` 是「安全的不等于」，它把 NULL 也当成一个普通值来比。
- 普通的 `<>`（不等于）遇到 NULL 会返回 NULL（既不是真也不是假），结果那一行就不会被 WHERE 选中，于是 NVDA、GOOG 这种「一边是 NULL」的 break 反而被漏掉。
- `IS DISTINCT FROM` 修好了这个坑：`300 IS DISTINCT FROM 250` → 真；`150 IS DISTINCT FROM NULL` → 真；`500 IS DISTINCT FROM 500` → 假。正好只留下三类 break。

输出（正是我们埋的三个 break）：

| account_id | symbol | internal_qty | custodian_qty |
|---|---|---|---|
| ACC001 | MSFT | 300 | 250 |
| ACC003 | NVDA | 150 | NULL |
| ACC004 | GOOG | NULL | 80 |

**这一条查询，就是对账的心脏。** 后面第 10 节的综合练习会在它基础上加容差和排序，但骨架就是这个：FULL OUTER JOIN 两张表 + WHERE 找出对不上的。

> ✅ 小测验：如果我把上面的 `FULL OUTER JOIN` 改成 `INNER JOIN`，其他不变，输出会变成什么？
>
> 答案：只剩 `ACC001 / MSFT / 300 / 250` 一行。因为 INNER JOIN 已经把 NVDA 和 GOOG 这两行丢掉了（它们匹配不上），`WHERE` 根本没机会看到它们。这再次说明：找缺失型 break，JOIN 的类型选错，后面 WHERE 写得再对也白搭。

四种 JOIN 一张表总结：

| JOIN 类型 | 保留哪些行 | 点名单类比 | 对账中抓哪类 break |
|---|---|---|---|
| INNER | 只保留两边都匹配上的 | 上午和下午都到的 | 只抓「都有但数量不符」 |
| LEFT | 左表全保留 | 以上午名单为准 | 「都有但不符」+「内部独有」 |
| RIGHT | 右表全保留 | 以下午名单为准 | 「都有但不符」+「托管行独有」 |
| FULL OUTER | 两边全保留 | 两份名单合并去重 | 三类全抓（对账首选） |

---

## 5. NULL 是什么，JOIN 之后的 NULL 在告诉你什么

NULL 值得单独一节，因为它是对账里「信息量最大」的东西，而且新手最容易在它上面栽跟头。

**NULL 不是 0，也不是空字符串，它表示「不知道 / 不存在 / 没有值」。** 用生活类比：调查问卷里「你的年龄」这一栏，有人填了 25（有值），有人填了 0（这个人真的 0 岁？不太可能，可能是乱填），有人干脆没填（这才是 NULL，我们不知道他多大）。0 是一个确定的值，NULL 是「压根没有值」。

在对账里，**JOIN 之后出现的 NULL 是一个非常明确的信号：这一边根本没有这条记录。**

- LEFT JOIN 后 `custodian_qty` 是 NULL → 托管行没有这条持仓（内部单方面记了）。
- FULL OUTER JOIN 后 `internal_qty` 是 NULL → 内部没有这条持仓（托管行单方面记了）。

所以你读 JOIN 结果时，看到 NULL 不要慌，要立刻翻译成业务含义：「哦，这边漏记了 / 那边多记了」。NULL 出现的位置，直接告诉你 break 在哪一侧。

**NULL 的三个反直觉行为，务必记住：**

1. **任何东西和 NULL 用 `=` 比较，结果都是 NULL（不是真也不是假）。** 所以 `WHERE c.quantity = NULL` 永远选不出任何行。判断空要用 `IS NULL` / `IS NOT NULL`。
2. **NULL 参与算术，结果还是 NULL。** `500 + NULL = NULL`，`NULL * 2 = NULL`。所以做减法算差异前，通常先用 `COALESCE(x, 0)` 把 NULL 当成 0 处理（见下）。
3. **聚合函数（第 6 节）默认忽略 NULL。** `SUM` / `AVG` 会跳过 NULL 值，`COUNT(列名)` 不数 NULL，但 `COUNT(*)` 数所有行。

**对账里的实战：算两边数量差。** 你想加一列 `diff = internal_qty - custodian_qty`。但如果直接减，NVDA 那行 `150 - NULL` 会算出 NULL，你就不知道差多少了。正确做法是先把 NULL 补成 0：

```sql
SELECT
    COALESCE(i.account_id, c.account_id) AS account_id,
    COALESCE(i.symbol,     c.symbol)     AS symbol,
    COALESCE(i.quantity, 0) AS internal_qty,
    COALESCE(c.quantity, 0) AS custodian_qty,
    COALESCE(i.quantity, 0) - COALESCE(c.quantity, 0) AS diff
FROM internal_positions AS i
FULL OUTER JOIN custodian_positions AS c
    ON i.account_id = c.account_id AND i.symbol = c.symbol
WHERE i.quantity IS DISTINCT FROM c.quantity;
```

输出：

| account_id | symbol | internal_qty | custodian_qty | diff |
|---|---|---|---|---|
| ACC001 | MSFT | 300 | 250 | 50 |
| ACC003 | NVDA | 150 | 0 | 150 |
| ACC004 | GOOG | 0 | 80 | -80 |

现在 diff 列就是「这个 break 的量级」：MSFT 差 50，NVDA 差 150（内部多，托管行为 0），GOOG 差 -80（内部为 0，托管行多）。`diff` 的正负还告诉你差在哪一边。这个 diff 就是后面按风险排序的依据。

> ✅ 小测验：`COUNT(*)` 和 `COUNT(custodian_qty)` 在上面这张结果表上分别是多少？
>
> 答案：`COUNT(*)` = 3（数所有行）。`COUNT(custodian_qty)` = 3（这里没有真正的 NULL，因为我们已经 COALESCE 成 0 了）。但如果是在没 COALESCE 的原始 FULL OUTER JOIN 结果上，`COUNT(custodian_qty)` 会是 5（GOOG 那行原本是 NULL 被跳过，剩 5 行有值）而 `COUNT(*)` 是 6。这就是「COUNT 列名不数 NULL、COUNT(*) 数所有行」的区别。

---

## 6. GROUP BY + 聚合函数 + HAVING：把 break 汇总成风险敞口

到目前为止我们是「逐条」看 break。但运营团队经常要的是「汇总视角」：**每个账户一共有多少个 break、总敞口多大**，好决定先查哪个账户。这就要用 GROUP BY 聚合。

**先理解 GROUP BY。** 大白话：把行按某一列的值分成若干堆，每一堆算出一个汇总数字。类比：一叠交易小票，按「账户」分堆，每堆数一下张数、加一下总金额。这跟 Pandas 的 `df.groupby('account_id').agg(...)` 是一模一样的思路。

**接回你的 C++ 直觉：** GROUP BY 就是你在 C++ 里遍历一遍数据、用一个 `map<account_id, 累加器>` 边扫边累加的过程。你手写的那个累加循环，SQL 用一句 GROUP BY 表达。

**聚合函数**（对每一堆算一个数）常用五个：`COUNT(...)` 计数、`SUM(...)` 求和、`AVG(...)` 平均、`MAX(...)` 最大、`MIN(...)` 最小。

先建一个只含 break 的中间结果（用第 5 节那条查询），然后按账户汇总。为了讲清楚，我们先把「每个账户内部持仓的总敞口」算出来（一个更简单的 GROUP BY 例子）：

```sql
SELECT
    account_id,
    COUNT(*)            AS num_positions,
    SUM(ABS(quantity))  AS gross_exposure
FROM internal_positions
GROUP BY account_id;
```

逐句看：

- `GROUP BY account_id` ：按账户分堆。ACC001 一堆、ACC002 一堆、ACC003 一堆。
- `COUNT(*)` ：每堆有几行（这个账户有几个持仓）。
- `SUM(ABS(quantity))` ：`ABS` 取绝对值（做空 -200 的敞口也是 200），再求和，得到这个账户的总敞口（gross exposure，中后台常看的风险指标）。
- SELECT 里只能放两种东西：**要么是 GROUP BY 里的列**（account_id），**要么是聚合函数**（COUNT、SUM）。这是铁律，下面误区会讲。

输出：

| account_id | num_positions | gross_exposure |
|---|---|---|
| ACC001 | 2 | 800 |
| ACC002 | 2 | 300 |
| ACC003 | 1 | 150 |

> ⚠️ 常见误区：`SELECT account_id, symbol, SUM(quantity) FROM ... GROUP BY account_id`。这会报错，因为 symbol 既不在 GROUP BY 里、也不是聚合函数。想象一下：ACC001 分成一堆后，这堆里有 AAPL 和 MSFT 两个 symbol，数据库不知道该显示哪个。要么把 symbol 也加进 GROUP BY（那就变成按「账户 + symbol」分堆），要么对它用聚合（比如 `MAX(symbol)`）。记住：**SELECT 里的非聚合列，必须全部出现在 GROUP BY 里。**

**现在用 HAVING 过滤分组结果。** 假设你只关心「敞口超过 400 的账户」。你可能想写 `WHERE gross_exposure > 400`，错，WHERE 管不了聚合结果。要用 HAVING：

```sql
SELECT
    account_id,
    COUNT(*)            AS num_positions,
    SUM(ABS(quantity))  AS gross_exposure
FROM internal_positions
GROUP BY account_id
HAVING SUM(ABS(quantity)) > 400;
```

输出只剩：

| account_id | num_positions | gross_exposure |
|---|---|---|
| ACC001 | 2 | 800 |

**WHERE 和 HAVING 的区别（面试高频，必须说清）：**

- **WHERE 在分组之前过滤单行**，它决定「哪些行参与分组」。它看不到聚合结果，因为那时候还没算聚合。
- **HAVING 在分组之后过滤分组**，它决定「哪些堆留下来」。它能用聚合结果。

回忆第 2 节的执行顺序：FROM → **WHERE** → GROUP BY → **HAVING** → SELECT。WHERE 先跑（还没分组），HAVING 后跑（分好组算完聚合了）。所以「过滤原始行」用 WHERE，「过滤聚合后的组」用 HAVING。

一条同时用到两者的例子，「只统计 settle_date 在 7 月 3 日结算的持仓，按账户汇总，只保留敞口大于 400 的账户」：

```sql
SELECT
    account_id,
    SUM(ABS(quantity)) AS gross_exposure
FROM internal_positions
WHERE settle_date = '2026-07-03'        -- 先过滤行：只要7月3日结算的
GROUP BY account_id
HAVING SUM(ABS(quantity)) > 400;        -- 再过滤组：只要敞口>400的账户
```

`WHERE settle_date = '2026-07-03'` 把 ACC002 的 TSLA（7 月 4 日结算）先剔除了，然后才分组聚合，最后 HAVING 挑敞口大的组。这个「WHERE 管行、HAVING 管组」的分工，务必内化。

> ✅ 小测验：想「找出至少有 2 个持仓的账户」，条件应该放 WHERE 还是 HAVING？
>
> 答案：HAVING。因为「有几个持仓」是 `COUNT(*)` 聚合结果，只有分组后才知道。写法：`... GROUP BY account_id HAVING COUNT(*) >= 2;`。如果错放进 WHERE，数据库会报错，因为 WHERE 阶段还没有 COUNT。

---

## 7. 窗口函数 ROW_NUMBER()：在组内排名，但不合并行

GROUP BY 有个「副作用」：它把每一堆压成一行，你就看不到堆里的明细了。但有时候你想要「既保留每一行明细，又给它在组内排个名次」。这就是**窗口函数**登场的地方。

**大白话：** 窗口函数就像「一边保留所有行，一边在旁边开一个小窗口，往窗口里看一眼周围的行，算个名次 / 累计值 / 排名」。它不合并行，行数不变，只是多给你一列信息。

**接回你的 C++ 直觉：** `ROW_NUMBER() OVER (ORDER BY diff DESC)` 本质就是「先按 diff 降序排，再从 1 开始编号」，正是你手写排序后打序号的操作。加了 `PARTITION BY account_id`，就是「先按账户分组，每组内部各自从 1 开始编号」，相当于你在每个 hash bucket 里各排各的。

最常用的窗口函数就是 `ROW_NUMBER()`，给每行一个从 1 开始的序号。任务要求你会用它做排名，我们就用它给 break 按严重程度排名。

先给每个 break 的严重程度定义为 `ABS(diff)`（差得越多越严重），然后在**每个账户内部**排名：

```sql
SELECT
    account_id,
    symbol,
    diff,
    ROW_NUMBER() OVER (
        PARTITION BY account_id
        ORDER BY ABS(diff) DESC
    ) AS rank_in_account
FROM (
    SELECT
        COALESCE(i.account_id, c.account_id) AS account_id,
        COALESCE(i.symbol,     c.symbol)     AS symbol,
        COALESCE(i.quantity, 0) - COALESCE(c.quantity, 0) AS diff
    FROM internal_positions AS i
    FULL OUTER JOIN custodian_positions AS c
        ON i.account_id = c.account_id AND i.symbol = c.symbol
    WHERE i.quantity IS DISTINCT FROM c.quantity
) AS breaks;
```

先别被外层内层吓到，从里往外读：

- 里面括号那段（起了别名 `breaks`）就是第 5 节算好的 break 列表，有 account_id、symbol、diff。这种「把一条查询当成一张临时表塞进 FROM」的写法叫**子查询**，第 9 节会教你用更清爽的 CTE 写法替代它。
- 外层的重点是 `ROW_NUMBER() OVER (...)`：
  - `OVER (...)` ：告诉数据库「这是个窗口函数，窗口的规则在括号里」。
  - `PARTITION BY account_id` ：按账户分区，每个账户内部各自排名（类似 GROUP BY 的分堆，但不合并行）。
  - `ORDER BY ABS(diff) DESC` ：在每个分区内，按差异绝对值从大到小排。
  - `ROW_NUMBER()` ：按上面这个排序，从 1 开始编号。
  - `AS rank_in_account` ：这一列叫账户内排名。

我们的数据每个账户各只有一个 break，所以排名都会是 1，看不出效果。为了让你看到 ROW_NUMBER 的威力，假设 ACC001 有两个 break（MSFT diff=50，AAPL diff=30），输出会是：

| account_id | symbol | diff | rank_in_account |
|---|---|---|---|
| ACC001 | MSFT | 50 | 1 |
| ACC001 | AAPL | 30 | 2 |
| ACC003 | NVDA | 150 | 1 |
| ACC004 | GOOG | -80 | 1 |

看 ACC001：MSFT 差 50 排第 1，AAPL 差 30 排第 2。每个账户内部独立从 1 开始。

**窗口函数最经典的用途：取每组的 top N。** 比如「每个账户最严重的那个 break」，就是外面再套一层，取 `rank_in_account = 1`：

```sql
SELECT * FROM (
    ... 上面那段带 ROW_NUMBER 的查询 ...
) AS ranked
WHERE rank_in_account = 1;
```

> 💡 你可能会问：ROW_NUMBER、RANK、DENSE_RANK 有啥区别？OA 会考吗？
> 简单记：`ROW_NUMBER` 硬编号，就算并列也强行给不同号（1,2,3,4）。`RANK` 遇并列给相同号、然后跳号（1,1,3,4）。`DENSE_RANK` 遇并列给相同号、不跳号（1,1,2,3）。OA 里「取每组第 N 名」几乎都用 ROW_NUMBER。你先把 ROW_NUMBER 用熟，另两个知道差别即可。

> ⚠️ 常见误区：想在 `WHERE` 里直接写 `WHERE ROW_NUMBER() OVER (...) = 1`。不行。窗口函数在 SELECT 阶段才计算，比 WHERE 晚（回忆执行顺序）。所以必须像上面那样「先在子查询 / CTE 里算出排名列，再在外层 WHERE 里过滤」。这是窗口函数题的固定套路，记死它。

---

## 8. 日期函数 + T+1 结算日过滤：把业务规则写进 WHERE

金融数据离不开日期，对账尤其绕不开「结算周期」。这一节把一个真实业务规则，T+1 结算，翻译成 SQL 过滤条件。

**先讲 T+1 是什么（大白话）。** 你今天（trade date，交易日 T）下单买了股票，钱和股票不是当天就一手交钱一手交货，而是要过一段时间才真正「结算（settle）」完成交割。美股现在是 T+1，即交易日之后的 1 个工作日结算。所以「今天成交、但还没结算」的持仓，和「已经结算」的持仓，在账上的状态是不同的。

**这对对账意味着什么？** 你在某个「截止日 as_of_date」做对账时，通常只应该比对**已经结算完成的**持仓（`settle_date <= as_of_date`）。那些还没到结算日的（`settle_date > as_of_date`），两边系统可能有正常的时间差，还没同步，硬比会产生一堆「假 break」。所以过滤掉未结算的，是对账前的标准清洗步骤。

**这就是任务里说的：把 domain 规则变成 WHERE 条件 `WHERE settle_date <= as_of_date`。** 我们来写。

先看几个日期基本操作：

```sql
-- 今天的日期
SELECT CURRENT_DATE;

-- 日期可以直接加减天数
SELECT CURRENT_DATE + 1;            -- 明天
SELECT DATE '2026-07-03' + 1;       -- 2026-07-04（T+1）

-- 从日期里抽取年 / 月 / 星期几
SELECT EXTRACT(DOW FROM DATE '2026-07-03');   -- DOW=day of week，0=周日
```

现在做「as_of 截止过滤」。假设今天是对账截止日 2026-07-03，只保留已结算持仓：

```sql
SELECT account_id, symbol, quantity, settle_date
FROM internal_positions
WHERE settle_date <= DATE '2026-07-03';
```

这会把 ACC002 的 TSLA（settle_date 是 2026-07-04，未来，还没结算）过滤掉，只留下 settle_date 在 7 月 3 日或更早的、已结算的持仓。

把它接进对账查询，**对账前，两张表都先按 as_of 过滤**，只对已结算的部分做 FULL OUTER JOIN：

```sql
SELECT
    COALESCE(i.account_id, c.account_id) AS account_id,
    COALESCE(i.symbol,     c.symbol)     AS symbol,
    COALESCE(i.quantity, 0) - COALESCE(c.quantity, 0) AS diff
FROM
    (SELECT * FROM internal_positions  WHERE settle_date <= DATE '2026-07-03') AS i
FULL OUTER JOIN
    (SELECT * FROM custodian_positions WHERE settle_date <= DATE '2026-07-03') AS c
    ON i.account_id = c.account_id AND i.symbol = c.symbol
WHERE i.quantity IS DISTINCT FROM c.quantity;
```

注意这里 FROM 和 JOIN 后面各接了一个「先过滤好的子查询」，保证进 JOIN 的两边都只有已结算数据。这种「先各自清洗、再 JOIN」的分层思路，正是第 9 节 CTE 要帮你写得更清爽的。

> 💡 你可能会问：为什么把 `settle_date <= ...` 放在子查询里，而不是放在最外层的 WHERE 里？
> 因为在 FULL OUTER JOIN 里，最外层 WHERE 引用 `i.settle_date` 会有 NULL 陷阱：对「只在托管行有」的行，`i.settle_date` 是 NULL，`NULL <= 日期` 是 NULL（既不真也不假），那一行会被 WHERE 意外剔除，你就漏了 GOOG 这种 break。在 JOIN 之前、各自表内先过滤，就没有 NULL 问题，最干净。这个坑很隐蔽，记住「过滤条件尽量在 JOIN 前、在各自表上做」。

> ✅ 小测验：美股 T+1，某笔交易的 trade_date 是周五 2026-07-03，它的 settle_date 大概是哪天？（先不考虑假日）
>
> 答案：不是 7 月 4 日（周六）。T+1 指 1 个**工作日**，跳过周末，所以结算日是下周一 7 月 6 日。真实系统里还要跳过交易所假日。这也是为什么日期过滤在金融里不能简单 `+1`，要用交易日历。OA 一般不会考到假日日历，但你知道这个 nuance 能在面试里加分。

---

## 9. CTE（WITH）：把复杂查询拆成看得懂的几步

前面第 7、8 节你已经看到查询开始套娃了（子查询套子查询）。当逻辑一多，嵌套子查询会变得像一团乱麻，从里往外读很累。CTE 就是来救这个的。

**CTE（Common Table Expression）大白话：** 用 `WITH 名字 AS (一段查询)` 先给一段查询起个名字，当成一张「临时表」，后面就能像用普通表一样用这个名字。它把「一大坨嵌套」拆成「先算这个、再算那个、最后组合」的顺序步骤，读起来像读代码里的几个变量赋值。

**接回你的编程直觉：** CTE 就是 SQL 里的「中间变量」。你在 C++ / Python 里不会把所有逻辑塞进一个巨长表达式，而是拆成 `step1 = ...; step2 = f(step1); result = g(step2);`。CTE 让 SQL 也能这么写。它对结果没有性能负担的心理压力时，优先用它，可读性提升巨大。

把第 8 节那个套娃查询用 CTE 重写，你感受下清爽多少：

```sql
WITH
-- 第一步：内部表，只留已结算
internal_settled AS (
    SELECT * FROM internal_positions
    WHERE settle_date <= DATE '2026-07-03'
),
-- 第二步：托管行表，只留已结算
custodian_settled AS (
    SELECT * FROM custodian_positions
    WHERE settle_date <= DATE '2026-07-03'
),
-- 第三步：两边对齐拼起来，算差异
joined AS (
    SELECT
        COALESCE(i.account_id, c.account_id) AS account_id,
        COALESCE(i.symbol,     c.symbol)     AS symbol,
        COALESCE(i.quantity, 0) AS internal_qty,
        COALESCE(c.quantity, 0) AS custodian_qty,
        COALESCE(i.quantity, 0) - COALESCE(c.quantity, 0) AS diff
    FROM internal_settled  AS i
    FULL OUTER JOIN custodian_settled AS c
        ON i.account_id = c.account_id AND i.symbol = c.symbol
)
-- 最后一步：从对齐结果里挑出 break
SELECT *
FROM joined
WHERE internal_qty <> custodian_qty
ORDER BY ABS(diff) DESC;
```

读法：`WITH` 后面用逗号隔开定义了三个「临时表」（internal_settled、custodian_settled、joined），每个都是一小段独立、好懂的查询。最后一条正式的 SELECT 直接从 `joined` 里挑 break。整个查询从上往下读，就是「清洗内部 → 清洗托管行 → 拼接算差异 → 挑出 break 排序」四个清楚的步骤。

对比第 8 节那个从里往外读的套娃版，同样的逻辑，CTE 版一眼就看懂在干嘛。**以后凡是查询超过一层嵌套，就用 CTE。** HackerRank 复杂题、面试白板题，用 CTE 写出来的人一看就专业。

> 💡 你可能会问：CTE 和子查询有性能差别吗？
> 在 PostgreSQL 里，现代版本（12+）会智能地把简单 CTE 内联优化，绝大多数情况下性能和子查询没差别，你放心用可读性换清晰度。真到了要抠性能才需要研究，那属于我们**明确不教**的调优范畴（岗位 JD 不要求，别在这上面浪费时间）。

---

## 10. 综合练习：一条完整的对账查询

现在把学到的全部拼起来，做一件真实运营会做的事：

> **需求：** 以 2026-07-03 为截止日，对内部持仓和托管行持仓做对账。只比对已结算持仓（T+1 规则）。两边数量差的**绝对值超过容差 tolerance = 10** 才算真正的 break（小于等于 10 的微小差异视为可接受，不报）。输出每个 break 的账户、symbol、两边数量、差异、以及一个风险等级，最后**按差异绝对值从大到小排序**（最该先查的排最前），并在每个账户内部给 break 编号。

自己先动手写，卡住了再看下面的参考答案。

**参考答案：**

```sql
WITH
-- 步骤1：内部表，按 as_of 只留已结算
internal_settled AS (
    SELECT account_id, symbol, quantity
    FROM internal_positions
    WHERE settle_date <= DATE '2026-07-03'
),
-- 步骤2：托管行表，同样只留已结算
custodian_settled AS (
    SELECT account_id, symbol, quantity
    FROM custodian_positions
    WHERE settle_date <= DATE '2026-07-03'
),
-- 步骤3：FULL OUTER JOIN 对齐两边，NULL 补 0，算差异
recon AS (
    SELECT
        COALESCE(i.account_id, c.account_id)              AS account_id,
        COALESCE(i.symbol,     c.symbol)                  AS symbol,
        COALESCE(i.quantity, 0)                           AS internal_qty,
        COALESCE(c.quantity, 0)                           AS custodian_qty,
        COALESCE(i.quantity, 0) - COALESCE(c.quantity, 0) AS diff
    FROM internal_settled  AS i
    FULL OUTER JOIN custodian_settled AS c
        ON i.account_id = c.account_id
       AND i.symbol     = c.symbol
),
-- 步骤4：只留差异绝对值 > 容差10 的真 break，并打风险等级
breaks AS (
    SELECT
        account_id,
        symbol,
        internal_qty,
        custodian_qty,
        diff,
        CASE
            WHEN internal_qty = 0 OR custodian_qty = 0 THEN 'MISSING'  -- 一边完全缺失
            WHEN ABS(diff) >= 100                      THEN 'HIGH'      -- 数量差很大
            ELSE                                            'MEDIUM'
        END AS risk_level
    FROM recon
    WHERE ABS(diff) > 10   -- 容差：差异<=10 视为可接受，不算 break
)
-- 步骤5：排序 + 账户内编号
SELECT
    account_id,
    symbol,
    internal_qty,
    custodian_qty,
    diff,
    risk_level,
    ROW_NUMBER() OVER (
        PARTITION BY account_id
        ORDER BY ABS(diff) DESC
    ) AS rank_in_account
FROM breaks
ORDER BY ABS(diff) DESC;
```

**逐步讲解（每一步用到了哪个知识点）：**

- **步骤 1、2（第 8 节 T+1 过滤）**：两张表各自先 `WHERE settle_date <= '2026-07-03'`，滤掉未结算的（ACC002 的 TSLA 被滤掉），避免时间差造成的假 break。放在 JOIN 之前、各自表上过滤，避开 NULL 陷阱。
- **步骤 3（第 4.4 节 FULL OUTER JOIN + 第 4/5 节 COALESCE）**：用 FULL OUTER JOIN 保住两边所有行，一次抓全三类 break。`COALESCE(..., 0)` 把缺失侧的 NULL 补成 0，好算 diff。
- **步骤 4（第 2 节 WHERE + 新的 CASE + 容差业务规则）**：`WHERE ABS(diff) > 10` 落实容差规则，把微小差异放过。`CASE WHEN ... THEN ... ELSE ... END` 是 SQL 的 if-else，给每个 break 打标签：任一边为 0 说明整条缺失，标 MISSING；差得≥100 标 HIGH；其余 MEDIUM。CASE 你没正式学过，但一看就懂，它就是分支判断。
- **步骤 5（第 7 节窗口函数 + 第 2 节 ORDER BY）**：`ROW_NUMBER() OVER (PARTITION BY account_id ORDER BY ABS(diff) DESC)` 在每个账户内按严重度编号。最外层 `ORDER BY ABS(diff) DESC` 让全局最严重的 break 排最前，运营照着从上往下查。

**在我们的数据上，这条查询输出：**

| account_id | symbol | internal_qty | custodian_qty | diff | risk_level | rank_in_account |
|---|---|---|---|---|---|---|
| ACC003 | NVDA | 150 | 0 | 150 | MISSING | 1 |
| ACC004 | GOOG | 0 | 80 | -80 | MISSING | 1 |
| ACC001 | MSFT | 300 | 250 | 50 | MEDIUM | 1 |

解读这个结果，就是你交给运营团队的东西：

- NVDA 差 150、整条在托管行缺失，最严重，排第一，先查。
- GOOG 托管行多出 80、内部缺失，第二。
- MSFT 两边都有但差 50，超过容差 10 所以上报，等级 MEDIUM。
- 注意：如果某个 break 的 diff 只有 5，会被步骤 4 的容差 `> 10` 挡掉，不出现在结果里，这就是容差的作用，避免运营被无关紧要的小差异淹没。

这条查询就是 POC-01 的核心产出。它把你这一篇学的所有东西串成了一条真实可交付的对账流水线：**T+1 清洗 → FULL OUTER JOIN 对齐 → COALESCE 补空算差 → 容差过滤 → CASE 分级 → 窗口排名 → 排序输出。** 把它敲进 psql 跑通、能对着结果讲清每一步为什么这么写，这一篇的目标就达成了。

> ✅ 终极小测验：如果把步骤 3 的 `FULL OUTER JOIN` 换成 `INNER JOIN`，最终结果表会少哪几行？
>
> 答案：会少 NVDA 和 GOOG 两行（它们只在单边存在，INNER JOIN 匹配不上直接丢）。只剩 MSFT 一行。也就是说，你会漏报两个 MISSING 型的严重 break，恰恰是最该先查的那两个。这就是为什么对账的 JOIN 必须是 FULL OUTER。把这个「反例」记牢，面试被追问「为什么用 FULL OUTER JOIN」时，你就用这个例子回答。

---

## 11. 怎么刷 HackerRank SQL 题 / 面试常被追问的点

你的目标很明确：OA 里 SQL 不过直接出局。所以这一节给你落地的刷题和应答策略。

### 11.1 刷题路线

在 HackerRank 上进 **SQL** 板块，按这个顺序刷（由易到难，覆盖 OA 高频）：

1. **Basic Select**：全刷。练 SELECT / WHERE / ORDER BY / LIMIT，手感为主，很快。
2. **Aggregation**：全刷。练 COUNT / SUM / AVG / GROUP BY / HAVING（本篇第 6 节）。
3. **Basic Join**：全刷。INNER / LEFT JOIN（第 4 节），OA 的绝对重点。
4. **Advanced Select**：练 CASE WHEN（第 10 节用到的）、字符串 / 日期处理。
5. **Advanced Join** 和 **窗口函数类题**：练 ROW_NUMBER / RANK / 自连接（第 7 节）。

每题的自我要求：**限时（简单题 5 分钟、中等 10-15 分钟）、不查文档、一次写对**。写完在你本地的 Docker Postgres 里用几行造的数据验证一下逻辑，比只在网页上提交学得扎实。

### 11.2 面试 / OA 里 SQL 最常被追问的点，逐条给你标准答法

- **「INNER JOIN 和 LEFT JOIN 有什么区别？什么时候用哪个？」** ，INNER 只留两边都匹配的行；LEFT 保左表全部、右表缺的填 NULL。要「找出左表有但右表没有的记录」（比如对账缺口）就用 LEFT JOIN + 右表关键列 IS NULL。
- **「WHERE 和 HAVING 的区别？」** ，WHERE 在分组前过滤单行，看不到聚合结果；HAVING 在分组后过滤组，能用聚合结果。用执行顺序 FROM→WHERE→GROUP BY→HAVING→SELECT 来解释。
- **「怎么找出两张本应一致的表之间的所有差异？」** ，FULL OUTER JOIN + COALESCE 对齐 key + `IS DISTINCT FROM` 或补 0 后比差。强调为什么不能用 INNER（会漏掉单边缺失），这正是本篇第 4.4 节，是你相对别的候选人的加分点，因为你能从对账业务讲透。
- **「LEFT JOIN 之后某列是 NULL 代表什么？」** ，代表右表没有能匹配上的行，即该记录只在左表存在。在对账里就是「某一侧漏记 / 另一侧多记」。
- **「取每个分组里排名第一的记录怎么写？」** ，窗口函数 `ROW_NUMBER() OVER (PARTITION BY 分组列 ORDER BY 排序列 DESC)` 算出排名列，外层套一层 `WHERE 排名 = 1`。强调窗口函数不能直接放 WHERE，要靠子查询 / CTE 包一层。
- **「为什么判断空值不能用 `= NULL`？」** ，因为任何值和 NULL 用 `=` 比较结果都是 NULL（不是真），永远选不出行。必须用 `IS NULL` / `IS NOT NULL`。

### 11.3 把你的 C++ 背景变成 SQL 面试的武器

你面 Trading Operations Analyst，简历上那套银行交易处理系统、事件驱动撮合系统是巨大优势，别只当它是「C++ 项目」。当面试官问 SQL / 对账时，你可以这样接：

- 「我在 C++ 里写过撮合系统的双边匹配和 partial fill，本质上和 post-trade 对账里按 key 匹配两侧记录、处理部分成交造成的数量差是同一类问题，只是这里用 SQL 的 FULL OUTER JOIN 来表达匹配。」
- 「我做过结算和审计查询，理解 T+1 结算周期，所以对账时我会先按 settle_date 过滤已结算部分，避免时间差造成的假 break。」

这套「用业务和系统直觉解释 SQL 选择」的能力，是纯刷题选手给不出的。你缺的只是 SQL 的手熟，那靠第 11.1 节的刷题补上，而底层的对账 / 撮合 / 结算直觉，你已经有了。

---

### 收尾清单（学完自检）

- [ ] 能用 Docker 起本地 Postgres、用 psql 连上、建表插数据。
- [ ] 能说清 INNER / LEFT / RIGHT / FULL OUTER 四种 JOIN 各留哪些行、各自何时用。
- [ ] 能用一句 SQL（LEFT JOIN + IS NULL）找出「内部有、托管行没有」的 break。
- [ ] 能解释 LEFT JOIN 后 NULL 的业务含义，知道判空用 IS NULL 不用 = NULL。
- [ ] 能写 GROUP BY + 聚合 + HAVING，并说清 WHERE 与 HAVING 的区别。
- [ ] 能用 ROW_NUMBER() OVER (PARTITION BY ... ORDER BY ...) 做组内排名，并知道要在子查询 / CTE 里算、外层过滤。
- [ ] 能把 T+1 规则写成 `WHERE settle_date <= as_of_date`，并知道为什么在 JOIN 前过滤。
- [ ] 能用 CTE 把复杂对账查询拆成清晰步骤。
- [ ] 能独立写出第 10 节那条完整对账查询并逐步讲解。

把这份清单每一项都能对着 psql 演示一遍，POC-01 就稳了。下一篇我们把这些 SQL 接进 Python，用 psycopg 从代码里跑对账、拿结果，凑齐 OA 要考的「Python + SQL」组合拳。
