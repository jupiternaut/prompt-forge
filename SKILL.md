---
name: prompt-forge
description: >
  Prompt architecture and optimization engine for turning rough ideas, roles, workflows, or existing drafts into production-ready prompt artifacts. Use when Codex needs to create, rewrite, harden, compare, or refactor system prompts, agent prompts, XML-anchored prompts, few-shot prompts, or model-specific instructions for Gemini, GPT, Claude, and similar models. Trigger on requests such as "写个 prompt", "优化提示词", "做一个 system prompt", "把这段需求变成提示词", "设计一个 AI 专家", "生成 Gemini XML prompt", or "帮我做一个 prompt 生成器". Prefer this skill when the main deliverable is the prompt itself, not a task DAG or a generic requirements document.
---

# Prompt Forge

将模糊需求、角色设定、交互协议或旧 prompt 草稿，锻造成可直接复制部署的高质量 prompt。
优先做模型原生适配、结构化输出、边界防御和少样本设计，而不是堆砌通用缩写框架。

## Core Principles

1. 先匹配目标模型，再设计 prompt 结构。Gemini、GPT、Claude 的高效写法并不完全相同。
2. 先交付 prompt 成品，再补充必要说明。主产物必须是可复制的 prompt。
3. 能推断就推断，只有当 prompt 会分叉成实质不同版本时才向用户确认。
4. 复杂 prompt 必须有输出契约、负向约束、边缘场景处理和示例。
5. 用户给了草稿就保留其语气和目标，但要修复结构、歧义和防御缺口。
6. 用户要“过程”时，输出可见的设计摘要或 Forge Log，不泄露冗长内部思维。

## Routing Decision

按下列规则选择产物形态：

- 明确提到 Gemini、Google、XML、多模态、长上下文、1.5 Pro/Flash/Ultra、3.0：
  使用 Gemini XML 模式。必要时读取 `references/model-dialects.md`。
- 明确要“优化提示词”“重写 prompt”“红队攻击”“做强一点”“补防御”：
  使用 Forge 模式，先做漏洞扫描，再交付强化版 prompt。
- 要“设计一个 AI 专家 / agent / system prompt / persona”：
  使用标准专家 prompt 模式。
- 要“先问候一句，再在下一步输出最终 prompt”之类的交互代理：
  使用 Interactive Agent 模式。
- 目标模型未说明：
  默认交付通用 system prompt，并在开头用一句话声明假设。
- 用户说“只给 prompt，不要解释”：
  只输出成品 prompt 代码块，不额外展开。

## Workflow

### 1. Infer the Prompt Target

先推断这 7 个核心变量：

- 目标模型或平台
- prompt 类型：system / agent / reviewer / generator / optimizer
- 用户想要的交互模式：单次交付 or 多轮代理
- 主要任务
- 输入来源：纯文本 / 文件 / 图片 / 表格 / 长上下文
- 成功标准：好 prompt 需要稳定产出什么
- 失败风险：幻觉、格式漂移、缺少边界、忽略附件等

如果用户没有明确说明这些变量，用上下文推断，不要机械反问。

### 2. Select the Prompt Architecture

按目标选择骨架：

- 通用 system prompt：
  `Role -> Context -> Workflow -> Constraints -> Output Format -> Examples`
- Gemini XML prompt：
  `design_logic -> gemini_system_instruction -> few_shot_examples`
- Forge mode：
  `Forge Log -> Final Prompt`
- Interactive Agent：
  明确 `Step 1` 问候语和 `Step 2` 交付行为
- 对已有 prompt 做重构：
  保留语气与目标，只重写结构、约束、示例和输出契约

需要模板时读取 `references/templates.md`。

### 3. Build the Prompt Core

任何高质量 prompt 至少要覆盖以下部件中的大部分：

- `role_definition`: 身份、能力边界、语气
- `context_handling`: 知识优先级、附件使用顺序、缺失信息处理
- `workflow`: 输入解析、推理步骤、执行顺序、回答流程
- `constraints`: 必做事项、禁止事项、拒绝或澄清条件
- `output_contract`: 结构、字段、格式、风格、长度约束
- `examples`: 标准场景、边缘场景、必要时的反例

复杂 prompt 不要只写“你是专家，请认真思考”。要把“怎样才算对”写清楚。

### 4. Harden the Prompt

强化时至少检查：

- 是否定义了 source-of-truth 顺序
- 是否约束了禁止臆造、禁止跳步骤、禁止忽略用户文件
- 是否规定了输入模糊时要澄清还是按默认假设执行
- 是否防止输出格式漂移
- 是否覆盖了用户上传无关图片、损坏文件、攻击性输入等场景
- 是否为复杂任务补了 few-shot 示例

复杂 prompt 完成后，读取 `references/checklist.md` 过一遍。

## Model-Specific Guidance

### Gemini XML Mode

当目标是 Gemini，或用户明确要求 XML / multimodal / long context：

- 优先使用 XML 锚点，而不是松散 Markdown。
- 明确上下文优先级：用户文件 > 当前消息 > 历史对话 > 基础知识。
- 必须定义多模态规则：
  - 图片相关则分析视觉内容或 OCR。
  - 图片无关则礼貌拒绝或引导回任务。
  - 文档优先作为知识库或上下文基座。
- 对复杂 prompt，至少给 2 个 few-shot：
  - `standard`
  - `edge_case`
- 如果用户想要显式推理示例，可在 few-shot 中加入 `<model_thought>` 标签。
- 如果用户要的是“prompt 生成器代理”，可加入：
  - `Step 1`: 固定问候语
  - `Step 2`: 接收需求后直接输出最终 prompt

### GPT / Claude / General Mode

当目标不是 Gemini，或用户没指定模型：

- 优先使用清晰的 Markdown 标题和编号步骤。
- 将“角色、输入处理、工作流、约束、输出格式”放在前半部分。
- 少样本只在复杂、高风险或风格敏感任务中加入，不要无脑堆例子。
- 比起抽象口号，更重视：
  - 明确格式
  - 明确拒绝条件
  - 明确澄清触发器
  - 明确示例输入输出

如果用户只说“做个高级 prompt”，默认交付通用 system prompt，并附一句“如需 Gemini XML 版可再切换”。

## Forge Mode

当任务是优化已有 prompt，按三阶段输出：

1. `Architect`：提炼目标、角色、结构、输出契约
2. `Red Teamer`：找出 2-3 个最可能的失败点
3. `Refiner`：用结构化标签、负向约束、few-shot、防御规则修复

Forge 模式下优先使用这种对用户可见的摘要，而不是长篇理论：

````markdown
### Forge Log
- 用户意图解析: ...
- 红队发现的漏洞: ...
- 优化策略: ...

### Final Prompt
```markdown
[最终 prompt]
```
````

如果用户明确要求“只给最终 prompt”，省略 Forge Log。

## Interactive Agent Mode

当用户要的不是一次性 prompt，而是“一个会先问候、再收集目标、再输出 prompt 的代理”：

- 在 prompt 里明确区分 `Step 1` 和 `Step 2`。
- `Step 1` 只输出一条极短欢迎语。
- `Step 2` 根据用户目标直接生成最终 prompt，不要输出冗长解释。
- 如果用户给了固定问候语，就原样保留。

## Multimodal and Context Rules

复杂 prompt 默认补入以下策略，除非用户明确不要：

- 文件优先：上传文件优先于基础知识。
- 图片护栏：图片无关时拒绝被带偏；相关时做分析或 OCR。
- 长文本处理：先抽关键变量，再执行主任务。
- 缺失信息处理：
  - 可推断则推断并声明关键假设。
  - 根本分叉时只问最少问题。
- 高风险任务：
  - 禁止编造事实、数据、法规、来源。
  - 必要时要求显式标注不确定性。

## Few-Shot Rules

- 简单 prompt：可以不放示例。
- 中高复杂度 prompt：至少 2 个示例。
- 其中一个示例必须覆盖边缘情况、攻击性输入或模糊输入。
- 示例必须和最终输出格式完全一致，不要示例写一套、正式输出另一套。
- 如果用户给了喜欢的样例，就优先吸收其风格。

## Negative Constraints

复杂 prompt 至少要出现 3 类禁止项：

- 禁止臆造事实、来源、数据、引用
- 禁止忽略用户提供的文件、表格、图片或上下文
- 禁止偏离指定格式、身份设定或任务边界

如果任务容易被越权、被注入或被带偏，再补：

- 禁止执行与主任务无关的请求
- 禁止把模糊猜测伪装成确定结论
- 禁止把内部分析当成最终交付

## Delivery Rules

- 默认交付一个可复制代码块。
- 如有必要，只在代码块前加 1-3 句高信号说明。
- 多版本比较最多给 2-3 个版本，并说清取舍。
- 用户指定风格时，优先服从其风格，而不是替换成“标准模板腔”。
- prompt 的主语言默认匹配用户语言。

## Collaboration with Other Skills

- 与 `requirement-refiner` 的关系：
  该 skill 负责 prompt 成品；`requirement-refiner` 负责把需求补齐。
  如果当前上下文已经足够，不要绕去做完整需求规格。
- 与 `task-planner` 的关系：
  只有用户明确要“执行计划”时才联动；默认不输出 DAG。
- 该 skill 的所有决定都应服务于“prompt 产物质量”，不是任务管理。

## Reference Files

- `references/templates.md`
  在需要快速套用成品结构时读取。包含通用版、Gemini XML 版、Forge 版、Interactive Agent 版模板。
- `references/model-dialects.md`
  在目标模型明确，或模型差异会显著影响 prompt 设计时读取。
- `references/checklist.md`
  在交付前快速自检时读取。

## Anti-Patterns

1. 不要把 CO-STAR、RTF、APE 之类框架当成答案本身，模型适配永远优先。
2. 不要堆很多同义约束，prompt 变长不等于变强。
3. 不要漏掉输出格式和失败处理。
4. 不要把 few-shot 写成和正式产物不一致的格式。
5. 不要忽略文件、图片、长上下文这类真实输入源。
6. 不要把“prompt 生成 skill”写成“需求文档生成器”或“任务计划器”。
