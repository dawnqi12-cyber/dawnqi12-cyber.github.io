---
title: Agent Skills 学习笔记：Skill、Prompt、MCP、Function Calling 到底怎么分工
date: 2026-07-07 22:10:00
updated: 2026-07-07 22:10:00
tags:
- Agent
- Skills
- MCP
- Prompt
- AI应用开发
categories:
- 技术学习
---

> 参考原文：[Agent Skills 是什么？和 Prompt、MCP 到底差在哪？](https://javaguide.cn/ai/agent/skills.html)
>
> 这篇文章不是原文搬运，而是把我认为最值得记住的主线重新整理成一份学习笔记。重点不在于复述，而在于把 Skill 放回 Agent 系统里理解：它到底是什么、解决什么问题、和 Prompt / MCP / Function Calling 怎么分工，以及 `SKILL.md` 到底应该怎么写。

## 一、先抓核心定义：Agent Skill 到底是什么

如果只记一句话，我会这样定义：

> Skill 是一份可被 Agent 发现、按需加载的任务说明，用来沉淀某类任务的经验、约束、流程和边界。

它不是模型“突然学会的新能力”，而是把原来散落在团队规范、脚本、经验、口头约定里的东西，整理成一个可复用的说明包，让 Agent 在命中某类任务时再读。

简单理解：

- Prompt 更像用户这次的临时请求。
- Skill 更像团队里已经沉淀好的“做事方法”。

所以 Skill 的真正价值，不是“让模型更聪明”，而是：

- 让同类任务有一致做法；
- 让经验可以跨会话复用；
- 让模型少走弯路；
- 让复杂任务不必每次重新讲一遍。

## 二、Skill 解决的到底是什么问题

很多团队第一次接触 Skill，都是从这些痛点开始的：

- 代码审查规范每次都要重新贴进 Prompt。
- 某个任务只有老同事知道隐含规则。
- 工具很多，但模型不知道什么场景下该怎么组合。
- 任务流程明明固定，却每次都要重新解释一遍。

### Skill 解决的不是“执行能力缺失”

模型可能已经有能力调用工具、写代码、分析文件，问题在于：

> 它不知道你们团队在这个任务里“默认怎么做、先做什么、后做什么、哪些不能做”。

这恰好就是 Skill 最适合承接的内容。

### 关键词

- Task Instruction
- Reusable Workflow
- Operational Convention
- Task Boundary
- Team Practice
- Execution Guidance
- Context Injection

## 三、Skill 和 Prompt、MCP、Function Calling 的关系

这是全文最重要的一部分。

### 1. 先给一句结论

> Skill 不是 Prompt、MCP、Function Calling 的替代品，它们根本不在同一层。

如果硬要放到一条执行链路里理解，可以这样看：

1. 用户提出任务：这是 Prompt。
2. 模型判断需要调用某个工具并生成参数：这是 Function Calling。
3. 这个工具如果来自外部服务和标准协议：这通常是 MCP。
4. 模型在执行这类任务时应该遵守什么流程、顺序、约束、检查项：这是 Skill。

### 2. 一句话区分四者

#### Prompt

用户这次到底想做什么。

#### Function Calling

模型如何把“想调用工具”表达成结构化动作。

#### MCP

工具和外部能力是如何被标准化接入的。

#### Skill

模型在做这类任务时，应该按什么经验和流程来执行。

### 3. 一个完整链路例子

假设用户说：

```text
帮我分析这份报表，先看字段含义，再找异常值，最后给业务结论。
```

这时：

- 这句话本身是 Prompt。
- 模型决定调用 `read_file`、`parse_csv`、`summarize`，这是 Function Calling。
- 如果这些工具来自 MCP Server，那接入和协议层是 MCP。
- “分析报表要先看字段，再看异常，再做结论，不要一上来堆统计指标”，这部分就是 Skill。

### 4. 真正要记住的结论

> Prompt 说“这次要干什么”，Skill 说“这类事平时该怎么干”。

## 四、Skill 不是工具，它更像“上下文注入机制”

很多人会误以为：

> Skill = 工具封装

其实这会把理解带偏。

Skill 不一定非要调工具。  
有些 Skill 只是给模型一个检查框架，例如：

- Code Review Skill
- 架构评审 Skill
- 设计检查 Skill

它们可能根本不需要外部工具，也一样有价值。

所以更准确的理解是：

> Skill 更像一份在需要时才注入上下文的“任务操作手册”。

### 关键词

- Context Injection
- Lazy Loading
- Task Routing
- On-Demand Loading
- Instruction Pack
- Reusable Expertise

## 五、一个 Skill 最小长什么样

最小可用 Skill 其实很简单：

```text
skill-name/
├── SKILL.md
├── scripts/
├── references/
└── assets/
```

其中真正必须的，通常只有：

- 一个目录
- 一个 `SKILL.md`

其他目录是为了复杂 Skill 做渐进扩展。

### 目录职责

#### `SKILL.md`

主文件。被命中时加载，是 Agent 真正要读的操作手册。

#### `scripts/`

放脚本。适合那些“应该稳定执行，不该靠模型自由发挥”的部分。

#### `references/`

放长说明、详细 checklist、规范文档。主文件不要塞太长，把低频细节拆过来按需加载。

#### `assets/`

放模板、静态文件、样例输出等。

## 六、`SKILL.md` 的两层结构

一个好用的 `SKILL.md`，通常就两层：

1. Frontmatter 元数据
2. 正文说明

## 七、Frontmatter 怎么写：重点是 `name` 和 `description`

原文里对这部分讲得很清楚，我整理成最值得记的版本。

### 1. `name`

`name` 是 Skill 的标识。

#### 关键词

- Skill Identifier
- Naming Convention
- Kebab Case
- Stable Label

### 2. `description`

`description` 比 `name` 更关键，因为：

> 模型不是先读所有 Skill 正文再决定用哪个，而是先看 `description` 来做路由判断。

所以 `description` 实际上承担的是：

- 路由提示
- 触发条件
- 使用场景说明

### 3. 一个好 `description` 应该包含什么

建议至少说清两件事：

1. 这个 Skill 做什么
2. 什么场景下该用它

如果还能补上常见触发词，会更好。

### 4. 好坏对比怎么判断

#### 好的写法

- 有能力边界
- 有使用场景
- 有触发词

#### 不好的写法

- 只说“我可以帮你做 X”
- 只写功能名
- 没说什么时候该用
- 太泛，任何任务都像能匹配

### 5. 一个简化原则

> `description` 决定“它会不会被调用”，正文决定“被调用后它会不会有用”。

## 八、正文怎么写：别写科普文，写操作手册

这是原文里特别值得记住的一点。

Skill 正文不是 README，也不是概念介绍，更不是背景科普。

正文应该只放：

- 当前任务真的需要的信息
- 模型猜不到的信息
- 高价值的约束、默认项、流程和踩坑点

### 1. 正文最值得放什么

#### 默认做法

例如：

- 默认用哪套库
- 默认先跑哪个命令
- 默认按什么顺序执行

#### 私有约束

例如：

- 表里有软删除字段，查询必须带 `deleted_at IS NULL`
- 某个目录不能改
- 某个命令必须带参数

#### 失败处理

例如：

- 测试失败先看哪里
- 输出为空先排查什么
- 工具异常时如何兜底

#### 检查清单

例如：

- 代码审查先看安全，再看性能，再看可维护性
- 提交前必须通过哪些校验

### 2. 正文最不该放什么

- 通用概念解释
- 工具百科
- 模型已经知道的常识
- 冗长背景介绍

### 3. 一句非常实用的话

> Skill 正文里最值钱的内容，通常不是概念定义，而是“模型自己猜不到的规则”和“真实踩坑经验”。

## 九、为什么 `SKILL.md` 不能太长

原文强调了一个非常现实的点：

> 上下文窗口是公共资源。

Skill 一旦被加载，它的内容就要和：

- system prompt
- 用户请求
- 对话历史
- 工具返回结果
- 其他上下文

一起竞争模型注意力。

### 过长的风险

- 重点规则被冲淡
- 真正有价值的约束被埋没
- Token 成本上升
- 任务执行反而变差

### 关键词

- Context Budget
- Token Competition
- Attention Dilution
- Progressive Disclosure
- Lazy Read

### 一个重要原则

> 主文件只放“当前任务必须知道的内容”，低频细节拆到 `references/` 或 `scripts/`。

## 十、渐进式披露：复杂 Skill 的关键设计

这是 Skill 设计里非常重要的工程思想。

### 什么叫渐进式披露

不是把所有说明一股脑塞进 `SKILL.md`，而是：

- 主文件只保留主流程
- 次级细节放到额外文件
- 真需要时再让 Agent 读进去

### 为什么重要

因为大多数复杂任务并不总是需要全部细节。

例如：

- Code Review Skill 不需要每次都把全部 SOLID checklist 塞进主文件
- 主文件只要写：需要做 SOLID 检查时，读取 `references/solid-checklist.md`

### 关键词

- Progressive Disclosure
- Reference Loading
- Context Saving
- Demand-Driven Reading

## 十一、自由度怎么把控：这是 Skill 设计里最容易忽略的问题

原文这部分非常实用。

写 Skill 时，不能只想“怎么写流程”，还要想：

> 这个任务到底应该让 Agent 自己发挥多少？

### 一个简单判断原则

- 出错代价高：自由度要低
- 需要综合判断：自由度可以高一点

### 三档理解

#### 1. 高自由度

适合：

- 技术方案评估
- 代码审查
- 设计分析
- 头脑风暴

特点：

- 给检查方向
- 不把步骤完全写死

#### 2. 中自由度

适合：

- 有模板但允许调整的任务
- 文档生成
- 规则化分析任务

特点：

- 给模板
- 给参数
- 给边界

#### 3. 低自由度

适合：

- 数据库迁移
- 生产部署
- 风险高的批量操作
- 必须严格按顺序的命令执行

特点：

- 命令写清楚
- 参数写清楚
- 顺序写清楚
- 明确不能改

### 一句话理解

> Skill 不是越严格越好，也不是越自由越好，而是要和任务风险匹配。

## 十二、Skill 的最佳使用场景

不是所有知识都值得做成 Skill。

### 特别适合做 Skill 的内容

- 有清晰触发场景
- 有稳定流程
- 有团队约束
- 有高频复用价值
- 需要跨会话保持一致做法

### 典型例子

- TDD
- Code Review
- 提交消息生成
- PDF 解析流程
- SQL 排查流程
- 某类项目特定规范

## 十三、什么内容不适合做 Skill

### 1. 纯常识

如果模型本来就知道，不值得塞进 Skill 正文。

### 2. 没有明确触发边界

如果你自己都说不清“什么时候该用它”，那它就不适合单独做成 Skill。

### 3. 完全依赖实时数据的任务

这类内容更多应该交给工具、RAG、MCP，而不是 Skill。

### 4. 超长百科式说明

适合文档库，不适合直接成为 `SKILL.md` 主体。

## 十四、把全文真正串起来：一条最清晰的学习路线

如果你是第一次系统学 Skill，我建议按下面这条路线理解：

### 第一步：先分清 Skill 解决的是“做法问题”

Prompt 解决“用户这次要什么”，  
Skill 解决“这类事平时该怎么做”。

### 第二步：再分清 Skill 和工具不是同一层

Skill 不等于工具，不等于 MCP，也不等于 Function Calling。  
Skill 更像任务经验包和上下文注入机制。

### 第三步：再理解 `SKILL.md` 的结构

一个 Skill 最少包括：

- `name`
- `description`
- 正文流程

### 第四步：再理解路由机制

模型先看元数据决定要不要加载 Skill，  
不是一开始就读完整正文。

### 第五步：再理解正文写作原则

正文不是科普，不是百科，而是：

- 默认做法
- 私有约束
- 流程顺序
- 失败处理
- 踩坑经验

### 第六步：再理解上下文预算

Skill 本质上是上下文资源的一部分。  
写得太长，反而会稀释真正重要的信息。

### 第七步：最后理解自由度控制

风险高的任务写死流程，  
需要判断的任务保留发挥空间。

## 十五、最值得背下来的关键词

如果你要做复习速记，我会记这些：

### 基础概念

- Skill
- Prompt
- Function Calling
- MCP
- Context Injection
- Task Routing
- Lazy Loading

### 文件结构

- `SKILL.md`
- `scripts/`
- `references/`
- `assets/`

### 元数据

- `name`
- `description`
- Trigger
- Route
- Capability
- Scenario

### 正文设计

- Workflow
- Constraint
- Default Action
- Failure Handling
- Checklist
- Pitfall

### 上下文工程

- Context Budget
- Token Competition
- Progressive Disclosure
- Demand-Driven Reading

### 自由度控制

- High Autonomy
- Medium Autonomy
- Low Autonomy
- Risk-Based Design

## 总结

如果把整篇文章压缩成一句话，我会这样说：

> Agent Skill 不是新能力，而是把“某类任务应该怎么做”的经验、规则、顺序和边界整理成一份可按需加载的说明，让 Agent 在正确场景下少走弯路、做法一致、执行更稳。

所以最好的理解路径不是死记定义，而是按下面这条主线去看：

**Skill 是什么 -> 它和 Prompt / MCP / Function Calling 怎么分工 -> `SKILL.md` 怎么写 -> 为什么不能写成长 README -> 如何通过渐进式披露节省上下文 -> 如何根据任务风险控制 Agent 自由度**

只要这条链通了，你就不会把 Skill 误解成“工具封装”或者“更长的 Prompt”，而会把它看成 Agent 系统里非常关键的一层任务经验基础设施。
