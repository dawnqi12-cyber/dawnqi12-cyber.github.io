---
title: AI 工作流学习笔记：Workflow、Graph 与 Loop 怎么串起来
date: 2026-07-08 22:30:00
updated: 2026-07-08 22:30:00
tags:
- Agent
- AI工作流
- Workflow
- Graph
- Loop
- AI应用开发
categories:
- 技术学习
---

> 参考原文：[AI 工作流中的 Workflow、Graph 与 Loop：从概念到实现](https://javaguide.cn/ai/agent/workflow-graph-loop.html)
>
> 原文中有几张配图，但页面只看到 `Copyright © 2026 Guide`，没有明确图片转载授权说明。因此这篇博客只发布文字整理版，不复制原文图片。如果后续确认图片授权允许转载，再单独补图会更稳妥。

## 一、先抓主线：为什么 AI 系统需要工作流

传统软件里，流程通常是确定的：

- 输入确定；
- 步骤确定；
- 分支条件明确；
- 结果相对稳定。

但 AI 应用不是这样。LLM 的输出天然有不确定性：

- 可能格式不对；
- 可能答非所问；
- 可能出现幻觉；
- 可能工具调用失败；
- 可能一次生成达不到质量要求；
- 可能因为上下文太长导致关键信息丢失。

所以真实 AI 应用不是“问一句、答一句”就能稳定交付，而是需要一个能处理不确定性的执行系统。

这个系统要具备三个能力：

1. **动态决策**：根据当前生成结果决定下一步去哪。
2. **自动修正**：结果不达标时能回退、重试、修改。
3. **可控收敛**：循环不能无限跑，必须有终止条件和安全边界。

一句话概括：

> AI 工作流的核心价值，是把一次性的生成过程，变成一个可迭代、可观察、可控制、可收敛的流程。

## 二、传统 Workflow 和 AI Workflow 的区别

### 1. 传统 Workflow

传统 Workflow 关注的是：

> 这件事按什么步骤做完。

它的特点是：

- 任务步骤提前设计；
- 分支条件大多是确定规则；
- 同样输入通常走同样路径；
- 适合审批、订单、ETL、定时任务等确定性场景。

例如：

```text
金额 > 10000 -> 高级审批
金额 <= 10000 -> 普通审批
```

这个规则是明确的，不需要模型在运行时理解“质量是否达标”。

### 2. AI Workflow

AI Workflow 面对的是不确定输出，所以它更强调：

- 运行时评估；
- 状态驱动；
- 条件回退；
- 失败重试；
- 质量检查；
- 人工介入；
- 成本和 Token 控制。

例如：

```text
生成初稿 -> 审核质量 -> 不达标就修改 -> 再审核 -> 达标后输出
```

这里的关键不只是“步骤”，而是：

- 审核结论可能每次不同；
- 修改方向依赖当前草稿；
- 是否继续循环取决于评分、轮次和成本；
- 中间状态必须被记录。

所以 AI Workflow 相比传统 Workflow，更像是：

> 状态驱动的动态执行图。

## 三、Workflow、Graph、Loop 三者是什么关系

原文里最重要的一句话是：

> Workflow 是目标与过程，Graph 是结构与载体，Loop 是图上的控制模式。

这句话可以拆开理解。

### 1. Workflow：目标和过程

Workflow 描述的是：

> 为了完成一个目标，任务应该经历哪些阶段。

例如：

```text
生成文章 -> 审核文章 -> 修改文章 -> 输出最终稿
```

这描述的是任务过程。

### 2. Graph：结构和载体

Graph 把 Workflow 变成可执行结构。

它包括三个核心元素：

- Node：节点，负责执行具体动作；
- Edge：边，负责控制流转路径；
- State：状态，负责保存和传递上下文。

如果没有 Graph，Workflow 只是流程描述；  
有了 Graph，流程才有明确的结构。

### 3. Loop：图上的回溯控制

Loop 是 Graph 里一种特殊控制模式。

它表示：

> 某个节点执行完后，根据状态判断是否回到前面的节点继续执行。

比如：

```text
审核不通过 -> 回到修改节点 -> 再审核
```

这就是一个典型 Loop。

### 4. 三者合起来怎么理解

用一句话串起来：

> Workflow 说明要做什么和大致怎么做，Graph 把它组织成节点、边和状态，Loop 让流程在结果不达标时可以回退修正。

## 四、Graph 的核心元素：Node、Edge、State

Graph 是 AI 工作流的核心结构。  
真正做工程实现时，重点就是设计好 Node、Edge、State。

## 五、Node：节点是执行单元

Node 负责执行某个具体职责。

在“生成文章并审核”的例子中，可以有：

- `DraftNode`：生成初稿；
- `ReviewNode`：评估质量；
- `ReviseNode`：根据反馈修改；
- `ExitNode`：输出最终结果。

### Node 设计关键

Node 不应该按“这次调用哪个 API”来抽象，而应该按职责抽象。

好的 Node 关注：

- 输入是什么；
- 产出是什么；
- 修改 State 的哪些字段；
- 失败时怎么处理；
- 是否需要调用模型或工具。

### 常见错误

把节点拆得过细：

```text
检查标题长度 -> 检查语气 -> 检查字数 -> 检查格式 -> 检查关键词
```

这样短期看起来清楚，但长期会导致：

- 图变得很碎；
- 修改成本高；
- 复用性差；
- 新增规则时到处改。

更稳定的抽象应该是：

```text
生成 -> 评估 -> 修改 -> 输出
```

具体评估细节放在 `ReviewNode` 内部处理。

## 六、Edge：边是控制流规则

Edge 决定执行路径怎么走。

常见边类型包括：

### 1. 顺序边

固定从 A 到 B。

```text
DraftNode -> ReviewNode
```

### 2. 条件边

根据运行时状态选择下一步。

```text
score >= 80 -> ExitNode
score < 80 -> ReviseNode
```

### 3. 循环边

回到前面某个节点继续执行。

```text
ReviseNode -> ReviewNode
```

### 4. 并行边

一个节点分发到多个节点并行处理。

例如多文档摘要、多工具并发检索。

### 5. 动态路由

候选分支不是固定写死的，而是在运行时决定。

这类能力适合：

- map-reduce；
- 动态拆任务；
- 多工具组合；
- 多子任务并行。

### Edge 设计关键

Edge 不只是连线，它是路由规则。  
一个清晰的 Edge 应该回答：

- 什么状态下走这条边？
- 什么状态下结束？
- 什么状态下回退？
- 什么状态下转人工？

## 七、State：状态是工作流的运行记忆

State 是整个 Graph 中最容易被低估的部分。

如果说 Node 和 Edge 让流程“跑起来”，那么 State 让流程“知道自己跑到哪了”。

State 负责保存：

- 用户输入；
- 当前草稿；
- 审核评分；
- 审核反馈；
- 工具调用结果；
- 当前轮次；
- 错误信息；
- 最终输出。

### 常见 State 字段

```text
input: 用户原始输入
messages: 对话历史
current_draft: 当前草稿
review_score: 审核评分
review_feedback: 审核反馈
iteration_count: 当前迭代次数
tool_result: 工具调用结果
error_info: 错误信息
output: 最终输出
```

### State 更新策略

State 不只是“存值”，还要设计“怎么更新”。

常见三种策略：

### 1. 覆盖（Replace）

新值替换旧值。

适合：

- 当前草稿；
- 当前评分；
- 当前状态；
- 最终输出。

### 2. 追加（Append）

新值追加到列表。

适合：

- 对话历史；
- 中间步骤；
- 工具调用记录；
- 日志。

### 3. 自定义合并（Reducer）

用自定义逻辑合并新旧值。

适合：

- 并行节点同时写入；
- 消息去重；
- 根据 ID 更新；
- 多路检索结果融合。

### State 设计最容易踩的坑

#### 太粗

所有内容塞进一个大对象，最后很难知道谁改了什么。

#### 太细

字段拆得太碎，每个节点都要拼装很多字段，调试成本很高。

#### 并行写冲突

多个节点同时写同一个覆盖字段，会出现竞态。  
这种字段必须提前设计合并策略。

## 八、Loop：AI 工作流里的可控回溯

Loop 不是简单“多跑几次”。

更准确地说：

> Loop 是 Graph 中通过回边形成的迭代控制结构。

在 AI 工作流里，Loop 的典型用途是：

- 生成结果不达标，回到修改；
- 工具调用失败，重试；
- 格式校验失败，重新生成；
- 人工反馈后，继续修正。

## 九、Graph Loop 和 Agent Loop 的区别

这两个概念很容易混。

### Agent Loop

Agent Loop 是 Agent 的顶层执行循环：

```text
思考 -> 行动 -> 观察 -> 再思考
```

它是整个 Agent 的运行模式。

### Graph Loop

Graph Loop 是工作流图内部的局部回溯：

```text
审核 -> 修改 -> 再审核
```

它是某个子流程里的控制结构。

### 一句话区分

> Agent Loop 是外层运行引擎，Graph Loop 是工作流内部的局部迭代机制。

两者可以嵌套：  
Agent Loop 里可以执行一个带 Graph Loop 的工作流。

## 十、Loop 的两种类型

### 1. 固定次数循环

类似 `for`。

例如：

```text
最多重试 3 次
```

优点是安全、可控。  
缺点是不够灵活。

### 2. 条件驱动循环

类似 `while`。

例如：

```text
只要评分低于 80 分，就继续修改
```

优点是更贴近 AI 任务。  
缺点是如果没有边界，可能一直跑。

### 实际工程建议

两者通常要结合：

```text
只要评分低于 80 分就继续修改，但最多迭代 3 轮。
```

这才是可控 Loop。

## 十一、可靠 Loop 的三个条件

一个可靠的 Loop 必须有三类约束。

### 1. 继续条件

为什么还要再来一轮？

例如：

- 评分低于阈值；
- 格式校验失败；
- 工具调用失败；
- 用户要求继续优化。

### 2. 退出条件

什么时候可以结束？

例如：

- 评分达标；
- 输出格式正确；
- 用户确认满意；
- 达到可接受质量。

### 3. 安全边界

什么时候必须停？

例如：

- 最大轮次；
- 最大 Token；
- 最大耗时；
- 最大费用；
- 连续失败次数；
- 熔断条件。

### 最重要的一句话

> 没有安全边界的 Loop，不是自我修正，而是无限打转。

## 十二、嵌套循环怎么理解

实际项目里经常有嵌套循环：

### 外层循环

负责整体质量迭代：

```text
生成 -> 审核 -> 修改 -> 再审核
```

### 内层循环

负责某个节点内部的工具重试：

```text
调用 API -> 失败 -> 指数退避重试
```

这两层循环要分开设计：

- 计数器独立；
- 超时独立；
- 失败处理独立；
- 安全边界独立。

否则会出现：

- 内层工具重试耗尽外层预算；
- 外层已经结束，内层还在重试；
- 错误被吞掉，流程看起来还在正常跑。

## 十三、框架映射：Spring AI Alibaba 和 LangGraph

原文用 Spring AI Alibaba Graph 和 LangGraph 做了对照。  
对学习来说，最重要的是把概念映射清楚。

### 1. State

Spring AI Alibaba：

- `OverAllState`
- `KeyStrategyFactory`
- `ReplaceStrategy`
- `AppendStrategy`

LangGraph：

- `TypedDict`
- `Annotated[type, reducer]`
- 默认覆盖
- reducer 控制追加或合并

### 2. Node

Spring AI Alibaba：

- `NodeAction`

LangGraph：

- 普通函数

### 3. Edge

Spring AI Alibaba：

- `addEdge`
- `addConditionalEdges`

LangGraph：

- `add_edge`
- `add_conditional_edges`

### 4. Loop

两边本质上都是：

> 条件边回指前序节点。

Spring AI Alibaba 还提供更显式的 `LoopAgent` / `LoopMode`。  
LangGraph 通常需要自己在 State 中维护计数器和退出逻辑。

### 5. 持久化

两类框架都会支持 checkpoint / saver 思路。

这很重要，因为带 Loop 的流程可能跑很久，不能中断后从头来。

## 十四、工作流抽象能力：不要把图画碎

原文对抽象能力的提醒很关键。

很多初学者设计工作流时，会把每个细节都变成一个节点。  
结果就是图越来越碎，维护越来越难。

### 更好的抽象方式

Node 抽象职责，不抽象 API 调用。

Edge 抽象流转规则，不在图外写满 `if-else`。

State 抽象必须持久记住的信息，不把所有临时变量都塞进去。

### 一个稳定结构

对于“生成并审核”这类场景，稳定结构通常是：

```text
DraftNode -> ReviewNode -> ReviseNode -> ReviewNode -> ExitNode
```

而不是：

```text
检查标题 -> 检查字数 -> 检查格式 -> 检查语气 -> 检查引用 -> 修改标题 -> 修改正文 ...
```

## 十五、工作流落地常见坑

### 1. State 粒度不合理

太粗：不好查、不好追踪。  
太细：节点之间传值复杂，容易错。

建议按业务块拆：

- 用户输入；
- 当前结果；
- 审核结论；
- 路由控制；
- 错误信息；
- 最终输出。

### 2. Loop 没有终止条件

不要只写：

```text
如果不满意就继续优化
```

要明确：

- 最多几轮？
- 评分阈值是多少？
- 超时怎么办？
- 费用超限怎么办？
- 连续失败怎么办？

### 3. 错误处理只有 try-catch

AI 工作流应该把错误也变成图上的路径。

常见处理方式：

- 重试；
- 降级；
- 转人工；
- 输出当前最优结果并说明限制；
- 熔断终止。

### 4. Token 与成本失控

Loop 会放大成本。

每多一轮，都会增加：

- 输入 Token；
- 输出 Token；
- 工具调用次数；
- 等待时间；
- 失败风险。

所以要思考：

- 哪些节点必须调用大模型？
- 哪些节点可以用规则或代码替代？
- 是否可以先粗筛再精修？
- 是否达到“足够好”就退出？

### 5. 节点间数据结构不统一

如果不同节点各自定义字段名和 schema，图复杂后会非常难调试。

建议尽早统一：

- 字段命名；
- JSON Schema；
- Pydantic 模型；
- 错误格式；
- 状态更新规则。

## 十六、安全风险：AI 工作流会放大问题

工作流给 LLM 输出加了结构，但也引入了新风险。

### 1. State 污染

如果恶意输入被写进 State，后续节点可能继续读取并传播。

### 2. Prompt Injection 级联

一个节点被注入，后续节点可能都受到影响。

### 3. Loop 放大攻击

攻击者可能诱导系统反复重试，消耗 Token、时间和费用。

### 4. 工具调用风险

模型输出一旦影响工具参数，就要做权限控制和参数校验。

### 防护方向

- 用户输入和系统指令隔离；
- 对 LLM 输出做安全检测；
- 工具调用加权限边界；
- Loop 加最大轮次和预算；
- State 写入做校验；
- 敏感节点加人工确认。

## 十七、把整篇文章串起来

如果按学习路线理解，我建议这样走：

### 第一步：先理解为什么需要 Workflow

因为 AI 输出不稳定，真实任务需要检索、生成、审核、修正、重试和交付。

### 第二步：再理解 Graph

Graph 把 Workflow 变成节点、边、状态组成的可执行结构。

### 第三步：再理解 Loop

Loop 让 Graph 可以在结果不达标时回退修正，而不是一次失败就结束。

### 第四步：再理解 State

State 是整个系统的工作记忆，决定流程能否动态决策、恢复和调试。

### 第五步：再看框架实现

Spring AI Alibaba 和 LangGraph 名字不同，但核心都离不开：

- Node；
- Edge；
- State；
- Conditional Edge；
- Reducer；
- Checkpoint。

### 第六步：最后看工程坑

真正落地时，难点不是图怎么画，而是：

- State 粒度；
- Loop 边界；
- 错误恢复；
- Token 成本；
- 数据结构；
- 安全控制。

## 十八、最值得记住的关键词

### 核心概念

- Workflow
- Graph
- Loop
- Node
- Edge
- State

### 控制流

- Sequential Edge
- Conditional Edge
- Dynamic Routing
- Back Edge
- Termination Edge
- Parallel Edge

### State 更新

- Replace
- Append
- Reducer
- Concurrent Update
- Checkpoint

### Loop 设计

- Continue Condition
- Exit Condition
- Safety Boundary
- Max Iteration
- Timeout
- Token Budget
- Fallback

### 工程落地

- Error Handling
- Retry
- Circuit Breaker
- Human-in-the-Loop
- State Persistence
- Schema Design
- Token Cost Control

### 框架关键词

- Spring AI Alibaba Graph
- LangGraph
- NodeAction
- StateGraph
- MemorySaver
- SqliteSaver
- LoopAgent

## 总结

如果把整篇文章压缩成一句话，我会这样说：

> AI 工作流不是简单把多个 Prompt 串起来，而是用 Graph 组织节点、边和状态，再用 Loop 处理不确定输出带来的修正、重试和收敛问题。

真正要掌握的不是某个框架 API，而是这套抽象：

```text
Workflow 定目标和过程
Graph 定结构和状态
Loop 定回退和收敛
State 定运行时记忆
Edge 定路由规则
Node 定职责边界
```

只要这条线清楚，Spring AI Alibaba、LangGraph、CrewAI 或未来新的工作流框架，底层理解都不会变。
