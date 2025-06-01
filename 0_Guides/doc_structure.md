## 文档结构说明

睿图 AI 软件生产系统文档库（初版）

本文档库旨在支持“AI Agent + Human 协同软件生产线”的建设与运营，作为睿图AI流程再造项目的知识资产中枢。

⸻

📁 0_全局指南（0_Guides）

doc_structure.md

说明整个文档库结构、内容用途与维护方法。

agent_integration_guide.md

规范如何将AI Agent嵌入具体业务流程，包括任务分配、输入输出定义、与人类的交互点等。

⸻

📁 1_流程结构说明（1_FlowSpecs）

01_RequirementFlow.md

描述需求阶段的流程结构、关键任务、参与角色、所需文档与Agent协作点。

02_DesignFlow.md

描述系统设计阶段的任务拆解、输入输出文档、Agent应用点等。

（其它阶段略，可后续补充）

⸻

📁 2_任务模板（2_TaskTemplates）

📂 requirement_analysis

#### prompt_template.md

```
你是一名资深软件产品分析师，请根据以下自然语言需求说明，完成结构化的需求要素提取，包括：
- 业务目标（business_goal）
- 涉及模块（related_modules）
- 涉及数据字段（key_fields）
- 风险点（risk_points）

请输出为标准JSON结构。
```

#### io_schema.json

```json
{
  "business_goal": "string",
  "related_modules": ["string"],
  "key_fields": ["string"],
  "risk_points": ["string"]
}
```

sample_input_output.md

**输入（客户原始需求）：**

> “我们希望预约模块支持医生端查看已预约的患者信息，并能导出为Excel，方便线下处理。”

**AI输出：**

```json
{
  "business_goal": "提升医生端预约管理效率",
  "related_modules": ["预约模块", "导出功能"],
  "key_fields": ["患者姓名", "预约时间", "医生姓名"],
  "risk_points": ["权限控制不当可能导致隐私泄露"]
}
```

⸻

📁 3_知识资产（3_KnowledgeAssets）

📂 prompt_samples

存储经过实测有效的提示词模版（如：代码审查、接口生成、会议总结等）

📂 design_patterns

常见设计模式库，供设计Agent参考，如MVC拆解、接口聚合、模块职责划分等

📂 issue_faqs

项目中常见问题与解决方案，逐步沉淀成AI可用知识图谱

⸻

📁 4_交互日志与反馈（4_WorkingLogs）

📂 2025-05-28_requirement_session_001
	•	raw_input.md：原始需求说明
	•	ai_output.md：AI解析与初步生成的任务草案
	•	human_feedback.md：人工修改建议与确认结果
	•	patch_revision.md：AI根据反馈进行修正的记录

（将成为未来训练Agent的重要数据）

⸻

📁 5_审查标准（5_ReviewStandards）

requirement_review.md

人类如何评估AI生成的需求分析结果，包括结构、合理性、可追溯性等维度

design_review.md

设计文档的审查要点与评分标准

code_quality_checklist.md

面向AI或人工生成代码的质量审查清单（风格、性能、安全性等）

⸻

📁 6_工具集成与脚本（6_Toolkits）

vscode_plugin_setup.md

如何将VSCode插件与本系统联通，实现Agent触发与审查同步

ai_agent_debug_script.py

示例脚本：用于调试Agent任务执行逻辑与输出

⸻

📌 使用说明
	•	所有文档建议以Markdown维护，便于版本控制与协作管理。
	•	文档应与实际Agent开发、部署、验证过程并行建设，不做“事后补填”。
	•	该库将作为睿图未来“AI增强型软件运营系统”的知识核心，持续迭代。

⸻
