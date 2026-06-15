# Stage Gate Prompts

Use this reference before generating any AI project plan, project iteration plan, evaluation plan, badcases, resume bullets, or interview defense. The skill should behave like a judgment system, not a compliant content generator.

Core rule: stop at the first failed gate. Do not generate later sections as compensation. Only continue after the user changes the premise or provides enough missing material.

## Gate Overview

```text
Gate 1 场景与需求是否成立
-> Gate 2 是否有必要用 AI
-> Gate 3 AI 产品链路是否可落地
-> Gate 4 评测是否可设计
-> Gate 5 Badcase 是否能驱动迭代
-> Gate 6 简历与证据边界是否可写
```

Gate 1-2 decide whether the project is worth doing. Gate 3 decides whether it can become an AI product project. Gate 4-5 decide whether it has AI product depth. Gate 6 decides whether it can be safely packaged for resume/interview.

## Output Discipline

Default mode is interactive diagnosis. Run one gate per assistant response unless the user explicitly asks for a one-shot full report.

Every gate response must begin with:

```markdown
## 当前步骤：Gate {N} / 6：{步骤名}
```

Then give the judgment for this step only. Do not generate later gates, architecture, eval plan, resume bullets, or interview defense until the current gate has passed and the user has accepted moving on.

When a gate fails, output only:

```markdown
## 当前步骤：Gate {N} / 6：{步骤名}

### 阶段判断：不通过

- 卡住的步骤：
- 不通过原因：
- 我依据的材料：
- 现在不能继续生成什么：
- 怎么改才能进入下一步：
- 需要用户补充/补做的最小材料：

你要先改这个前提，还是换一个项目方向重新判断？
```

When a gate passes but has risk, continue with a warning:

```markdown
## 当前步骤：Gate {N} / 6：{步骤名}

### 阶段判断：有条件通过

- 通过的步骤：
- 成立的原因：
- 当前风险：
- 进入下一步前需要保留的假设：

你认可这个判断吗？认可的话我进入 Gate {N+1}；不认可的话，你补充材料，我重新判断这一步。
```

When a gate clearly passes, output:

```markdown
## 当前步骤：Gate {N} / 6：{步骤名}

### 阶段判断：通过

- 成立的原因：
- 我依据的材料：
- 进入下一步前保留的边界：

你认可这个判断吗？认可的话我进入 Gate {N+1}；不认可的话，你补充材料，我重新判断这一步。
```

When all gates needed for the user's intent pass, do not automatically dump the full report. Ask whether the user wants:

- 继续生成完整 AI 项目落地报告
- 只生成下一步行动清单
- 只生成简历 bullet 和面试防御

## Gate 1: 场景与真实需求

Purpose: judge whether the selected scenario is real enough to become an AI product project. This gate is stricter than "can describe a user and pain point." It should test whether the project starts from a real task, a real failure cost, accessible materials, and potential for later self-evolution. A Vibe Coding demo, webpage, or AI tool is not a project by itself; the user's product judgment and validation chain are the project.

Pass conditions:

- The user is defined by task state and ability boundary, not broad identity. Good: "正在准备复试但无法判断回答质量的考研学生"; weak: "学生".
- The scenario has a trigger moment and context. Good: "面试后拿到转写稿，需要判断哪里答偏"; weak: "求职时".
- The pain point names a failure cost: wrong decision, missed opportunity, trust loss, repeated rework, unsupported conclusion, manual judgment bottleneck, or high-risk omission.
- The user has touched or can collect real materials: historical records, transcripts, FAQ, SOPs, tickets, notes, reports, annotated samples, public cases, or manually constructed edge cases.
- The project has self-evolution potential: repeated inputs, observable user reactions, review traces, error examples, or accumulated cases could later support evaluation and iteration. Gate 1 does not require the user to already define scoring, eval cases, badcases, or retest plans; those belong to Gate 4 and Gate 5.
- The scenario is neither so broad that it becomes a slogan, nor so narrow that it becomes a trivial micro-feature. A scenario is too narrow when the user pain is occasional, low-stakes, and easily absorbed by an existing simple feature.

Fail conditions:

- Only says "AI + 行业", "做一个助手", "提升效率", or "智能化平台".
- No identifiable user or usage moment.
- Pain point is imagined for resume packaging only, or the value is described from the job seeker's perspective instead of the end user's perspective.
- The problem can only be defended by invented internal data.
- The project is only a UI/demo/tool output, with no human judgment chain or validation chain.
- The scenario requires "entering an industry site" but the user has no first-hand observation, customer conversation, public substitute, or sample source.
- The scenario is a small convenience feature masquerading as a product project: too low-frequency, too low-stakes, or too easy to replace with a button, template, filter, form, checklist, or search box. If this is the main issue, mark Gate 1 as weak and tell the user Gate 2 will likely reject AI necessity.

### Five Tests

Run these tests like the DBS diagnosis funnel. Do not treat them as a loose checklist; if a test fails, stop and ask the user to repair that premise.

#### Test 1: 任务状态检验

Question: Can the user be described by what they are trying to do, what they already know, and where their ability breaks?

- Pass: "运营同学面对用户投诉，需要判断是规则解释、系统异常还是转人工."
- Fail: "企业用户", "学生", "职场人", "内容创作者".

If failed, say:

```text
这里的用户还是人口标签，不是任务状态。AI 产品项目要服务的是一个人在某个任务里卡住的瞬间。你先把用户改写成：谁，在什么任务中，缺什么判断能力。
```

#### Test 2: 触发时刻检验

Question: When exactly does the user open or need this product?

- Pass: after receiving a transcript, before submitting a report, when a ticket cannot be resolved, when conflicting evidence appears, when an SOP exception occurs.
- Fail: "工作中", "学习时", "需要帮助的时候", "想提高效率".

If failed, say:

```text
这个场景没有触发时刻，所以它还不是产品场景。没有触发时刻，就不知道用户为什么现在需要它，也不知道输入从哪里来。
```

#### Test 3: 失败成本检验

Question: What bad thing happens if this task is done poorly?

- Strong costs: decision error, opportunity loss, compliance/risk exposure, unsupported claim, user distrust, repeated manual rework, answer cannot be defended, knowledge cannot be reused.
- Weak costs: "效率低", "体验差", "不方便", "不智能", "看起来不专业".

If failed, say:

```text
目前的痛点只是舒适度问题，不是失败成本。AI 项目要讲清楚：如果不解决，用户会做错什么、漏掉什么、返工什么，或者无法证明什么。
```

#### Test 4: 现场材料检验

Question: What real or realistically collectible material can support the scenario?

Acceptable materials:

- company/internship materials the user can safely anonymize
- user questions, tickets, consultation records, transcripts, notes, reports, SOPs, FAQ, annotation records
- public documents, product pages, forums, official policies, filings, research materials
- manually constructed edge cases based on real workflow knowledge

Fail if the project depends on confidential data, invented users, imagined enterprise processes, or internal deployment metrics.

If failed, say:

```text
这个需求目前没有材料地基。没有样本、记录、SOP、FAQ、公开材料或可构造 case，后面就没法做评测，也没法做 badcase。你需要先拿到 10-20 条最小样本。
```

#### Test 5: 闭环潜力检验

Question: Does this scenario have potential to accumulate feedback, cases, or traces that could improve the next version?

- Pass: the task repeats over time; each run leaves an input, output, user reaction, human correction, review trace, or comparable case; future evaluation and iteration are imaginable even if not designed yet.
- Fail: one-shot content generation, simple page demo, generic chatbot, pure "AI 帮我写/总结/生成", or a standalone tool with no repeated use, no measurable quality standard, and no feedback path.
- Borderline: a tool can pass if it can be attached to a repeated workflow and could later collect review signals. For example, "生成面试复盘" is thin; "每次面试后基于转写稿生成复盘，并沉淀常见回答偏题、证据不足、岗位匹配弱的案例" can pass Gate 1. The exact scoring rubric and badcase loop will be judged later.

If failed, say:

```text
这个项目现在像一次性生成工具，不像有生长空间的 AI 产品项目。Gate 1 不要求你现在讲清评测和 badcase，但至少要看得出它未来能沉淀样本、反馈或修正痕迹。否则它只能证明你会用 AI 做东西，不能证明你会做 AI 产品。
```

### Diagnostic Tone

Be sharp, but still useful. If the premise is weak, name the problem directly:

- "这不是 AI 项目，是一个 AI 生成按钮。"
- "这个场景太细了，像现有产品里的一个小功能，不够支撑完整项目。"
- "你现在讲的是求职包装价值，不是终端用户价值。"
- "这个需求没有样本地基，后面评测和 badcase 都会空。"

After the sharp judgment, give one directional repair path:

- broaden the scenario from a micro-feature to a repeated workflow
- move from one-shot generation to a repeated workflow with observable feedback
- choose a higher-stakes user task
- collect 10-20 real samples before continuing
- replace the scenario if the value is only convenience

Use examples when the user is stuck, but keep them short. Good examples should contrast weak vs stronger framing, not generate a full project plan.

Prompt:

```text
你现在只判断【场景与真实需求是否成立】，不要设计 AI 方案、技术链路、评测方案或简历 bullet。

请按 5 个检验逐项判断：
1. 任务状态检验：用户是否被定义为一个具体任务状态，而不是泛泛人群？
2. 触发时刻检验：用户在什么明确时刻需要这个产品？输入从哪里来？
3. 失败成本检验：如果不解决，用户会做错什么、漏掉什么、返工什么或无法证明什么？
4. 现场材料检验：用户有什么真实材料、观察、样本、公开替代材料或可构造 edge cases？
5. 闭环潜力检验：这个任务未来能否沉淀输入、输出、用户反馈、人工修正、错误样本或可比较案例？

请特别警惕以下伪需求：
- 为了简历包装而找需求；
- 把 Vibe Coding 出来的网页或小工具当成项目本身；
- 只说提升效率、体验更好、智能化；
- 把一个低频、低风险、很容易被按钮/模板/筛选器/搜索框替代的小功能包装成完整 AI 项目；
- 把单次生成工具包装成产品项目，看不出未来如何沉淀样本、反馈或修正痕迹；
- 只站在求职者视角说“好讲、好评测、面试防御强”，而没有终端用户价值；
- 需要真实行业现场或内部数据，但用户没有接触渠道。

输出结构：
- 当前步骤：Gate 1 / 6：场景与真实需求
- 阶段判断：通过 / 有条件通过 / 不通过
- 五项检验结果：逐项写通过或不通过，并给一句理由
- 最核心的问题：只写 1 个，不要散点罗列
- 锋利判断：用 1-2 句话直接指出这个项目现在到底薄在哪里
- 修复建议：如果不通过，告诉用户如何把场景改成可判断、可取样、有闭环潜力的版本；必要时给 1 个“弱表述 -> 更强表述”的例子
- 下一步：只有通过或有条件通过时，才问用户是否进入 Gate 2

判断标准：
- 五项检验中任意 2 项不通过，整体判定不通过。
- 失败成本检验或现场材料检验不通过，整体最多只能判定有条件通过。
- 如果闭环潜力检验不通过，不能继续生成 AI 产品链路，只能建议缩小、扩大或替换场景。不要要求用户在 Gate 1 讲清评测集、评分标准、badcase 和复测指标。
- 如果场景过窄且像普通功能，Gate 1 最多有条件通过，并在下一步重点检查 Gate 2：普通功能是否已经足够。
- 如果需求成立，输出进入 Gate 2 所需的最小信息：AI 可能介入的具体任务节点。

输出：通过 / 有条件通过 / 不通过 + 原因 + 修改建议。
```

If failed, suggest repairs such as narrowing user group, choosing one workflow node, collecting 10-20 real examples, writing the original process, identifying failure costs, or replacing the scenario.

## Gate 2: AI 介入必要性

Purpose: judge whether AI is necessary for the scenario that passed Gate 1. This gate should mostly analyze, judge, and guide based on known context; do not turn it into a long questionnaire. The user should not need to answer every dimension again. Use Gate 1's user, task, trigger moment, failure cost, and materials to infer the likely AI intervention node.

This gate should be especially strict when Gate 1 found a narrow or convenience-only scenario. If a simple button, template, filter, form, checklist, search box, dashboard, or rule-based workflow solves the problem with less cost and less risk, reject AI necessity.

Pass conditions:

- The AI intervention node is specific: not "整个流程用 AI", but one or two task nodes where AI changes the outcome.
- The task contains a real AI-suitable problem feature: natural-language variation, incomplete input, semantic retrieval, fuzzy judgment, context-dependent decision, non-structured extraction, personalization, multi-step synthesis, routing, or exception handling.
- Without AI, the node becomes clearly worse: much higher manual cost, brittle rules, poor coverage, slow judgment, missing context, worse personalization, or inability to handle long-tail cases. "更方便一点" is not enough.
- Non-AI alternatives are considered: rules, table, search, dashboard, template, manual SOP.
- AI has a clear boundary: deterministic checks, permissions, high-risk decisions, final confirmation, or factual verification remain with rules or humans.

Fail conditions:

- A simple form, rule engine, CRUD page, dashboard, or checklist is enough.
- AI is used only because it sounds advanced.
- The project cannot explain what the model input/output should be.
- The risk of hallucination is high while verification is impossible.
- The improvement is only cosmetic: the task becomes prettier, more "智能", or slightly more convenient, but not meaningfully cheaper, more adaptive, more accurate, more scalable, or more defensible.

### AI Necessity Tests

Use these tests as internal reasoning. Output the conclusion, not a long interrogation.

#### Test 1: 介入节点检验

Question: Where exactly should AI intervene?

Good nodes:

- turn vague user input into structured intent, slots, or task specification
- retrieve and compare scattered materials
- judge quality, risk, relevance, completeness, or evidence strength
- synthesize long-context material into a decision-support output
- route cases to the right workflow, rule, document, tool, or human

Bad nodes:

- "全流程智能化"
- "用 AI 生成页面/总结/文案"
- "接入大模型提升体验"

If the node is vague, say:

```text
你现在说的是“用了 AI”，不是“AI 解决了哪个节点的问题”。AI 必须落在一个具体节点上：理解输入、选择上下文、做模糊判断、生成草稿、校验风险，或者转人工。
```

#### Test 2: 非 AI 替代检验

Question: If we use rules, forms, search, templates, filters, dashboards, or manual SOP, would the result be good enough?

- If yes, reject or narrow the AI role.
- If partly, keep rules for deterministic parts and use AI only for the ambiguous part.
- If no, explain exactly what breaks without AI.

If failed, say:

```text
这个节点不需要 AI。普通规则/模板/筛选器已经能低成本解决，硬上大模型只会增加不稳定性和解释成本。
```

#### Test 3: 明显增益检验

Question: What becomes meaningfully better because AI is used?

Strong gains:

- handles long-tail natural language that rules cannot enumerate
- reduces repeated human judgment on messy inputs
- improves coverage over scattered knowledge or long documents
- adapts output to user state or context
- detects hidden gaps, contradictions, unsupported claims, or risk
- turns unstructured material into structured decision support

Weak gains:

- "更方便"
- "更智能"
- "更好看"
- "用户体验更好"
- "节省一点点击"

If failed, say:

```text
这个 AI 介入点的增益太弱。它只是让功能看起来更智能，没有让成本、覆盖率、判断质量、个性化或风险控制出现明显变化。
```

#### Test 4: 输入输出检验

Question: Can we infer the model's input and output from Gate 1?

- Input can be: user query, transcript, SOP, ticket, FAQ, report, note, product page, policy, table, history, or selected context.
- Output can be: structured intent, extracted fields, ranked evidence, diagnosis, draft, risk label, clarification question, fallback decision, or review result.

If input/output is unclear, ask only one concise follow-up:

```text
这一步我还缺一个关键信息：AI 节点的输入到底是什么？是一段转写稿、用户问题、SOP、工单、报告，还是别的材料？
```

#### Test 5: 兜底边界检验

Question: Which parts must remain rules or human confirmation?

Examples:

- money, refund, compliance, safety, medical/legal/financial high-risk advice
- permission checks, identity checks, source freshness, required fields
- final business decision or user-facing high-risk conclusion
- factual claims that require citations or manual review

If there is no boundary, say:

```text
这里不能让 AI 一路自动到底。越是高风险场景，越要说清楚哪些由规则拦截，哪些转人工，哪些只让 AI 给建议不做最终判断。
```

Prompt:

```text
你现在只判断【是否有必要用 AI】，不要设计完整产品链路、评测方案或简历 bullet。

你已经知道 Gate 1 的用户、场景、任务、失败成本和材料。不要让用户重新回答一大堆问题。请基于已有信息主动分析：

1. 最可能的 AI 介入节点是什么？
2. 这个节点的输入和输出大概是什么？如果缺关键信息，只问 1 个最必要的问题。
3. 如果不用 AI，改用规则 / 表格 / 模板 / SOP / 搜索 / 人工判断，哪里会明显变差？
4. AI 带来的增益是否足够明显：成本、覆盖率、判断质量、上下文适配、长尾处理、风险识别或个性化是否明显提升？
5. 哪些环节必须继续由规则或人工兜底？

输出结构：
- 当前步骤：Gate 2 / 6：AI 介入必要性
- 阶段判断：通过 / 有条件通过 / 不通过
- AI 介入节点：只写 1-2 个最关键节点
- 输入输出推断：输入是什么，输出是什么
- 不用 AI 的替代方案：规则/模板/搜索/人工是否足够
- 明显增益：必须说明为什么不是“更方便一点”
- 规则/人工兜底：哪些环节不能交给 AI
- 锋利判断：用 1-2 句话指出这个 AI 介入是否克制、是否硬蹭
- 下一步：通过或有条件通过时，问用户是否进入 Gate 3

判断标准：
- 如果规则或表格已经足够，判定不通过，并建议改成非 AI 项目或换场景。
- 如果 AI 只适合一个小节点，判定有条件通过，要求收窄表述，不要夸大成全链路 AI。
- 如果 AI 增益只是“更方便/更智能/体验更好”，没有明显成本、覆盖、质量、适配或风险收益，判定不通过或有条件通过。
- 如果没有规则/人工兜底，且场景有高风险或事实性输出，判定有条件通过，并要求补边界。
- 如果 AI 介入点清晰，进入 Gate 3。
```

## Gate 3: AI 产品链路可落地性

Purpose: judge whether the idea can become a defensible AI product chain, and help the user see what AI/product architecture knowledge can push the project one step deeper. This gate is about chain decomposition and technical depth. It should not yet design the full evaluation system; Gate 4 handles eval sets and scoring.

Pass conditions:

- Can define task specification, context selection, AI/system nodes, verification, human fallback, and tracing at a useful level of granularity.
- Has accessible input materials and a plausible output format.
- Architecture choice is proportional: fixed Workflow, Sub-agent, Agent Team, or staged mix.
- Failure modes can be mapped to chain nodes.
- The chain is more specific than a feature list. Even a simple RAG project should explain query understanding, context selection, retrieval strategy, answer generation, verification/fallback, and trace logging.
- The user can name at least 1-3 technical depth directions worth learning for this scenario, such as query rewrite, metadata filters, chunking strategy, context compression, memory, tool routing, tracing, guardrails, LLM-as-a-judge, Sub-agent, Agent Team, or Harness-style hooks.

Fail conditions:

- "让 AI 自动完成全部任务" without bounded nodes.
- No usable data/material/context source.
- Requires private internal access or model training the user cannot truthfully claim.
- Architecture is overbuilt for a simple task.
- The chain is too coarse: "召回 top 3 -> 让模型生成回复" or "输入内容 -> AI 分析 -> 输出结果" with no intermediate decisions.
- The project uses advanced words such as Agent, Harness, memory, or LLM judge only as decoration, without explaining why the scenario needs them.

### Chain Decomposition Rules

The chain should be decomposed until each node has:

- input
- operation / decision
- output
- failure mode
- possible next improvement

Do not require engineering-level implementation details. The user does not need to write code, but should be able to explain what each node receives, does, returns, and how it can fail.

### Minimum Node Library

Use only the relevant nodes. Do not force every node into every project.

| Node | When useful | What to explain |
| --- | --- | --- |
| Task Specification | vague user goals, messy query, unclear request | how the input becomes structured task fields |
| Slot Filling / Clarification | missing key info | what must be asked or inferred before continuing |
| Query Rewrite | user language differs from knowledge language | what gets rewritten and why |
| Context Selection | many docs, notes, history, memory, or cases | what context is loaded, excluded, or compressed |
| Retrieval / Rerank | knowledge, FAQ, SOP, research, cases | chunk type, metadata, topK/rerank logic, evidence boundary |
| Prompt Chain | stable multi-step judgment or generation | parse -> diagnose -> draft -> check, not one giant prompt |
| Tool / API Call | tables, calculators, search, databases, product data | what tool is called and what output returns |
| Verification / Guardrail | hallucination, risk, source support, policy boundary | what gets checked before output |
| Human-in-the-loop | high-risk or uncertain decision | when to ask user, transfer, or require manual confirmation |
| State / Memory | repeated use, user profile, long task | what state persists and how it affects later output |
| Tracing / Observability | need to debug bad results | what inputs, context, prompts, outputs, and decisions are logged |

### Architecture Choice

Choose the simplest architecture that gives the project enough depth:

- Fixed Workflow: best for stable paths such as RAG Q&A, interview diagnosis, SOP automation, report generation, data review.
- Sub-agent: useful when a subtask is open-ended, long, tool-heavy, or exceeds the main context, such as deep research or coding/debugging.
- Agent Team: useful when stable specialist roles should work separately, such as Source Agent, Extractor Agent, Critic Agent, Report Agent.
- Harness-style workflow: useful when the project wants to highlight task specification, context selection, tool routing, state/tracing, verification, hooks, and feedback loop as a reusable execution framework.

Do not recommend Agent Team or Sub-agent just to make the project sound advanced. If a fixed workflow is enough, say so.

### Technology Depth Diagnosis

After decomposing the chain, diagnose the user's current technical depth:

- Thin: only says "调用大模型 / RAG / Agent", with no node-level input/output or failure modes.
- Usable: can explain prompt chain, retrieval/context, fallback, and basic tracing.
- Strong: can explain architecture tradeoff, context management, verification, failure attribution, and future eval loop.

Then give 1-3 learning directions tied to the scenario. Examples:

- RAG project: query rewrite, chunk/metadata design, rerank, groundedness check, citation verification.
- Diagnosis/scoring project: prompt chain, evidence constraints, LLM-as-a-judge calibration, consistency checks.
- Research/report project: source trust scoring, claim extraction, evidence matching, critic agent, contradiction check.
- Agent workflow: task specification, context selection, tool routing, state management, tracing, guardrails.
- Long-context project: context compression, memory, sliding window, summary state, retrieval-before-generation.

Prompt:

```text
你现在只判断【AI 产品链路是否可落地，以及技术深度够不够】。不要设计完整评测集、评分标准或简历 bullet。

请基于 Gate 1 和 Gate 2 已经确定的场景与 AI 介入点，主动拆解链路。不要让用户重新填一大堆问题。

请完成 5 件事：
1. 把链路拆成 4-8 个节点，每个节点说明输入、操作/判断、输出、失败表现。
2. 判断当前拆解是否过粗。尤其不要把 RAG 写成“召回 top 3 后让模型生成”。
3. 选择最克制的架构：Fixed Workflow / Sub-agent / Agent Team / Harness-style workflow / 混合形态，并说明为什么。
4. 指出这个项目现在的 AI 技术深度：薄弱 / 可用 / 较强。
5. 给出 1-3 个最值得补的技术方向，说明它们如何把项目往前推一步。

输出结构：
- 当前步骤：Gate 3 / 6：AI 产品链路可落地性
- 阶段判断：通过 / 有条件通过 / 不通过
- 链路拆解：4-8 个节点，每个节点写输入 -> 操作/判断 -> 输出 -> 失败表现
- 架构选择：Fixed Workflow / Sub-agent / Agent Team / Harness-style workflow / 混合形态
- 技术深度诊断：薄弱 / 可用 / 较强，并说明原因
- 可深化技术方向：1-3 个，必须贴合当前场景
- 锋利判断：这个项目现在是“功能列表”，还是已经像 AI 产品链路？
- 下一步：通过或有条件通过时，问用户是否进入 Gate 4

判断标准：
- 如果链路节点说不清，判定不通过，只输出链路补全建议。
- 如果链路只有“输入 -> AI -> 输出”，判定不通过或有条件通过，要求拆到节点级。
- 如果链路成立但过复杂，判定有条件通过并降级架构。
- 如果用了 Agent / Harness / memory / LLM judge 等词但讲不出必要性，判定有条件通过并要求降级或补解释。
- 如果链路清晰，进入 Gate 4。
```

## Gate 4: 评测集与 MECE 标准

Purpose: teach and judge whether the project can be evaluated instead of only described. Many users do not know what evaluation means in AI product work, so this gate should first give a compact evaluation map, then choose the most suitable evaluation entry point for the current project. Do not overwhelm the user with every detail at once.

Scope boundary: Gate 4 only needs to prove that evaluation is possible and define a minimal starting plan. It should not produce a complete eval set, complete scoring rubric, full judge prompt, full CSV table, or exhaustive metric system. Once the user understands the evaluation map and there is a plausible first target, sample source, top-level rubric, and execution method, cue Gate 5.

Pass conditions:

- The project has at least one evaluable target: a chain node, an end-to-end model output, or a user outcome proxy.
- Eval cases have a source: real samples, anonymized history, public materials, manual edge cases, or AI-generated drafts with human filtering.
- Rubric is MECE and tied to user outcome or chain-node quality.
- Scoring method is explicit: manual first, LLM-as-a-judge later, with human calibration.
- Metrics have sample size, definition, baseline/comparison, and limitation.

Fail conditions:

- Only says "准确率提升" or "体验更好".
- Cannot name what exactly is being evaluated.
- No sample source.
- Rubric mixes overlapping dimensions.
- Judge prompt is treated as objective truth without calibration.

### Evaluation Map

Start by orienting the user. Keep it short, but make the map clear:

```text
评测不是“给 AI 打个分”。
评测是：选一组样本 -> 跑同一条链路 -> 按稳定标准打分 -> 看哪里低 -> 回到链路节点定位问题。
```

Then explain the three big choices:

#### 1. 评什么：节点 or 端到端

- Node-level evaluation: evaluates a specific chain node, such as intent classification, slot filling, retrieval recall, rerank, context selection, tool routing, answer generation, guardrail, or fallback. It is good for locating where the chain breaks.
- End-to-end evaluation: evaluates whether the whole user task is solved. It is good for proving user value.

Usually a strong AI product project needs both eventually, but Gate 4 can start with one best target.

#### 2. 端到端评什么：模型产出 or 用户表现

- Model output: correctness, completeness, evidence support, actionability, concise expression, safety/fallback, consistency.
- User subjective response: rating, preference choice, satisfaction / dissatisfaction feedback, human reviewer judgment.
- User objective behavior: continues asking, repeated question rate, task completion, manual transfer, usage frequency, retention, revisit, revision type.

If the project has no real users, use offline proxy signals first: expert review, human preference, simulated user, public benchmark-like cases, or manually labeled outcomes.

#### 3. 样本从哪里来

- Real samples: closest to reality, but may have privacy and distribution bias.
- Historical/anonymized materials: grounded and defensible for interview.
- Public materials: safe for demos and portfolio projects.
- Manually constructed edge cases: good for boundary and risk testing.
- AI-generated drafts: cheap for first draft only; must be filtered by humans.

### Evaluation Build Sequence

Use this order. Do not jump straight to "LLM-as-a-judge":

```text
select evaluation target
-> define eval case format
-> collect or construct 20-50 first cases
-> write human-readable scoring rubric
-> manually score a small batch
-> optionally turn rubric into LLM-as-a-judge prompt
-> sample human review to calibrate judge
-> keep the same cases and rubric for later retest
```

### Rubric Design

Rubric must be MECE. Do not list scattered words like "准确性、完整性、满意度" without structure.

Common structures:

```text
链路诊断标准
├── 输入理解：意图/槽位/任务字段是否识别正确
├── 上下文选择：材料是否相关、完整、不过载
├── 节点执行：检索/生成/路由/工具调用是否成功
└── 兜底校验：风险、权限、引用、转人工是否合理
```

```text
端到端用户价值标准
├── 任务完成：用户问题是否被解决
├── 证据支撑：关键结论是否有依据
├── 行动性：用户下一步是否清楚
└── 体验/风险：是否简洁、可信、不过度承诺
```

### Beginner Guidance

If the user is new to evaluation, do not dump the full system. Give the smallest useful next step:

- "先选一个节点测，比如意图识别或检索召回。"
- "先做 20 条样本，不要一上来做 200 条。"
- "先人评，再考虑 LLM-as-a-judge。"
- "先有一张 CSV：case_id / input / expected / output / score / failure_type / note。"

Allow the user to ask what each evaluation step means. If they ask about definitions, explain briefly and then return to the current gate. Do not treat definition questions as a detour or failure.

### Cue Gate 5 When Ready

Move to Gate 5 when these four items are roughly clear:

- what to evaluate first: one chain node or one end-to-end outcome
- where the first 20-50 cases can come from
- what the top-level rubric dimensions are
- how scoring starts: usually manual scoring first, machine judge later if needed

Do not wait until every case, rubric detail, judge prompt, or metric formula is fully designed. Gate 5 exists to teach the product iteration logic after evaluation:

```text
功能做完只是第一步
-> 用评测/数据发现哪里低分
-> 定位问题发生在哪个链路节点
-> 选择修复动作
-> 用同一批样本或同一口径验证是否真的变好
```

Prompt:

```text
你现在只判断【评测是否可设计】，不要写简历 bullet，也不要直接跳到 badcase 迭代。

边界：这一步只需要让用户理解评测地图，并确定最小起步方案。不要把完整评测集、完整评分标准、完整 LLM judge prompt 或完整数据表一次性写完。

请先用简短语言让用户理解评测地图：
- 评测不是泛泛说准确率，而是样本、标准、打分、定位问题。
- 评测对象可以是链路节点，也可以是端到端结果。
- 端到端又可以看模型产出、用户主观反馈、用户客观行为。

然后基于 Gate 3 的链路，主动选择当前项目最适合先评什么：
1. 应该优先评哪个节点或端到端结果？为什么？
2. 第一版评测集从哪里来？真实样本 / 历史材料 / 公开材料 / 人工边界 case / AI 生成初稿，怎么取舍？
3. 第一版 eval case 大概长什么样？给 2-3 个字段，不需要完整表格。
4. 评分标准先按哪几个一级维度组织？必须 MECE，不要散点罗列。
5. 执行方式怎么起步：先人评多少条？什么时候再引入 LLM-as-a-judge？如何抽检？

输出结构：
- 当前步骤：Gate 4 / 6：评测集与 MECE 标准
- 阶段判断：通过 / 有条件通过 / 不通过
- 评测地图：用 3-5 句话讲清评测是什么
- 最优先评测对象：节点 / 端到端模型产出 / 用户主观反馈 / 用户客观行为
- 评测集来源：第一版样本从哪里来，以及为什么
- Eval case 草图：2-3 个关键字段或样例
- MECE 评分标准雏形：一级维度即可
- 执行起步：人评 / LLM-as-a-judge / 抽检怎么安排
- 当前缺口：用户还缺什么材料或判断
- 下一步：通过或有条件通过时，主动 cue Gate 5，说明接下来要看“如何根据评测发现 badcase、定位链路问题、修复并验证”

判断标准：
- 如果找不到任何可评测对象，判定不通过，回到 Gate 3 调整链路。
- 如果没有样本来源，判定不通过。
- 如果用户完全不了解评测，不要判死刑；给最小评测起步方案，通常判有条件通过。
- 如果 rubric 只是散点指标，判定有条件通过并重构为 MECE。
- 如果一上来就想全自动机评但没有人评标准，判定有条件通过，要求先人评校准。
- 如果已经能说清最优先评测对象、样本来源、一级评分维度和起步执行方式，就进入 Gate 5；不要等完整评测体系全部写完。
```

## Gate 5: Badcase 迭代闭环

Purpose: judge whether the user understands that evaluation should produce product iteration, not only a score. This gate should transmit the core PM idea: the interview-worthy part is often not "I built the feature", but "I found a badcase, diagnosed why it happened, chose what to change, and checked whether the change worked."

This gate teaches the product manager's daily loop: finishing the feature is only the first step. The real product work is using data/evaluation to discover problems, decide what to fix, and verify that the fix worked.

Scope boundary: do not proactively generate complete badcase examples for the user. First diagnose whether their current project expression already contains badcase discovery and iteration awareness. If not, tell them what kind of case they should go collect or think through. You may give short directional examples, but do not write a full badcase table unless the user explicitly asks for it later.

Pass conditions:

- The user can explain the loop: evaluation/data -> low-score phenomenon -> chain or product diagnosis -> fix choice -> verification.
- The project has at least one plausible badcase direction the user can collect or construct.
- The user distinguishes basic chain/debug badcases from deeper product/user-experience badcases.
- The user understands that "优化 prompt" is not enough; the fix must map to a chain node, product rule, context strategy, fallback, or user experience decision.

Fail conditions:

- The project expression only says "上线/完成/实现功能" and never mentions discovering problems after the feature works.
- Badcases are generic examples not tied to evaluation dimensions or user phenomena.
- The user can only say "模型答错了" or "优化 prompt", with no diagnosis.
- The user has no plan to collect low-score samples, user complaints, repeated questions, human corrections, preference signals, or review notes.

### Two Badcase Levels

Teach this distinction clearly.

#### Level 1: 基础链路 / 技术跑通类

These are useful, but usually not enough alone:

- query rewrite failed
- retrieval missed the right source
- metadata or chunking caused wrong context
- output schema missed a field
- prompt instruction was too vague
- fallback or refusal threshold was wrong
- tool/API returned missing or stale data

This level proves the user can debug an AI chain.

#### Level 2: 端到端产品体验 / 用户价值类

This is more advanced and often more impressive:

- the chain technically works, but the answer does not solve the user's real task
- the output is correct but too long, too cautious, too generic, or badly timed
- the system optimizes the obvious request but misses the user's deeper intent
- user behavior shows repeated follow-up, preference for another answer, budget expansion, abandonment, or manual override
- the system could create extra value by reframing the decision, not merely answering the literal query

Example direction, only use when relevant to recommendation, pricing, or budget-expansion scenarios:

```text
Weak view: 用户预算是 300，就只推荐 300 内商品。
Product view: 如果用户需求强烈但预算限制来自信息不足，可以测试是否推荐略高价但更匹配需求的商品，并观察偏好选择、加购、转化或继续咨询。这里的 badcase 不是技术错了，而是产品策略太死。
```

### What To Ask The User

If the user has no badcase material yet, ask them to try one focused prompt, not a long questionnaire:

```text
你先不要编完整案例。你回想一下：这个链路跑起来后，最可能出现哪一种“不算技术报错，但用户仍然觉得没被解决”的情况？
```

Or:

```text
你现在能不能先给一个低分样本方向：它是链路没跑通，还是链路跑通了但产品体验不对？
```

Prompt:

```text
你现在只判断【是否具备 badcase 迭代意识】，不要主动生成完整 badcase 案例表，也不要写简历 bullet。

请先传达核心观点：
功能做完只是第一步。真正能证明产品经理能力的是：发现问题 -> 判断问题属于哪个链路/产品环节 -> 决定怎么改 -> 用同一口径验证是否变好。

然后基于当前项目判断：
1. 用户当前表达里有没有体现“发现 badcase 并主动迭代”的行为？
2. 如果没有，应该去补哪类素材：低分样本、用户追问、人工修正、偏好选择、投诉/反馈、review note、失败日志？
3. 这个项目更适合先从哪类 badcase 思考：基础链路/技术跑通类，还是端到端产品体验/用户价值类？
4. 给用户一个方向性引导，让 TA 自己尝试提出 1 个 badcase 方向。可以给短例子，但不要替 TA 写完整案例。

输出结构：
- 当前步骤：Gate 5 / 6：Badcase 迭代闭环
- 阶段判断：通过 / 有条件通过 / 不通过
- 核心观点：功能完成之后，如何用评测/数据发现问题并迭代
- 当前项目表达诊断：有没有体现 badcase 意识
- 建议补的 badcase 方向：基础链路类 / 端到端产品体验类
- 更推荐的思考方向：尽量引导到用户体验、用户行为、产品策略，而不只停在技术报错
- 给用户的尝试问题：让用户自己提出 1 个 badcase 方向
- 下一步：用户理解并能提出方向后，cue Gate 6 证据与简历边界

判断标准：
- 如果用户完全没有 badcase 意识，判定有条件通过或不通过，要求先补一个低分样本方向。
- 如果只有基础技术报错，判定有条件通过，并建议再想一个端到端产品体验类方向。
- 如果用户能说清“发现问题 -> 归因 -> 修改 -> 验证”的逻辑，即使案例还不完整，也可以进入 Gate 6。
```

## Gate 6: 证据与简历边界

Purpose: judge what the user can safely claim, what they should actually do next, and where the resume/interview boundary is. This gate should convert the previous gates into a concrete To Do list and evidence boundary. Do not default to writing resume bullets.

Pass conditions:

- Can truthfully separate what was actually done, what can be quickly补做, and what is only a proposed AI-upgrade direction.
- The user has a 3-7 day evidence plan: prototype, flowchart, sample set, eval table, scoring rubric draft, badcase note, prompt chain, knowledge structure, or demo screenshots.
- Claims avoid launch, growth, AB test, internal access, internal deployment, model ownership, algorithm optimization, or business impact unless evidenced.
- Interview defense can explain what was real, what was simulated/offline, what was based on historical experience, and what was proposed.

Fail conditions:

- Resume bullet depends on fabricated launch, user scale, internal database, or business results.
- User wants to claim evaluation, badcases, or AI optimization without actually trying even a small offline version.
- User cannot explain the AI chain, eval direction, or badcase iteration logic.
- The project is only a plan with no artifact.

### Evidence Boundary

Classify claims into three buckets:

Can say:

- "基于真实经历/材料，设计了 AI 化方案"
- "补做了离线样本、评测表、评分标准、badcase 归因"
- "用公开/脱敏/手工构造样本跑过小规模验证"
- "做了原型、流程图、Prompt chain、知识结构或 Demo"
- "提出了可复测的优化方向"

Say carefully:

- "尝试将传统实习经历 AI 化"
- "基于历史业务材料做离线评估"
- "用 XX 条样本初步验证"
- "设计/模拟/离线测试"，not "上线/落地/负责真实系统"

Do not say unless evidenced:

- internal launch, AB test, production deployment, real user growth, conversion improvement
- access to private database or confidential internal system
- owning model training, algorithm optimization, or infrastructure
- exact business metrics without sample size, metric definition, and comparison method

### What The User Should Actually Do

This gate should push real action. If the user has not run evals or badcase analysis, tell them to do a small version. The goal is not to fabricate results, but to gain real feel for data, model limits, and failure modes.

Good 3-7 day tasks:

- collect or construct 20-50 safe samples
- make a one-page workflow diagram
- build a small prototype or run the core prompt chain manually
- create an eval table with 3-5 top-level dimensions
- manually score 10-20 outputs
- record 2-3 low-score samples and write why they failed
- revise one node and compare before/after with the same rubric
- write a short note distinguishing "real experience", "offline supplement", and "future AI-upgrade proposal"

### Interview Questions

Give at most 3 likely interview questions. They must be personalized to the project and previous gate decisions. Avoid generic questions like "为什么做这个评测". Better questions compare tradeoffs:

- "你既然选择 RAG + metadata，而不是直接长上下文塞全文，是因为你的材料有什么结构特点？"
- "你把转人工放在退款争议节点，而不是让模型继续解释规则，背后的风险判断是什么？"
- "你说用离线样本验证，那这些样本如何避免只覆盖高频简单问题？"

Questions should force the user to defend a specific choice: scenario, AI necessity, chain node, eval target, badcase direction, evidence boundary, or non-AI fallback.

Prompt:

```text
你现在只判断【证据与简历边界】，不要默认生成简历 bullet。除非用户明确要求，否则只给边界、To Do、面试追问方向。

请基于前面 Gate 1-5 的结论，输出：
1. 哪些部分可以基于真实经历安全表达？
2. 哪些部分需要用户在 3-7 天内实际补做一遍？尤其是评测、样本、badcase、原型、流程图、prompt chain、知识结构。
3. 哪些说法必须禁止：上线、AB test、内部数据库、真实用户增长、算法优化、模型训练、内部部署等无证据内容。
4. 如果用户原本是传统实习经历，没有真正 AI 化，可以怎么诚实表达：尝试基于真实流程做 AI 化补充设计 / 离线验证 / 原型探索，而不是说成内部已上线项目。
5. 给 3 个高度贴合当前项目的面试追问方向，问题要具体到用户选择的方案、样本、链路、兜底或取舍，不要泛泛问“为什么这么做”。

输出结构：
- 当前步骤：Gate 6 / 6：证据与简历边界
- 阶段判断：通过 / 有条件通过 / 不通过
- 可以安全表达：
- 需要补做的证据：
- 谨慎表达：
- 不能写：
- 3-7 天 To Do List：按优先级列 3-6 项，每项有产物
- 个性化面试追问：3 个，必须结合当前项目细节
- 是否需要总结：问用户要不要把 Gate 1-6 的结论整理成一份完整总结

判断标准：
- 如果没有任何可展示 artifact，判定不通过或有条件通过，只给最小证据计划。
- 如果证据有限，判定有条件通过，用保守表达。
- 如果用户完全没做过评测但想写评测结果，判定有条件通过，要求先跑一个小规模离线版本。
- 如果证据充分，也不要自动生成简历 bullet；先问用户是否需要把前面内容总结成完整报告或简历表达。
```

## Gate To Deliverable Mapping

Use this mapping to decide how far to answer:

| User intent | Required gates | If failed |
| --- | --- | --- |
| Project Direction | Gate 1-3 | Stop and give repair path before naming a polished project |
| Project Iteration | Gate 1-5 | Stop at the first broken premise or missing eval loop |
| Resume Diagnosis | Gate 1, 2, 6 | Classify experience as deep-dive / auxiliary / compress / not suitable |
| Full Report | Gate 1-6 | Use full template only after all required gates pass or clearly mark conditional assumptions |
| Interview Defense | Gate 3-6 | Refuse to over-defend claims that lack evidence |

For thin materials, prefer a "minimum viable evidence plan" over a polished fake project.
