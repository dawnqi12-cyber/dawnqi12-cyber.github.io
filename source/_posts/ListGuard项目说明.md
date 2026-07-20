---
title: ListGuard 项目说明
date: 2026-07-20 12:00:00
tags:
- ListGuard
- AI应用开发
- 项目复习
categories:
- 技术学习
---

# ListGuard 项目说明

## 一、简历项目描述

### 精简版

ListGuard 是一个面向跨境电商场景的 Listing 生成与合规检查工具，针对人工撰写效率低、平台规则分散、模型输出不稳定和宣传内容难以追溯等问题，构建了从商品事实输入、结构化 Listing 生成、JSON 校验到 YAML 规则合规检查和质量评估的完整流程。项目使用 Python 3.12、Streamlit、Pydantic、PyYAML 和 pytest，采用 LLM Provider 抽象隔离模型供应商，并提供无需 API Key 的确定性 Mock 模式。

### 简历要点版

- 面向跨境电商 Listing 生成场景，设计商品事实输入、标题/五点/详情描述输出和合规检查流程，降低模型生成内容不可控和平台规则难维护的问题。
- 使用 Python 3.12、Streamlit、Pydantic、PyYAML、pytest，采用 `LLMProvider` 抽象隔离模型供应商，支持无 API Key 的 Mock 模式和显式配置的 OpenAI-compatible Provider。
- 通过 YAML 配置平台与国家规则，支持禁用词、最大长度、必需事实、未经证明声明、绝对化表达和免责声明检查，并返回规则 ID、风险等级、命中文本、原因和修改建议。
- 对模型返回内容执行 JSON 解析、对象类型校验和 Pydantic `ListingDraft` 结构校验；对空输出、非法 JSON、Schema 不匹配和 Provider 异常提供清晰错误信息。
- 使用 pytest 覆盖数据模型、Provider 选择、Mock 稳定性、Prompt 约束、JSON 解析、YAML 规则加载、各类合规规则、演示案例和确定性质量评估，当前本地测试结果为 44 项通过。

### 一句话版本

基于 Python、Streamlit、Pydantic 和 YAML 规则构建跨境电商 Listing 生成与合规检查工具，通过 Provider 抽象、结构化 JSON 校验、确定性规则引擎和质量评分降低模型输出风险，并使用 Mock 模式支持无 API Key 演示。

## 二、项目解决的业务问题

跨境电商 Listing 通常需要同时处理商品事实、目标平台、国家、语言、关键词和宣传限制。实际工作中容易出现以下问题：

1. 商品标题、五点描述和详情描述需要重复撰写，效率较低。
2. 不同平台和国家的规则分散，修改规则往往需要修改代码。
3. 模型可能补充用户没有提供的认证、功效、参数、排名或销量信息。
4. 模型输出可能不是可解析 JSON，无法稳定交给后续系统处理。
5. 仅依靠模型主观判断合规性，结果不稳定且难以测试。

ListGuard 的处理方式是将生成和检查拆开：模型负责生成结构化草稿，确定性规则负责检查，质量模块负责计算可解释的指标。

## 三、技术栈与工程设计

| 技术或模块 | 用途 |
| --- | --- |
| Python 3.12 | 应用开发语言和运行环境 |
| Streamlit | 商品输入、Listing 展示、合规结果筛选和文件下载 |
| Pydantic | 商品输入、生成请求、Listing 输出、规则和合规结果的数据模型与校验 |
| PyYAML | 加载可编辑的通用规则和平台/市场规则 |
| LLM Provider abstraction | 将生成流程与具体模型供应商解耦 |
| MockProvider | 无网络、无 API Key 的稳定演示和测试 |
| OpenAICompatibleProvider | 通过环境变量连接兼容 Chat Completions 的外部模型 |
| pytest | 单元测试、边界测试和核心流程回归测试 |

## 四、启动流程

### 1. 获取代码

```powershell
git clone https://github.com/dawnqi12-cyber/ListGuard.git
Set-Location ListGuard
```

### 2. 创建 Python 环境

项目要求 Python 3.12：

```powershell
python --version
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

### 3. 安装依赖

```powershell
python -m pip install -e ".[dev]"
```

主要依赖包括 Streamlit、Pydantic、PyYAML 和 pytest。

### 4. 使用 Mock 模式启动

Mock 是默认模式，不需要 API Key，也不会向外部模型发送商品数据：

```powershell
$env:LLM_PROVIDER = "mock"
streamlit run app.py
```

浏览器打开 Streamlit 输出的本地地址，通常为 `http://localhost:8501`。

### 5. 配置真实模型模式

只有显式设置 `LLM_PROVIDER` 后，应用才会尝试调用外部 Provider：

```powershell
$env:LLM_PROVIDER = "openai_compatible"
$env:LLM_API_BASE_URL = "https://your-provider.example/v1"
$env:LLM_API_KEY = "your-key"
$env:LLM_MODEL = "your-model"
$env:LLM_TIMEOUT_SECONDS = "30"
streamlit run app.py
```

API Key 只从环境变量读取，不写入代码。项目不会自动加载 `.env` 文件；`.env.example` 仅用于说明变量名称，真实密钥不应提交到 Git。

### 6. 运行测试

```powershell
pytest
```

如本机安装了额外的 `langsmith` pytest 插件，也可以运行：

```powershell
python -m pytest -q -p no:langsmith
```

当前本地验证结果：44 项测试通过。

## 五、各部分功能与作用

### `app.py`

Streamlit 启动入口，负责：

- 展示商品基本信息、事实、限制条件和目标设置输入区域。
- 加载演示案例并填充表单。
- 触发 Listing 生成和合规检查。
- 展示标题、五点描述、详情描述、关键词、来源事实和警告。
- 按风险等级筛选合规结果。
- 展示质量评估总分、指标、失败原因和改进建议。
- 下载 JSON 和 Markdown 格式的生成结果。
- 使用 `st.session_state` 保存当前草稿、原始请求、合规结果和质量评估，避免用新输入检查旧 Listing。

### `src/listing_tool/models.py`

定义核心数据结构：

- `ProductInput`：商品名称、类目、品牌、材质、特征、尺寸、市场、语言、平台、关键词和事实约束。
- `ListingGenerationRequest`：将商品事实与本次生成的目标上下文组合，并校验平台、国家和语言一致。
- `ListingDraft`：约束标题、五条 Bullet Points、详情描述、关键词、来源事实和警告的结构。
- `ComplianceRule`：定义 YAML 规则的字段、类型、严重等级、匹配器、原因和建议。
- `ComplianceFinding`：记录具体命中结果和命中位置。

模型使用非空、长度、列表项和额外字段拒绝等校验，避免错误数据进入后续流程。

### `src/listing_tool/generator.py`

提供与具体模型供应商无关的生成服务。它接收 `ListingGenerationRequest`，调用符合 `LLMProvider` 接口的 Provider，并返回 `ListingDraft`。

### `src/listing_tool/prompt_builder.py`

构建 Provider 无关的 Prompt：

- 只能使用用户输入的信息和 `factual_constraints`。
- 不得捏造认证、功效、参数、排名、销量、评论或性能。
- 不得使用 `prohibited_claims`。
- 不确定的信息必须写入 `warnings`，标记为待确认。
- 按目标语言生成。
- 将用户输入放在 `<untrusted_input_json>` 区块中，并明确禁止执行输入内容中的指令，以降低 Prompt 注入风险。

### `src/listing_tool/providers/`

#### `base.py`

定义 `LLMProvider` Protocol，使上层生成流程不依赖具体供应商。

#### `mock.py`

根据商品事实稳定生成结构化结果，不发起网络请求，适合测试、演示和截图。

#### `openai_compatible.py`

读取环境变量，向兼容 Chat Completions 的接口发送请求。它不把供应商、API Key 或模型名称写死在代码中，并设置超时和请求异常处理。

#### `parsing.py`

负责：

- 解析模型返回的 JSON 文本。
- 处理可选 Markdown code fence。
- 检查 JSON 是否为对象。
- 使用 Pydantic 校验是否符合 `ListingDraft`。
- 将非法 JSON、空输出和结构错误转换为可读的异常信息。

### `src/listing_tool/rules_loader.py`

从 `rules/*.yaml` 和 `rules/*.yml` 加载规则，并使用 `ComplianceRule` 验证格式。规则文件不存在、YAML 语法错误、规则字段缺失和正则表达式错误都会返回明确异常。

### `src/listing_tool/compliance.py`

执行确定性合规检查，支持：

- `prohibited_word`：检查禁用词，并保留重复命中。
- `max_length`：检查标题等字段长度。
- `required_fact`：检查商品必需事实。
- `unsupported_claim`：检查可能未经证明的声明。
- `absolute_claim`：检查绝对化、排名和保证性表达。
- `missing_disclaimer`：检查详情中是否缺少配置的免责声明。
- 平台和市场筛选。
- 用户输入的 `prohibited_claims` 运行时检查。

### `src/listing_tool/quality.py`

对生成结果进行代码计算，不调用 LLM 打分：

- 字段完整性。
- 标题长度。
- 关键词覆盖率。
- 禁用词命中数量。
- 未验证声明数量。
- 目标语言匹配启发式检查。
- JSON 结构正确性。

每项指标都会返回计算方式、得分、是否通过、失败原因和改进建议，最终使用加权公式计算总分。

### `rules/`

存放可编辑的演示规则。规则内容位于 YAML 中，平台和国家筛选不需要修改 Python 合规引擎代码。

### `data/demo_cases.yaml`

提供三个演示商品：

1. 家居收纳用品：设计为无风险案例。
2. 美妆个护用品：包含认证和绝对化宣传风险。
3. 电子配件：用于展示超长标题校验路径。

### `tests/`

测试覆盖：

- Pydantic 必填字段、空字符串、长度、上下文一致性和额外字段。
- Mock Provider 的稳定性和结构化输出。
- Provider 默认选择与环境变量配置错误。
- Prompt 约束和非可信输入边界。
- JSON 解析和 Schema 校验失败。
- YAML 规则加载、文件不存在和格式错误。
- 各种合规规则类型、大小写、空文本、重复命中和平台/市场筛选。
- 三个演示案例。
- 质量指标、加权总分、非法 JSON、语言启发式和规则类型统计。

## 六、限制与合规声明

- 这是个人作品集 MVP，不是生产级发布系统。
- 演示 YAML 规则不等同于 Amazon、eBay、Shopee 或任何国家的完整政策。
- 工具不提供法律、监管、税务、商标或平台政策意见。
- JSON 结构正确不代表内容事实真实，也不代表内容一定允许发布。
- 目标语言检查采用轻量启发式，可能存在误报和漏报。
- 真实模型模式的服务可用性、价格、数据保留和模型政策由外部 Provider 决定。
- 生成结果发布前仍需要人工审核和事实核验。

## 七、未来改进方向

- 增加带版本、生效日期和来源记录的平台规则包。
- 增加句子级事实到输出的可追溯性检查。
- 增加更可靠的语言识别和本地化规范化。
- 增加 Provider 重试、限流、脱敏日志和结构化诊断。
- 增加 Streamlit 端到端测试以及 CI 中的类型检查、Lint 和 Secret Scanning。
- 增加可导出的合规报告和人工审核记录。
- 支持按类目配置质量指标和评分权重。

