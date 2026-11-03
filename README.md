# exam-cram · 考前突击复习教练

> 一个 Agent Skill。你给课程和考期，它找卷子、排考点、逐题讲透。

适用于 WorkBuddy / CodeBuddy，以及其他遵循通用 Agent Skills 规范（一个 `SKILL.md` + YAML frontmatter）的客户端。

---

## 它解决的具体问题

不是「帮你总结知识点」。是一个很窄但很真实的场景：

**没去上课，离考试只剩几天，要从零开始。**

这种时候最难受的不是没资料，是**资料太多不知道学哪个**，以及**看了解析还是不会**。

exam-cram 针对这两件事分别下手。

## 和普通 AI 解析的区别

普通解析从「解：」开始，省掉了三次「这里为什么想到这个」。

六段式把这三次补回来：

| 段 | 做什么 |
|---|---|
| ① 一句话翻译 | 题干翻成人话，先知道要干什么 |
| ② 定点 | 考哪个知识点，在整门课的什么位置 |
| ③ 前置补课 30 秒 | 只补这题用得着的那一点，不展开成章节 |
| ④ 手把手推 | 每步交代为什么这么想，**并标出卡点** |
| ⑤ 老师还能怎么问 | 2–3 个变式方向，防「换个说法就不会」 |
| ⑥ 保底写法 | 完全不会时写什么能拿步骤分 |

其中 **④ 的卡点** 是分水岭：

> 普通解析告诉你怎样做对，它告诉你**别人在哪里错、为什么错**。

比如「把开区间可导写成闭区间可导」「只背了概念不会用」——这类才是一个没上过课的人真正会掉进去的坑。

## 三个反直觉的设定

**一、把你的时间打六折。**
你说「还剩 3 天每天能学 10 小时」，它算出来是 9.6 小时。学生普遍高估 2–3 倍，不折一下，计划从第一天就是假的。

**二、明确告诉你放弃什么。**
考点表里有一档叫 ⚪ 放弃，会把「哪几章不用看、省下多少小时」写出来。对突击的人来说，知道不学什么比知道学什么更值钱。

**三、会劝你去睡觉。**
边际收益低于 1 分/小时、或者距考试不足 8 小时，它会直接说「剩下的放弃，去睡」。通宵多学 4 小时换考场反应下降 30%，净亏。

## 安装

**方式一：git clone（推荐）**

```bash
git clone https://github.com/shizhanhshuo/exam-cram.git ~/.workbuddy/skills/exam-cram
```

Windows：

```powershell
git clone https://github.com/shizhanhshuo/exam-cram.git "$env:USERPROFILE\.workbuddy\skills\exam-cram"
```

**方式二：上传技能包**

下载 [Release](https://github.com/shizhanhshuo/exam-cram/releases) 里的 `exam-cram.zip`，在技能页 → 添加技能 → 上传技能，导入即可。

**方式三：手动**

把整个目录拷到 `~/.workbuddy/skills/exam-cram/`（项目级则放 `.workbuddy/skills/exam-cram/`），确保 `SKILL.md` 在这一层。

装完**新开一个对话**生效。

## 用法

在对话里直接说需求，不用记命令：

**有卷子**

> 用 exam-cram 帮我复习《管理会计》，11 月 12 号上午考，还剩 5 天，每天能学 6 小时，目标及格。卷子在附件里。

**一份卷子都没有**

> exam-cram，复习《数据结构》，XX 大学，教材是严蔚敏版，后天考，实际只有 8 小时，能过就行。卷子我搞不到。

**只想讲一道题**

> exam-cram，这道题给我讲透（贴题目或拍照）

没自动认出来时，开头写上 `exam-cram` 就一定会触发。

## 它产出什么

在你自己指定的目录下建 `复习-<课程>-<考试名>/`：

| 文件 | 作用 |
|---|---|
| `00-作战计划.md` | 模式、时间块分配、目标、要放弃什么 |
| `01-考点热力表.md` | 全部考点排序，划 60% / 80% / 95% 三条线 |
| `02-讲题卡片/<考点>.md` | 逐题六段式讲解 |
| `03-自测记录.md` | 每题对错、错因归类 |
| `04-保底清单.md` | 考前 20 分钟只看这一页 |

## 它是怎么排序的

```
Score = 0.35 × 频率 + 0.30 × 分值占比 + 0.20 × 新近度 + 0.15 × 章节权重
```

样本不足时自动降级：只有 1 份卷子就弃用频率，完全没有本校卷就改成「章节权重 + 通用先验」，并且**在表里标注这是推断，不是统计**。

按累计分值划三条线：

- 60% —— 及格底线
- 80% —— 稳过线
- 95% —— 冲高线

线的意义是给一个心理锚点：**复习到这里就能及格了**。否则人会以为要全学完，然后直接摆烂。

## 取题：两条路

**有卷子** → 解析。OCR 校对、切题、六字段抽取、同题归并。

**没卷子** → 联网检索，按性价比排来源：教材课后题 > 老师提纲 > 同教材异校卷 > 考研真题 > 院系官网 PDF > 学科垂直站 > 文库。

每一份材料都标可信度（✅ 本校 / 🟡 同课异校 / ⚠️ 同教材 / 🔵 考研 / ⛔ 假真题），**混进统计会让整张表失真**。

## 诚实边界

这个技能不承诺这几件事，也不假装能：

- **不保证押中。** 考点排序是启发式推断，不是预测。它做的是把有限时间分到期望收益更高的地方，仅此而已。
- **「你们学校」的真题经常搜不到。** 大学专业课卷子基本不外流，公网命中率约 20–40%。搜不到时会明说，改用教材课后题和同教材异校卷替代，**不会编一份卷子出来**。
- **网上大量「2026 年 XX 期末试卷」是假的。** 没有校名、题题带解析、机械排列的那种，多半是题库拼的。技能里加了识别规则，见到就排除。
- **它不能替代你真的动手做题。** 看懂了和会做之间隔着一次动手。自测环节就是防这个的，但你可以骗过它。

## 目录结构

```
exam-cram/
├── SKILL.md                  # 主流程 + 五条红线
├── references/
│   ├── modes.md              # 时间分层、模式切换、睡眠规则
│   ├── sourcing.md           # 双路径取题、可信度分级、假真题识别
│   ├── analysis.md           # 考点标签、加权排序、分数线、错因分类
│   └── teaching.md           # 六段式讲题法（核心）
└── assets/                   # 五份输出模板
```

`references/teaching.md` 是最值得看的一份，里面有完整的好坏对照示例。

## 许可

[MIT](LICENSE)。随便用、改、传，保留署名即可。

## 贡献

欢迎提 Issue 和 PR。最想要的两类反馈：

1. **某门课跑出来的考点排序明显不对** —— 说明权重公式要调，请附上课程名和材料样本量
2. **某道题的讲法没讲透** —— 直接说哪一段是废话，我改教学模板

---

**English TL;DR** — exam-cram is an Agent Skill for last-minute exam prep. Give it a course and a deadline; it locates past papers (or falls back to textbook problems), ranks topics by `0.35×frequency + 0.30×marks + 0.20×recency + 0.15×chapter weight`, draws 60/80/95% score lines, then teaches each question in six passes — translation, placement, 30-second prerequisite, walkthrough with **common-pitfall callouts**, variant predictions, and a minimum-credit fallback. It deliberately cuts your self-reported study time by 40%, tells you what to skip, and will tell you to go to sleep. MIT licensed.
