# AI 产品项目教练

一个面向 AI 产品实习求职、转行和个人项目包装的 AI assistant skill。

它可以根据用户上传的简历、已有项目、个人经历或具体困惑，先判断用户真正想解决的问题，再给出对应的辅导：AI 项目方向、项目迭代方案、简历诊断、AI 产品经理能力答疑，或一份完整的 AI 项目落地方案。

## 支持的场景

- 生成完整的 AI 产品项目落地方案
- 给没有 AI 项目的用户寻找不烂大街的项目方向
- 优化个人 demo、网页项目、RAG 工具、Agent 工作流或 Prompt 工具
- 诊断 AI 产品岗简历里的问题
- 回答 AI 产品经理工作内容、技术理解深度、RAG、Agent、Prompt Chain、Evals 等问题
- 准备项目面试追问、badcase 讲解和边界防御

## Skill 结构

```text
ai-product-project-coach/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── anonymized-cases.md
    ├── assessment-framework.md
    ├── harness-project-logic.md
    ├── intent-routing.md
    ├── judgment-principles.md
    ├── output-template.md
    └── project-patterns.md
```

## 设计原则

- 从用户真实经历、真实材料和可补做证据出发。
- 不编造上线结果、内部权限、业务指标、算法归属或商业结果。
- 强调完整项目闭环：功能点、AI 技术方案、评测集、MECE 评分标准、badcase 归因、数据驱动迭代。
- 优先输出具体的 AI 产品架构，而不是泛泛的功能列表。
- 输出风格保持直接、实用、能经得起面试追问。

## 许可证

MIT
