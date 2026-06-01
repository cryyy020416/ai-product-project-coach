# Project Patterns

Match only the 1-2 most relevant patterns. Templates provide structure, not copy. Always adapt to the user's real scenario and boundary.

Before using any pattern, apply `harness-project-logic.md`: every pattern should explain function point -> AI technical solution -> eval set/rubric -> eval results -> badcase-driven iteration.

## Pattern A: RAG / Knowledge Base Project

Use for: internal knowledge assistant, FAQ, sales/support answer bot, document Q&A, policy assistant, operations knowledge base.

Low-quality writing:

- "整理文档并搭建 AI 知识库"
- "接入 AI 助手，提升效率"

Key decisions:

- QA pairs vs text knowledge base.
- Chunking by topic, workflow node, problem type, or document structure.
- Metadata: business line, intent, risk level, role, source.
- When to answer, clarify, refuse, or transfer to human.
- Eval purpose: online-like distribution, internal iteration, or boundary pressure test.

Product chain:

```text
user query
-> intent classification / query rewrite / slot filling
-> context selection with metadata filter
-> retrieval / rerank / topK evidence return
-> answer generation with source citation
-> verification gate: groundedness, completeness, fallback
-> clarification/refusal/manual fallback
-> tracing + badcase loop
```

Architecture choice:

- Fixed Workflow is usually enough for stable Q&A.
- Add Sub-agent only for deep research over many documents.
- Add Agent Team when source discovery, evidence extraction, critique, and report generation need separate roles.

Metrics:

- direct resolution rate
- manual fallback rate
- average dialogue rounds
- knowledge coverage
- badcase ratio
- answer accuracy/completeness/actionability

Resume skeleton:

```text
针对【用户】在【场景】中重复咨询多、知识分散的问题，设计 RAG 智能答疑方案。
- 设计「意图识别 / query rewrite / metadata filter / RAG 检索 / 引用生成 / 兜底校验」固定 Workflow，对比 QA 对与文本库方案，选择【方案】以平衡覆盖率和可控性。
- 按【主题/流程/问题类型】重构知识文本，并通过 topK 召回率、引用命中率和答案完整性评估 chunk、标签和 query rewrite 策略。
- 构建【XX】条评测集，按内容正确性、证据支撑、表达可用性、风险兜底四层 MECE rubric 评估回答质量。
- 基于【漏召回/证据不支撑/回答冗余】等 badcase 调整 metadata、prompt 约束和人工转接策略，并用同一评测集复测。
```

## Pattern B: Intelligent Customer Service Evaluation

Use for: customer service bots, answer quality evaluation, annotation/QA work, activity-rule consultation, support tickets.

Low-quality writing:

- "参与智能客服标注打分"
- "优化客服机器人准确率"

Key decisions:

- Define user intents from business chain and real queries.
- Separate online user chain from internal evaluation chain.
- Convert user phenomena into chain diagnosis.
- Build scoring at two layers: business chain diagnosis and end-to-end experience.

Online chain:

```text
query -> platform/business intent -> slot check -> query rewrite -> RAG/rules/API -> generated or assembled reply -> verification -> manual fallback
```

Internal evaluation chain:

```text
samples -> standard intent/answer -> model output -> scoring -> badcase attribution -> offline retest
```

Metrics:

- intent hit rate
- answer quality score
- repeated question rate
- manual transfer rate
- first-contact resolution
- average dialogue rounds

Badcase examples:

- multi-intent query answered only partially
- correct retrieval but answer too shallow
- missing slot causes wrong answer
- should transfer to human but keeps answering

Iteration requirement:

- If intent score is low, modify intent taxonomy, examples, or routing prompt.
- If retrieval score is low, modify metadata, chunking, query rewrite, or topK/rerank.
- If end-to-end score is low while retrieval is correct, modify answer template, clarification strategy, or fallback rules.

## Pattern C: Prompt Optimization / AI Diagnosis Tool

Use for: interview review, writing diagnosis, scoring feedback, report generation, AI coach, structured analysis from long text.

Low-quality writing:

- "优化 prompt，提升体验"
- "支持角色识别、问答切分、报告生成"

Key decisions:

- Define the unique product value: what does AI find that users cannot easily find themselves?
- Single long prompt vs multi-step Prompt chain.
- Require source quotes/evidence to reduce hallucination.
- Judge quality by accuracy, actionability, consistency, completeness, and job fit.
- Manage long-context decay and token cost.

Metrics:

- diagnosis accuracy
- suggestion actionability
- consistency across runs
- hallucination rate
- long-text omission rate
- token cost with quality guardrail

Resume skeleton:

```text
针对【用户】在【任务】中标准缺失、难以自查的问题，设计 AI 诊断工具。
- 将任务拆为【解析 -> 切分 -> 诊断 -> 建议 -> Judge 复核】的 Prompt chain，对比单 prompt 与多步链路方案。
- 针对幻觉、长文衰减、点评脱靶等 badcase，加入原文引用约束、context compression 和 LLM-as-a-judge 自动化评估。
- 构建【XX】条评测样本，按分析准确性、建议可执行性、证据引用率和一致性评估输出，并据低分维度迭代 prompt 和上下文选择。
```

## Pattern D: Data Dashboard / Anomaly Detection

Use for: tables, reports, dashboards, operations tracking, inventory/order/status monitoring.

Low-quality writing:

- "搭建多维数据看板"
- "实现实时化透明化追踪"

Key decisions:

- Which tables, fields, and nodes repeat or conflict?
- What unique ID links objects?
- What can be auto-synced vs manually confirmed?
- What anomaly should trigger alerts?
- Is the dashboard passive viewing or active warning?

Metrics:

- data entry time
- reporting time
- data consistency rate
- duplicate record rate
- anomaly discovery delay
- manual confirmation rate

AI angle:

- AI can classify anomalies, generate summaries, detect inconsistent text, draft explanations, or route exceptions.
- For stronger AI product depth, write a Workflow: table normalization -> anomaly detection/rule+LLM classification -> explanation generation -> human confirmation -> alert/report.
- Evaluate by anomaly recall, false positive rate, explanation usefulness, and manual confirmation rate.

## Pattern E: SOP / Workflow AI Automation

Use for: SOPs, approval flows, delivery process, operations handoff, repeated judgment, internal workflow.

Low-quality writing:

- "编写 5000 字 SOP"
- "规范流程，提高效率"

Key decisions:

- Process nodes, input, output, owner, exception.
- Which decisions are rule-based or AI-assisted?
- Which steps require human approval?
- Can the SOP become a Skill / Workflow instruction?

Metrics:

- onboarding time
- operation error rate
- manual intervention rate
- process completion time
- SOP coverage

Resume skeleton:

```text
针对【流程】权责模糊、异常处理依赖人工的问题，拆解关键节点并探索 AI 辅助执行。
- 明确每个节点的输入、输出、责任人和异常处理规则，设计可由 AI 执行的 Workflow/Skill 指令骨架。
- 识别【重复判断】环节，采用 Prompt chain 生成草稿、检查材料完整性，并在人审节点前设置 guardrails。
- 构建【XX】条流程 case，按任务完成率、人工介入率、异常识别准确率和操作差错率评估流程 AI 化效果。
```

## Pattern F: Model Prediction / Recommendation / Risk Project

Use for: prediction, classification, recommendation, risk screening, scientific/finance/health school projects.

Low-quality writing:

- "基于 XX 模型，AUROC 达到 XX"
- "使用多模态数据和消融实验"

Product translation:

- What real decision does the model support?
- Which error is more costly: false positive or false negative?
- Why this metric instead of accuracy?
- How does the user trust the prediction?
- What badcases reveal model boundary?

Metrics:

- AUROC for imbalanced data
- recall when missing good candidates is costly
- precision when wrong candidates waste resources
- Top-K hit rate
- conversion to downstream validation
- explainability usefulness

## Pattern G: Agent / Skill Workflow

Use for: multi-step AI workflow, tool use, skill routing, interview training agent, research agent, personal assistant, automation harness.

Low-quality writing:

- "做了一个 Agent"
- "支持多 Agent 协作"
- "调用工具完成任务"

Key decisions:

- Task specification: how vague user goals become executable tasks.
- Context selection: what information is loaded for each step.
- Tool/Agent routing: which worker handles which subtask.
- State management: what persists across steps.
- Guardrails: what requires human confirmation.
- Observability: can each step be traced?
- Verification: how outputs are judged.
- Failure attribution: can bad output be traced to context, tool, prompt, or criterion?

Product chain:

```text
task definition -> context selection -> tool/agent routing -> intermediate state -> output verification -> failure attribution -> human confirmation -> eval feedback
```

Metrics:

- task completion rate
- evidence citation rate
- routing accuracy
- manual correction rate
- hallucination rate
- repeated-run consistency
- before/after improvement on the same task

Good scenario examples:

- AI interview training Agent
- AI product intelligence/research Agent
- multi-skill routing sandbox
- consulting outline generation copilot

Resume skeleton:

```text
针对【复杂任务】中单次对话难以稳定完成、过程不可追溯的问题，设计 Agent Harness 工作流。
- 拆解 Task Specification、Context Selection、Tool/Agent Routing、State Management、Verification 等节点，选择【固定 Workflow / Sub-agent / Agent Team】架构承接任务。
- 设计【Agent A / Agent B / Critic / Judge】分工，结合 tracing 记录每步输入、检索材料、输出和评分，定位失败来自路由、上下文、工具还是生成。
- 构建覆盖常规任务、边界 case、多轮任务和高风险输出的评测集，按任务完成率、路由准确率、证据引用率、人工接管率和幻觉率迭代 Agent 链路。
```
