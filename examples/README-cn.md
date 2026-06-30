# examples 总览, 把改简历当成工程项目跑通的一整套主线

> 这是 examples 系列的入口. 13 节课其实就是一条主线: 先看清简历游戏的规则, 再把简历当工程项目去管理, 用 AI 把最难的几步包下来. 这篇 README 帮你建立全局心智模型, 再决定从哪一节切入. 全文 5 分钟内读完.

## 1. 这套课假设你已经有的两件事

第一件: 你已经知道自己要投哪一类岗位, 找得到具体的 Job Description (JD). 这部分由 prerequisite 课程 `learn_career_planning` 负责. 它会带你做职业方向探索, 选定 Job Family, 在招聘网站上检索到具体 JD.

第二件: 你已经会用 `understand-landscape` 把一份 JD 拆解开. 给定一份 JD, 你能讲清楚这个岗位背后是哪些技术栈, 哪些业务场景, 招聘的人在意什么. 你也已经会把自己简历上的几条 bullet 反向扩展成一份 case 文档, 也就是一段经历的长版本. 这两个能力都在 prerequisite 课程里训练好.

如果上面两件事还不具备, 先回去把 prerequisite 课程走完, 再回来看 examples.

---

## 2. examples 的主线: 一条不断转的循环

正式开始之前, 先把这条循环记在脑子里, 它贯穿全部 13 节:

> 看清规则 → 定方向 (JD) → 准备素材 (case) → 设计或拔高项目 → 压成 bullets → 反推 Summary → 派生投递版本 → 收到反馈 → 进入下一轮

这套循环里最麻烦的几步, 我们都让 AI 兜底. 学完 examples 后你会熟悉以下 10 个 skill:

| 卡点 | 对应的 AI skill |
| :--- | :--- |
| 诊断你跟目标岗位的 gap | `qualify-gap-analyze` |
| 设计一个能写进简历的项目 | `mini-project-design`, `mini-project-review` |
| 把技能 gap 填上 | `qualify-execution-plan`, `qualify-coach`, `qualify-mock-interview` |
| 把 5000 字的 case 压成 3 到 4 条 bullet | `bullet-writer`, `bullet-reviewer` |
| 从 bullets 反推每个方向的定位句 | `summary-writer`, `summary-reviewer` |

13 节课的顺序就是按这条循环铺开. 下面按阶段过一遍.

---

## 3. 阶段一: 看清简历游戏的本质 (01 到 04)

写之前先要知道写给谁看. 这一段不动手写, 但后面所有的具体写法都建立在这里.

- [01-hiring](./01-hiring/README-cn.md) 招聘流程的三条路 (大公司校招, 中小公司直投, 社招), Hiring Manager / Recruiter / ATS 各自看什么.
- [02-ats](./02-ats/README-cn.md) 拆穿 ATS 的五大误解, 立 Reader-Compliant 原则: 写给五秒钟扫一眼的真人, 不是写给想象中的算法.
- [03-new-grad](./03-new-grad/README-cn.md) 应届生没有正式工作经验时, 怎么用 Projects 板块加 Action + Technical Detail + Impact 撑起一份够格的简历.
- [04-experienced](./04-experienced/README-cn.md) 1 到 5 年经验的人, 怎么从"我做了什么"切到"我做成了什么", Work Experience 升上去, Education 落下来.

---

## 4. 阶段二: 把简历当工程项目, 一份母版加 N 份投递 (05)

[05-resume-matrix](./05-resume-matrix/README-cn.md) 是这套方法的骨架. AI 时代岗位之间的界限越来越模糊, 同一个候选人往往同时适合好几个 Job Family. 一个 CS 学生完全可以同时投 Software Engineer, Data Analyst, AI Engineer 三个方向, 有经验的人也可能同时投初创公司和大公司, 横跨医疗, 金融, 互联网. 这种"一人对多个方向"的投递逻辑跟短视频的账号矩阵是一回事: 同一个人, 同样的核心内容, 按不同侧重点产出多个版本铺到不同频道, 被看到的概率成倍增加.

这一节告诉你: 维护一份"巨长"的母版简历做素材库, 派生 N 份投递版本只做减法, 不做加法. 这一节没有实操, 是后面所有 AI 流程的脚手架.

---

## 5. 阶段三: 准备素材的三条路 (06 到 08)

bullets 是从长长的 case 文档里压出来的, 这一段告诉你 case 怎么来. 改简历的本质其实就是: 设计一个 3 到 6 个月, 学生自己跑得完, 又跟目标 JD 精准匹配的企业级项目, 同时配套一份学习资料和一组辅助 agent, 通过简单思想实验就能验证它能不能写进简历. 以前"设计这种项目"是有经验导师的专利, 因为 AI 可以设计得天花乱坠, 但学生没有判断力分辨好坏. 这套方法能让最终产出达到有经验导师 70% 到 80% 的质量, 对于改简历来说足够.

- [06-prepare-project-material](./06-prepare-project-material/README-cn.md) 三条路的总图: 找 mentor 给你设计项目, 拔高现有薄经历, 或者从 0 设计一个新项目.
- [07-elevate-existing-project](./07-elevate-existing-project/README-cn.md) 路径二的完整 6 阶段流水线. 输入是一段"听起来很薄"的实习或课程项目, 输出是面试就绪的 case 文档. 用 `qualify-gap-analyze` → `mini-project-design` → `qualify-execution-plan` → `qualify-coach` → `qualify-mock-interview` 一路打通.
- [08-design-new-project](./08-design-new-project/README-cn.md) 路径三: 完全没有相关经历时, 反着从 JD 推一个真能落地的项目. 用同一条 6 阶段流水线, 只是把 design skill 切到 from-scratch 模式.

读法上, 06 是地图, 人人都要读. 07 和 08 不是二选一: 如果你既有一段可以拔高的薄经历, 又想再补一个全新项目, 那 07 和 08 都做; 如果只能选一边, 看你手上有没有那段值得拔的旧经历, 有就走 07, 没有就走 08.

---

## 6. 阶段四: 压成 bullets, 再反推 Summary (09 到 10)

项目设计完, case 背景信息攒齐之后, 才轮到把它们压成简历正文. 一份优秀的简历不是把 bullets 堆起来强塞关键词, 而是一段完整有逻辑的职业叙事, 把你的能力矩阵和成长轨迹清晰展示出来. 既要能让机器筛简历的时候选中你, 又要让人类面试官扫一眼就爱上你. 写出这样的简历很难, 这一阶段就是搞定这件事.

顺序很关键: 先 bullets, 再 Summary.

- [09-write-bullets](./09-write-bullets/README-cn.md) 用 `bullet-writer` 把 5000 字 case 压成 3 到 4 条经得起追问的 bullet. 引入 B1 / B2 / B3 / B4 的递进结构, 同一段经历可以给不同 Job Family 各写一组 bullet.
- [10-write-summary](./10-write-summary/README-cn.md) 用 `summary-writer` 从已经写好的 bullets 反推 Summary. 每个方向写一份, 全部塞进母版的 Summary 区. Summary 不是自传, 是定位标签.

为什么 bullets 在前? 因为 bullets 是证据, Summary 只是给证据贴标签. 标签可以根据证据反推, 反过来很容易写空.

---

## 7. 阶段五: 真正投出去, 然后让弹药库越积越厚 (11 到 12)

- [11-submit-and-collaborate](./11-submit-and-collaborate/README-cn.md) 最后一公里. 从母版派生针对性投递版本, 用 GitHub 加 Google Doc 双轨跟 mentor 协作, 最后导出 PDF. 包括末行 70% 规则一类只能眼睛看出来的排版细节.
- [12-maintain-new-projects](./12-maintain-new-projects/README-cn.md) 从 0 到 1 完成后切到 1 到 N. 每来一段新经历走标准 8 步, 一两天就把新项目并进弹药库, 之后投每一家只剩 30 到 60 分钟. Summary 的人设也会随职业阶段从技能型逐步演化成领域专家型.

---

## 8. 阶段六: 终极闭环, 简历变副产品 (13)

[13-final-synthesis](./13-final-synthesis/README-cn.md) 没有新工具, 任务只有一件: 把前面 12 节扣成一个真的能自循环的飞轮. 设计 → 填技能 → 执行 → case → bullets → summary → 派生 → 投递 → 反馈, 反馈又驱动下一轮项目设计. 飞轮转起来之后, 简历就不再是焦虑事件, 而是工程师终身职业管理的副产品.

---

## 9. 怎么读这门课

第一次看, 按顺序从 01 读到 13 最稳, 总共大约 12 到 16 小时. 如果你已经有一些基础, 也可以按阶段切入: 想理解游戏规则, 读 01 到 04; 想直接动手写, 跳到 05 加 09 加 10; 想用 AI 设计一个新项目, 看 06 加 08; 想了解长期玩法, 看 12 加 13.

读完整个 examples, 你应该建立的能力是: 拿到任何一份 JD, 都知道下一步该用哪个 skill, 该产出什么文件, 该花多少时间. 简历不再是临时抱佛脚的产物, 而是你能一直运营下去的一个工程项目.
