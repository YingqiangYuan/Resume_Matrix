# POC-03：用 Python 重写 C++ 分批成交模块 + 限时刷题来练 OA 手感

> 严重度：🟡 Important ｜ 估时：8 天（并行轨）｜ 对应 tutorial：[../../tutorials/03-python-oa-readiness-cn.md](../../tutorials/03-python-oa-readiness-cn.md)

## 一句话 pitch

用把已有的 C++ 撮合模块用 Python 重写 + 刷中等 HackerRank 题，来把 Python 从"会用"练到"限时 OA 能流畅敲"。

## 支撑的 case 决策

case 决策 5：分批成交先聚合到订单级再匹配，按 as-of 口径。用 Python 重写这块，同时喂 case 的交易对账。

## 输入与 setup

- Wesley 已有的 C++"动态事件驱动撮合系统"里的分批成交（partial fill）匹配模块。
- 一批 HackerRank 中等题（哈希去重、双指针、简单 DP、字符串解析）。

## 期望产出物

1. Python 版分批成交聚合模块（喂给 case 交易对账）。
2. 刷题解答仓库（按题型分类）。
3. 一份"C++ 到 Python 我踩了哪些语法/惯用法坑"的短记。

## 成功标准（可自验）

- [ ] 能在 HackerRank 中等题上用 Python 限时做对、不卡语法。
- [ ] 能把 C++ 分批成交模块用 Python 重写并跑通。
- [ ] 能讲清 `defaultdict` / `Counter` / 推导式等惯用法什么时候用。
- [ ] 能解释 as-of 聚合口径（截至某日已成交量，不假设订单填满）。

## 教程

见 [../../tutorials/03-python-oa-readiness-cn.md](../../tutorials/03-python-oa-readiness-cn.md)。

## 踩坑 / 衍生

坑是"C++ 思维写 Python"（手动 for 循环而不用推导式/内置函数），限时场景吃亏。这是并行轨，全程每周固定时段推进，不占独立主周。衍生：把 Python 版分批成交和 POC-01 的 SQL 版做结果对照，验证两条路聚合量一致，是很好的面试谈资。
