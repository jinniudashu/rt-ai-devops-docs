## Agent 集成指南

描述如何接入 Agent 到流程中。

### Agent Prompt 策略建议

1️⃣事无巨细
把 Agent 当成一个新来公司的同事，提供详细且长的提示，明确角色、任务、输出和限制条件

2️⃣ 分配一个角色、描述任务
比如，You are xxx，an expert AI assistant ... with vast knowledge ...

3️⃣ 结构化 Prompt
使用 XML 来结构化 Prompt，也可以在 XML 标签中混用 Markdown.

4️⃣ Meta-Prompting
让LLM帮助你编写或优化提示。把你当前的提示、好的/坏的输出示例给它，让它“改进这个提示”或进行点评

5️⃣ 提供示例
有时候，示例比一连串的说明更有效

6️⃣ Prompt Folding & Dynamic Generation

Prompt Folding：指的是将一个复杂的任务拆分成多个更小、更具体的子任务，然后为每个子任务动态生成专门的提示（prompts）。
- Dynamic Generation：强调这些子提示不是预先写死的，而是根据实际输入、上下文或用户需求，实时生成。

根据不同的输入或任务，自动生成更细致、专用的子提示（sub-prompts），而不是依赖一套静态的通用提示。这样做的好处是，AI 代理可以更好地理解任务意图，输出更符合预期的结果。

7️⃣ 实现“逃生舱”机制
如果 LLM 在不知道答案或信息不足时要明确说明，不要对用户胡编乱造

比如：If you do not have enough information to make a determination, say 'I don't know' and ask for clarification.

8️⃣ 跟踪 LLMs 的思考过程

在输出中包含一个一小节，用来解释其推理过程或为何做出某些决策。这样做的好处，可以为调试和迭代提供宝贵的信息。

9️⃣ 关注 LLMOps

LLMOps 是关注 LLMs 在实际应用中的部署、监控、维护和优化工程。一个能评估 Prompt 优劣的系统才是最有价值的护城河。

🔟 因材使用

不同的 LLM 有不同的个性和优缺点，在不同环节可以兼顾质量和成本选择不同的类型。还可以通过对强大的模型进行复杂的 Meta-Prompting，然后蒸馏（Distill）给小模型使用
