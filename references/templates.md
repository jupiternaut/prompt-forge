# Prompt Templates

按场景选模板，不要混用到失去重点。

## 1. Standard System Prompt

适用于通用 GPT / Claude / 未指定模型场景。

````markdown
# Role
[定义身份、专业背景、语气]

## Objective
[说明任务目标和成功标准]

## Context Handling
- Source priority: user-provided materials > current request > chat history > prior knowledge
- If information is missing: [推断 / 澄清规则]

## Workflow
1. Parse the task.
2. Extract key variables and constraints.
3. Reason through the task.
4. Produce the final output in the requested format.

## Constraints
- Must do: [核心要求]
- Forbidden: [禁止臆造 / 禁止越界 / 禁止偏离格式]

## Output Format
[明确结构、字段、风格、长度]

## Examples
### Standard
User: ...
Assistant: ...

### Edge Case
User: ...
Assistant: ...
````

## 2. Gemini XML Prompt

适用于 Gemini、XML、多模态、长上下文场景。

```xml
<design_logic>
    <intent_analysis>[用户核心目标]</intent_analysis>
    <edge_case_defense>[关键防御策略]</edge_case_defense>
</design_logic>

<gemini_system_instruction>
    <role_definition>
        [定义 Agent 的身份、专业背景及语气风格]
    </role_definition>

    <context_handling>
        [定义知识获取优先级：用户文件 > 当前消息 > 历史对话 > 基础知识]
        <multimodal_protocol>
            <image_rule>[相关图片则分析视觉内容或 OCR；无关图片则礼貌拒绝或引导回任务]</image_rule>
            <file_rule>[用户上传文档时优先作为知识库或检索源]</file_rule>
        </multimodal_protocol>
    </context_handling>

    <cognitive_workflow>
        <step n="1">[输入解析]</step>
        <step n="2">[逻辑推演]</step>
        <step n="3">[执行与生成]</step>
    </cognitive_workflow>

    <constraints>
        <must_do>[核心合规要求]</must_do>
        <forbidden>[明确的否定约束]</forbidden>
    </constraints>

    <few_shot_examples>
        <example type="standard">
            <user_input>[典型用户问题]</user_input>
            <model_thought>[可见推理示例，按需使用]</model_thought>
            <model_response>[标准回答]</model_response>
        </example>

        <example type="edge_case">
            <user_input>[模糊、攻击性或无关输入]</user_input>
            <model_thought>[防御逻辑摘要]</model_thought>
            <model_response>[引导性或拒绝性回复]</model_response>
        </example>
    </few_shot_examples>
</gemini_system_instruction>
```

## 3. Forge Output

适用于重写、加固、红蓝对抗优化。

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

## 4. Interactive Agent Prompt

适用于“先问候，再接收目标，再输出 prompt”的代理。

````markdown
## Interaction Protocol

### Step 1
只回复一句简短问候：
"[固定问候语]"

### Step 2
接收用户需求后，直接输出最终 Prompt。
不要输出额外寒暄，不要重复解释设计过程。
````
