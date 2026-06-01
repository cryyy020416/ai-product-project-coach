# Harness Project Logic

Use this reference whenever drafting the core project chain, evaluation system, badcase iteration, and resume bullets. It encodes the required depth for deliverables.

## Mandatory bottom logic

Every project must explain this loop near the beginning:

```text
function point / user task
-> AI technical solution
-> eval set + MECE scoring standard
-> run evals and locate chain problems
-> modify the chain
-> retest and show which score improves
```

Do not jump from "I want to solve this scenario" directly to "I built these features". AI product value comes from proving that a specific AI method improves a specific task under a measurable standard.

## Start from a function point

Frame the project around a user task, not around a model buzzword.

Examples:

| Function point | Weak phrasing | Strong AI product phrasing |
| --- | --- | --- |
| Answer a business question | 做一个知识库问答 | Use RAG + intent/query rewrite to retrieve grounded evidence and answer with fallback rules |
| Generate a research brief | AI 写研报 | Use Agent Team to separate source discovery, evidence extraction, contradiction check, and report drafting |
| Diagnose interview answers | AI 面试助手 | Use prompt chain + evaluator agent to score evidence strength, structure, job fit, and hallucination risk |
| Monitor product updates | AI 竞品情报 | Use workflow + source-trust scoring + signal agent to detect high-value changes |
| Route skills/tools | 多 Agent 系统 | Use routing evals to compare all-broadcast vs classification-recall vs sub-agent decomposition |

## Choose an execution architecture

Select the simplest architecture that fits the task. Explain why it is enough and where it may fail.

### 1. Fixed Workflow

Use when the task path is stable and can be decomposed in advance.

```text
input -> classify/parse -> retrieve/transform -> generate -> verify -> output
```

Good for:

- RAG Q&A
- report generation with fixed sections
- resume/JD diagnosis
- data table anomaly review
- SOP-to-workflow conversion

What to mention:

- task specification
- prompt chain or node sequence
- query rewrite / slot filling if needed
- context selection
- verification gate
- tracing of each node's input/output

### 2. Sub-agent

Use when the main model must dynamically delegate a complex subtask, especially deep research, coding, long analysis, or open-ended exploration.

```text
main agent defines task boundary
-> creates sub-agent with local objective, context, tools, and stop condition
-> sub-agent returns traceable result
-> main agent merges, verifies, or rejects
```

Key point:

- A sub-agent is not "many roles". It is dynamically created when the main agent identifies that a subtask exceeds the main loop's efficient context or tool boundary.
- Explain the boundary: what is delegated, what remains under the main agent, and how the result is verified.

Good for:

- deep research on multiple companies
- debugging or coding a prototype
- searching evidence for competing hypotheses
- long-document comparison

### 3. Agent Team

Use when the task has stable specialist roles that should run independently and then be merged.

```text
planner/coordinator
-> specialist agents run in parallel or sequence
-> critic/evaluator checks outputs
-> final synthesizer writes answer
```

Good for:

- research agent with Source Agent, Extractor Agent, Evidence Agent, Critic Agent, Report Agent
- interview training with JD Agent, Interviewer Agent, Evaluator Agent, Coach Agent
- product intelligence with Source Agent, Comparator Agent, Signal Agent, Reviewer

What to mention:

- each agent's input/output
- why roles are separated
- whether agents see the same context
- how conflicts are resolved
- cost/latency tradeoff

## Harness concepts to include when relevant

| Concept | Meaning | Project expression |
| --- | --- | --- |
| Task Specification | Convert vague user goals into executable tasks | Parse query/JD/report goal into structured fields |
| Context Selection | Avoid dumping all materials into the model | Select relevant docs, history, memory, eval cases |
| Tool Access | Model uses retrieval, tables, search, judge, simulator, generator | Name the tools or workflow nodes |
| State Management | Multi-step task tracks current stage and intermediate outputs | Store task state, score history, badcase type, user profile |
| Guardrails / Permissions | High-risk output needs boundaries | Human confirmation, refusal, sensitive-source checks |
| Observability / Tracing | Each step can be inspected | Log inputs, retrieved docs, prompts, outputs, scores |
| Verification / Evals | Output must pass standards | Eval set, rubric, LLM-as-a-judge, manual sampling |
| Failure Attribution | Bad result can be mapped to a chain node | Retrieval, prompt, context, tool, judge, generator |
| Feedback Loop | Data changes the next version | Modify prompt, metadata, routing, compression, rubric |

## Evaluation section requirements

Write evaluation in this order.

### 1. Purpose and process

Explain what the evaluation is trying to prove.

Common purposes:

- simulate online distribution
- support internal iteration
- pressure-test boundary cases
- compare two technical solutions
- identify the weakest chain node

Then show the process:

```text
prepare eval cases -> run baseline chain -> score by rubric -> locate low-score dimensions -> inspect traces -> modify chain -> rerun same eval set
```

### 2. Eval set sources and tradeoffs

Compare sources instead of listing them.

| Source | Advantage | Risk | Use |
| --- | --- | --- | --- |
| Real user/query samples | closest to actual distribution | privacy, skewed toward common cases | online-like evaluation |
| Historical cases/materials | grounded and scenario-specific | may be stale or biased | internal iteration |
| Manually constructed edge cases | covers rare/high-risk failures | may be artificial | boundary pressure test |
| AI-generated cases | cheap and broad | may be unrealistic | first draft, then human filter |
| Public materials | easy to show in demo | less tied to user's real role | portfolio-safe prototype |

### 3. MECE scoring standard

Rubrics must be mutually exclusive and collectively exhaustive. Do not dump random metrics.

Example for answer/research output:

```text
Output Quality
├── Content correctness: facts match source, no hallucinated data
├── Evidence grounding: every key claim has supporting source
├── Reasoning usefulness: conclusion, causal chain, and implication are clear
└── Risk handling: uncertainty, conflict, and human-review cases are marked
```

Example for Agent workflow:

```text
Workflow Reliability
├── Task routing: correct agent/tool chosen
├── Context control: required information included, irrelevant noise excluded
├── Execution completion: task reaches usable output
└── Verification: output passes rubric or is rejected for revision
```

Example for RAG/customer service:

```text
Answer Experience
├── Content: correct, complete, actionable
├── Interaction: concise, clarifies missing slots, avoids extra rounds
└── Safety/fallback: refuses or transfers when needed
```

### 4. Evaluation target and automation

State whether the target is:

- chain node quality, such as retrieval, intent, generation, routing
- end-to-end user outcome
- comparison between solution A and B
- stability across repeated runs

Then state execution:

- manual scoring for first 20-50 cases
- LLM-as-a-judge for batch scoring
- human sampling to calibrate judge prompt
- tracing table storing case_id, inputs, retrieved context, output, score, badcase type, and revision note

## Badcase optimization structure

Every badcase must follow this structure:

```text
Low-score dimension:
Observed case:
Trace diagnosis:
Chain node to change:
Modification:
Retest metric:
Expected score movement:
```

Start with shallow problems first, then deeper problems.

### Shallow badcase

Usually easy to improve:

- wrong or missing metadata
- prompt instruction too vague
- output format unstable
- no source citation required
- missing slot clarification
- eval label too broad

### Deep badcase

Requires architecture or workflow change:

- long-context decay
- agent routing failure
- answer supports only one side of a hypothesis
- judge prompt rewards fluency over correctness
- evidence is relevant but does not support the claim
- cost grows because all agents are called every time

## Resume bullet requirement

Resume bullets must not sound like generic feature design. Each bullet should include:

- AI technical solution: Prompt chain, RAG, query rewrite, context compression, Agent Team, Sub-agent, LLM-as-a-judge, tracing, automated evals, tool routing, guardrails.
- Product decision: why this solution was chosen.
- Eval/iteration: which metric or badcase it improved.
- Boundary: avoid claiming launch or real business results without evidence.

Weak:

```text
设计 AI 投研助手，支持资料检索、报告生成和风险提示。
```

Stronger:

```text
设计 Source Agent、Evidence Agent、Critic Agent、Report Agent 的 Agent Team 工作流，将投研任务拆为信息源分级、证据抽取、反证检查和报告生成，并通过引用命中率、事实一致率和反证覆盖率评估自动化输出质量。
```
