# Model Dialects

同一个 prompt 目标，按模型不同要换写法。

## Gemini

适用信号：
- 用户明确说 Gemini / Google AI Studio / 1.5 Pro / Flash / Ultra / 3.0
- 用户要 XML 结构
- 用户要多模态、长上下文、文件优先

写法重点：
- 用 XML 锚定主结构，而不是松散段落。
- 明确附件、图片、文档的处理协议。
- 对复杂任务优先给 `standard + edge_case` 两个 few-shot。
- 如用户明确要求，可在 few-shot 中加入 `<model_thought>` 标签演示推理过程。
- 对交互代理，单独定义 Step 1 问候语和 Step 2 交付行为。

## GPT / Claude / General

适用信号：
- 未指定模型
- 用户只说 system prompt / expert prompt / agent prompt
- 任务更偏文本、代码、分析、审阅

写法重点：
- 用 Markdown 层级组织角色、流程、约束、输出格式。
- 优先把输出契约写清楚，再补方法论。
- few-shot 只在复杂、高风险或风格敏感任务中加入。
- 明确澄清触发器、默认假设和禁止项。

## Universal Rules

所有模型都应补齐：
- source-of-truth 优先级
- 模糊输入处理策略
- 负向约束
- 输出格式锁定
- 边缘场景处理

不要把某个模型的特性硬搬到所有模型上。
