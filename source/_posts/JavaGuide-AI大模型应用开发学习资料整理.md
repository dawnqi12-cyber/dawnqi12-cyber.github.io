---
title: JavaGuide AI / 大模型应用开发学习资料整理
date: 2026-06-28 20:06:52
tags:
- AI
- 大模型
- RAG
- Agent
- JavaGuide
categories:
- 技术学习
---

# JavaGuide AI / 大模型应用开发学习资料整理

整理日期：2026-06-28  
主要来源：

- [AI 应用开发面试指南](https://javaguide.cn/ai/interview-questions/ai-interview-guide.html)
- [AI 应用开发知识体系](https://javaguide.cn/ai/)
- [AI 应用开发面试题专题](https://javaguide.cn/ai/interview-questions/)
- [大模型基础专题](https://javaguide.cn/ai/llm-basis/)
- [AI Agent 专题](https://javaguide.cn/ai/agent/)
- [RAG 专题](https://javaguide.cn/ai/rag/)
- [AI 系统设计专题](https://javaguide.cn/ai/system-design/)

说明：本文不是原文逐字搬运，而是把 JavaGuide 页面中与大模型、Agent、RAG、MCP、Prompt 工程、系统设计和面试复习相关的资料做成一份复习索引与学习笔记。详细图解、完整案例和原文推导请回到对应链接阅读。

## 1. 总体学习路线

### 阶段 1：先建立面试与工程知识地图

先看总入口，明确 AI 应用开发面试到底考什么，避免只背概念。

| 资料 | 重点 | 适合目标 |
| --- | --- | --- |
| [AI 应用开发面试指南](https://javaguide.cn/ai/interview-questions/ai-interview-guide.html) | 大模型、RAG、Agent、MCP、系统设计的高频追问 | 快速建立复习地图 |
| [AI 应用开发面试题专题](https://javaguide.cn/ai/interview-questions/) | 四类面试题入口和阅读顺序 | 刷题前确定范围 |
| [AI 应用开发知识体系](https://javaguide.cn/ai/) | 大模型、Agent、RAG、MCP、Prompt、系统设计完整索引 | 系统学习主入口 |

### 阶段 2：补齐大模型基础

目标是把模型调用从“会调接口”升级到“知道成本、延迟、稳定性、结构化输出和评测怎么控制”。

| 资料 | 重点 | 学完要能回答 |
| --- | --- | --- |
| [大模型基础面试题总结](https://javaguide.cn/ai/interview-questions/llm-interview-questions.html) | Token、上下文、采样参数、API、结构化输出、评测 | 面试官问基础题时如何讲工程影响 |
| [LLM 运行机制](https://javaguide.cn/ai/llm-basis/llm-operation-mechanism.html) | Token、上下文窗口、Temperature、Top-p、Top-k、Token 预算 | 为什么长上下文不等于效果更好 |
| [大模型 API 调用工程实践](https://javaguide.cn/ai/llm-basis/llm-api-engineering.html) | 流式输出、重试、限流、超时、结构化返回 | 一个模型 API 怎么接入生产系统 |
| [大模型结构化输出详解](https://javaguide.cn/ai/llm-basis/structured-output-function-calling.html) | JSON Schema、Function Calling、Tool Calling、MCP 边界 | 为什么不能只靠 Prompt 要求 JSON |
| [AI 应用评测体系](https://javaguide.cn/ai/llm-basis/llm-evaluation.html) | Golden Set、LLM-as-Judge、Trace 回放、灰度闭环 | 如何证明新版 Prompt 或模型更好 |

### 阶段 3：学习 Prompt、Context 与 Agent

目标是理解复杂 AI 应用怎么编排，什么场景该用工作流，什么场景才适合 Agent。

| 资料 | 重点 | 学完要能回答 |
| --- | --- | --- |
| [AI Agent 面试题总结](https://javaguide.cn/ai/interview-questions/agent-interview-questions.html) | Agent Loop、Memory、Prompt、Context、MCP、Skills、Workflow | Agent 面试怎么讲边界和落地 |
| [AI Agent 核心概念](https://javaguide.cn/ai/agent/agent-basis.html) | Agent = LLM + Planning + Memory + Tools | Agent 和 Chatbot、Workflow 的区别 |
| [AI Agent 记忆系统](https://javaguide.cn/ai/agent/agent-memory.html) | 短期记忆、长期记忆、记忆生命周期 | 长任务里怎么避免上下文丢失 |
| [提示词工程实战指南](https://javaguide.cn/ai/agent/prompt-engineering.html) | Prompt 结构、约束、示例、注入风险 | Prompt 如何从 Demo 走向可维护 |
| [上下文工程实战指南](https://javaguide.cn/ai/agent/context-engineering.html) | 动态信息挂载、Token 预算、上下文持久化 | Context Engineering 和 Prompt Engineering 的差异 |
| [Agent Skills](https://javaguide.cn/ai/agent/skills.html) | 将流程、模板、脚本、校验规则封装成可复用能力 | Skills 和 Prompt、MCP、Function Calling 的区别 |
| [MCP 协议](https://javaguide.cn/ai/agent/mcp.html) | MCP Client、MCP Server、工具标准化、权限治理 | MCP 和 Function Calling 是什么关系 |
| [Harness Engineering](https://javaguide.cn/ai/agent/harness-engineering.html) | Model + Harness、上下文、工具、执行环境、验证 | 为什么强模型还需要工程外壳 |
| [AI Workflow、Graph 与 Loop](https://javaguide.cn/ai/agent/workflow-graph-loop.html) | 节点、边、状态、循环、安全边界 | 工作流、图和 Agent Loop 怎么选 |

### 阶段 4：系统掌握 RAG

目标是能把企业知识库问答拆成可排查、可优化、可评测、可更新的链路。

| 资料 | 重点 | 学完要能回答 |
| --- | --- | --- |
| [RAG 面试题总结](https://javaguide.cn/ai/interview-questions/rag-interview-questions.html) | RAG 基础、Embedding、向量库、Chunk、检索优化、GraphRAG、更新、评测 | RAG 面试如何从链路排查问题 |
| [RAG 基础概念](https://javaguide.cn/ai/rag/rag-basis.html) | 检索、增强、生成，RAG 和搜索、微调、长上下文的取舍 | 为什么 RAG 不能彻底消灭幻觉 |
| [RAG 文档处理与切分策略](https://javaguide.cn/ai/rag/rag-document-processing.html) | PDF/Word/Excel 解析、清洗、结构化、Chunk、多模态 | 为什么文档处理是 RAG 效果上限 |
| [RAG 向量索引算法和向量数据库](https://javaguide.cn/ai/rag/rag-vector-store.html) | Embedding、HNSW、IVFFLAT、向量数据库选型 | Milvus、pgvector、ES/OpenSearch 怎么取舍 |
| [RAG 知识库文档更新策略](https://javaguide.cn/ai/rag/rag-knowledge-update.html) | 增量更新、版本控制、去重、回滚、全量重建 | 知识库更新后怎么保证一致性 |
| [GraphRAG 详解](https://javaguide.cn/ai/rag/graphrag.html) | 实体、关系、社区发现、全局检索、局部检索 | GraphRAG 适合哪些复杂问题 |
| [RAG 检索优化](https://javaguide.cn/ai/rag/rag-optimization.html) | Hybrid Search、Query Rewrite、Rerank、上下文压缩 | RAG 召回率低、排序差、生成错分别怎么排查 |

### 阶段 5：把 Demo 升级成生产级 AI 系统

目标是从“功能能跑”升级到“稳定、可观测、可控成本、可评测、可治理”。

| 资料 | 重点 | 学完要能回答 |
| --- | --- | --- |
| [AI 系统设计面试题总结](https://javaguide.cn/ai/interview-questions/ai-system-design-interview-questions.html) | 生产级架构、模型网关、Prompt 管理、RAG、Memory、Tool、评测、安全 | 社招 AI 系统设计题怎么回答 |
| [AI 应用系统设计](https://javaguide.cn/ai/system-design/ai-application-architecture.html) | 入口层、编排层、Prompt/Context、RAG、Memory、Tool、可观测、评测、安全 | 如何设计企业级 AI 应用 |
| [大模型网关详解](https://javaguide.cn/ai/system-design/llm-gateway.html) | 多模型路由、Fallback、限流、配额、成本归因、审计、缓存 | 为什么生产系统需要 LLM Gateway |
| [AI 语音技术详解](https://javaguide.cn/ai/system-design/ai-voice.html) | VAD、ASR、LLM、TTS、流式播放、打断处理 | 实时语音 Agent 的低延迟链路怎么设计 |

## 2. 核心笔记

### 2.1 大模型基础

Token 是模型处理文本的基本单位，直接影响上下文容量、计费、响应延迟和截断风险。生产系统里不能只看模型标称窗口，还要把 system prompt、用户输入、历史消息、工具 schema、RAG 证据和最大输出一起算进预算。

上下文窗口不是越大越好。长上下文会带来更高延迟、更高成本、更大攻击面，也可能出现中间信息被忽略的问题。RAG、摘要、历史压缩、分阶段调用通常比无脑塞满上下文更稳。

Temperature、Top-p、Top-k 控制输出随机性和候选空间。生产里的结构化抽取、分类、工具参数生成通常偏低温；创意文案、头脑风暴可以适当提高随机性。不要只靠温度控制稳定性，还要做 schema 校验、重试和兜底。

结构化输出不要只依赖“请输出 JSON”。更可靠的链路是：定义 JSON Schema 或函数参数契约，调用模型生成结构化结果，服务端校验字段和类型，失败后重试或降级，并记录失败样本进入评测集。

Function Calling / Tool Calling 是模型选择和填充工具调用参数的能力。MCP 更偏向工具和上下文接入协议，把工具能力标准化，让不同客户端复用同一批 MCP Server。二者不是简单替代关系。

AI 应用评测要做业务 Golden Set。公开 benchmark 只能说明模型通用能力，不能证明你的客服、知识库、Agent 项目效果稳定。更好的方式是沉淀真实问题、期望答案、引用证据、失败类型和线上 Trace，形成可回放的质量闭环。

### 2.2 Prompt 与 Context

Prompt Engineering 关注“怎么把指令写清楚”。常见要素包括角色、任务、输入、约束、输出格式、示例和反例。它适合解决表达、格式、风格和简单推理约束。

Context Engineering 关注“模型这次调用应该看到什么”。它包含静态规则、动态业务数据、历史消息、RAG 证据、工具结果、用户状态、权限信息和 Token 预算。Agent 和 RAG 场景里，Context 往往比单条 Prompt 更决定效果。

一个可维护的 Prompt 不应该散落在代码里。生产环境建议做版本管理、变量模板、灰度发布、评测回放和变更记录。否则模型、Prompt、上下文、工具任何一项变化都可能让线上行为漂移。

Prompt 注入不是只靠提示词能解决的问题。需要结合上下文隔离、工具权限、敏感操作确认、输出过滤、审计日志和沙箱执行。越是能读写文件、调用 API、执行代码的 Agent，越要重视权限边界。

### 2.3 Agent

Agent 可以理解为围绕 LLM 构建的任务执行系统，常见结构是 LLM + Planning + Memory + Tools。它和普通聊天机器人的区别是能观察环境、决定下一步、调用工具、接收反馈并继续迭代。

Agent Loop 通常包含观察、规划、行动、反馈和终止判断。Loop 让 Agent 能处理不确定步骤，但也会带来成本增加、循环失控、错误累积和可观测困难。复杂流程不一定都要纯 Agent，稳定业务流程优先考虑 Workflow 或 Graph。

Memory 分短期和长期。短期记忆通常是当前任务上下文和最近对话，解决“刚才做到了哪一步”；长期记忆通常是向量库、知识图谱或数据库，解决“过去积累了什么”。记忆要考虑写入策略、召回策略、遗忘策略和隐私风险。

Tools 让 Agent 从“会说”变成“会做”。但工具调用必须有权限、参数校验、幂等、超时、重试、审计和回滚。高危操作应该人工确认，不能让模型直接越权执行。

MCP 的价值在于统一工具接入方式。真正落地时，难点不只是协议，而是谁能调用工具、工具能访问哪些数据、失败如何处理、调用日志如何审计。

Agent Skills 可以把稳定的操作流程封装成可复用能力。它适合那些需要固定步骤、模板、脚本、校验和产物验收的任务。相比单条 Prompt，Skills 更工程化；相比 MCP，Skills 更关注“怎么完成任务”而不只是“怎么接工具”。

Harness Engineering 强调模型外部的工程环境。模型负责推理和生成，Harness 负责上下文管理、工具执行、状态恢复、权限控制、结果验证和观测追踪。强模型没有好 Harness，也很难稳定完成长任务。

### 2.4 RAG

RAG 的核心不是“接一个向量数据库”，而是把正确证据稳定、低成本、可治理地送到模型面前。典型链路分为离线索引和在线检索生成。

离线索引包括文档解析、清洗、结构化、元数据补充、Chunk 切分、Embedding、索引构建和权限字段写入。在线链路包括 Query 理解、Query Rewrite、召回、过滤、Rerank、上下文构建、答案生成、引用和评测。

RAG 适合知识会变化、资料很长、需要引用溯源、依赖企业私有数据的场景。它不适合所有搜索问题。如果用户只是想找原文、模板或编号，传统搜索可能更快、更便宜、更可控。

RAG 和微调不是替代关系。RAG 解决“模型不知道新知识或私有知识”；微调更适合解决“模型不会按指定风格、格式或任务模式做事”。知识频繁变化时优先 RAG，固定输出习惯或领域能力时再考虑微调。

Chunk 策略直接影响召回上限。Chunk 太大容易引入噪声，太小容易丢上下文。标题、表格、代码块、列表、图片 OCR、页眉页脚都要特殊处理。不要把 PDF 解析坏了之后再指望更贵的 Embedding 模型救回来。

Embedding 和向量库只解决语义相似召回的一部分问题。真实系统常常需要关键词检索、向量检索、元数据过滤、混合检索、候选池扩大、Rerank 和上下文压缩组合使用。

RAG 效果差要分段排查。先看正确证据有没有进入候选池；再看正确证据排序是否靠前；再看上下文是否被截断、重复或污染；最后看模型是否忠实使用证据回答。只改 Prompt 往往治标不治本。

GraphRAG 适合实体关系强、跨文档推理多、需要全局视角的问题。它能补足纯向量检索在关系推理和全局问题上的短板，但成本、构建复杂度、更新难度也更高，不应默认替代普通 RAG。

知识库更新不是简单覆盖文件。需要考虑文档版本、增量索引、去重、回滚、灰度、旧索引清理和权限一致性。否则可能出现新旧答案混杂、引用过期、权限泄露等问题。

### 2.5 AI 系统设计

生产级 AI 应用通常包含入口层、业务编排层、Prompt/Context 管理、模型网关、RAG、Memory、Tool 调用、异步任务、观测日志、评测平台、安全治理和成本分析。

模型网关是生产 AI 系统的关键组件。它负责统一模型接入、多模型路由、Fallback、限流、配额、成本归因、缓存、审计和监控。没有网关，模型供应商限流、价格变化、故障切换和成本统计都会很难治理。

可观测性不能只记录最终回答。至少要记录请求参数、Prompt 版本、上下文摘要、检索结果、Rerank 分数、工具调用、模型用量、延迟、错误类型和用户反馈。这样线上出问题才能复盘。

安全治理要覆盖 Prompt 注入、RAG 权限过滤、工具越权、隐私泄露、日志脱敏、供应商数据合规和高危操作确认。AI 应用的风险不是只出在模型，也会出在上下文和工具链路。

实时语音 Agent 的难点在链路延迟和交互状态。VAD、ASR、LLM、TTS、音频播放、打断处理、状态机、流式输出都要协作。目标不是模型回答最完整，而是在低延迟下保持自然、可打断、可恢复。

## 3. 高频面试题与答题框架

### 3.1 通用答题结构

建议用“概念 -> 影响 -> 方案 -> 边界”组织答案。

| 步骤 | 说明 |
| --- | --- |
| 概念 | 一句话讲清楚它是什么 |
| 影响 | 说明它对成本、延迟、质量、稳定性或安全有什么影响 |
| 方案 | 给出生产环境中的设计、排查、优化或治理方法 |
| 边界 | 说明适用场景、不适用场景和取舍 |

### 3.2 大模型基础高频题

| 问题 | 答题要点 |
| --- | --- |
| Token 是什么，为什么重要？ | Token 是模型处理文本的单位，影响上下文容量、成本、延迟和截断风险；工程上要做预算、监控 usage、压缩历史和限制输出。 |
| 上下文窗口越大越好吗？ | 不一定。长上下文会提高成本和延迟，还可能出现关键信息被忽略；应结合 RAG、摘要、分段处理和上下文压缩。 |
| Temperature、Top-p、Top-k 怎么调？ | 低温更稳定，高温更发散；结构化任务偏低温，创意任务可提高；生产稳定性还要靠 schema、校验和重试。 |
| 为什么大模型会幻觉？ | 模型基于概率生成，不等于事实数据库；可通过 RAG、工具调用、引用校验、拒答策略和评测缓解，但不能完全消灭。 |
| 结构化输出如何保证稳定？ | 使用 JSON Schema、Function Calling、服务端校验、失败重试、兜底解析、失败样本回放，而不是只写一句“严格输出 JSON”。 |

### 3.3 Agent 高频题

| 问题 | 答题要点 |
| --- | --- |
| Agent 和 Chatbot 有什么区别？ | Chatbot 主要回复，Agent 能规划、调用工具、接收反馈并迭代执行；但 Agent 成本和风险更高。 |
| Agent 和 Workflow 怎么选？ | 流程固定、可枚举、稳定性要求高时用 Workflow；步骤不确定、需要动态判断时用 Agent；复杂场景可混合。 |
| Agent Loop 是什么？ | 观察、规划、行动、反馈和终止判断的循环；核心价值是动态执行，风险是循环失控、成本升高和错误累积。 |
| Memory 怎么设计？ | 短期记忆保存当前上下文，长期记忆保存历史经验和知识；要考虑写入、召回、淘汰、权限和隐私。 |
| MCP 和 Function Calling 有什么区别？ | Function Calling 是模型侧工具调用表达，MCP 是工具和上下文接入协议；MCP 让工具服务可被多个客户端复用。 |

### 3.4 RAG 高频题

| 问题 | 答题要点 |
| --- | --- |
| 什么是 RAG？ | 检索增强生成，回答前先从外部知识库取证据，再让模型基于证据生成。 |
| RAG 和微调怎么选？ | 知识更新快、需要引用和私有数据时优先 RAG；要固定风格、格式或任务能力时考虑微调。 |
| RAG 召回率低怎么排查？ | 先看文档解析和 Chunk，再看 Embedding、元数据、Query Rewrite、Hybrid Search、候选池和权限过滤。 |
| Rerank 解决什么问题？ | 它不扩大召回范围，而是在候选结果中重新排序，把更相关证据排到前面。 |
| GraphRAG 什么时候值得用？ | 实体关系强、跨文档推理多、需要全局概览时值得考虑；普通 FAQ 和简单知识库不一定需要。 |
| 知识库更新怎么设计？ | 做增量索引、版本管理、去重、灰度、回滚和旧索引清理，保证引用和权限一致。 |

### 3.5 系统设计高频题

| 问题 | 答题要点 |
| --- | --- |
| 如何设计企业级知识库问答系统？ | 入口鉴权、文档处理、索引、检索、Rerank、上下文构建、模型网关、引用、评测、观测、安全。 |
| 为什么需要模型网关？ | 统一多模型接入，做路由、Fallback、限流、配额、成本统计、缓存、审计和观测。 |
| 如何证明 Prompt 新版本更好？ | 用 Golden Set 离线回放，结合人工标注、LLM-as-Judge、线上灰度、Trace 对比和失败样本归因。 |
| 工具调用如何保证安全？ | 最小权限、参数校验、敏感操作确认、沙箱、幂等、审计日志、失败回滚。 |
| 实时语音 Agent 怎么设计？ | VAD 识别语音边界，ASR 流式转写，LLM 流式生成，TTS 流式播报，支持打断、状态机和低延迟链路。 |

## 4. 复习计划

### 7 天快速复习

| 天数 | 任务 |
| --- | --- |
| 第 1 天 | 看 AI 应用开发面试指南和面试题专题，建立全局地图。 |
| 第 2 天 | 看 LLM 运行机制、大模型基础面试题，整理 Token、上下文、采样参数。 |
| 第 3 天 | 看 API 工程实践、结构化输出、评测体系，整理生产调用链路。 |
| 第 4 天 | 看 Agent 核心概念、Agent 面试题，整理 Loop、Memory、Tools、Workflow。 |
| 第 5 天 | 看 RAG 基础、RAG 面试题，画出离线索引和在线检索链路。 |
| 第 6 天 | 看 RAG 文档处理、向量库、检索优化、知识库更新，整理排查框架。 |
| 第 7 天 | 看 AI 系统设计、模型网关，按“概念 -> 影响 -> 方案 -> 边界”模拟回答。 |

### 14 天扎实复习

| 阶段 | 任务 |
| --- | --- |
| 第 1-2 天 | 总览 AI 应用开发知识体系和面试指南，列出个人薄弱点。 |
| 第 3-5 天 | 深入大模型基础、API 调用、结构化输出、评测。 |
| 第 6-8 天 | 深入 Agent、Prompt、Context、MCP、Skills、Workflow。 |
| 第 9-11 天 | 深入 RAG 基础、文档处理、向量库、检索优化、GraphRAG、知识库更新。 |
| 第 12-13 天 | 深入 AI 系统设计、模型网关、语音 Agent、安全治理。 |
| 第 14 天 | 用项目经历做一次完整复盘，准备 3 个系统设计题和 20 个高频问答。 |

## 5. 项目表达模板

面试讲 AI 项目时，不要只说“我接了大模型 API”或“用了 LangChain”。可以按下面结构讲。

| 模块 | 建议表达 |
| --- | --- |
| 业务场景 | 项目解决什么业务问题，用户是谁，输入输出是什么。 |
| 总体架构 | 入口层、业务编排、Prompt/Context、RAG、Tool、模型网关、评测和观测怎么分层。 |
| 大模型调用 | 模型怎么选，怎么处理流式输出、重试、限流、结构化输出和成本。 |
| RAG 链路 | 文档如何解析、切分、向量化、检索、重排、引用和更新。 |
| Agent 编排 | 是否使用 Agent Loop，哪些步骤固定成 Workflow，哪些工具可调用。 |
| 质量评测 | Golden Set 怎么构建，如何做离线回放、线上灰度和人工反馈。 |
| 安全治理 | 权限、Prompt 注入、工具越权、日志脱敏和高危操作确认怎么做。 |
| 线上问题 | 遇到过什么效果问题，如何定位到文档、召回、排序、上下文或模型生成阶段。 |

## 6. 最后复习清单

- 能画出 AI 应用的生产级架构分层。
- 能解释 Token、上下文窗口、采样参数对成本、延迟和质量的影响。
- 能说明结构化输出为什么需要 schema、校验和重试。
- 能区分 Prompt Engineering、Context Engineering、Function Calling、Tool Calling、MCP 和 Skills。
- 能说明 Agent、Workflow、Graph、Loop 的适用边界。
- 能画出 RAG 离线索引和在线检索生成链路。
- 能按文档处理、召回、排序、上下文、生成、评测排查 RAG 效果差。
- 能讲清模型网关、Fallback、限流、配额、成本归因和审计的价值。
- 能说明 AI 应用评测如何从 Golden Set 到线上灰度闭环。
- 能把自己的项目按“业务场景 -> 架构 -> 关键链路 -> 评测 -> 安全 -> 线上问题”讲完整。
