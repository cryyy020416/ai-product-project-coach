# Anonymized Cases

Use these cases as reasoning references, not as copyable content. Do not expose names, source files, or private details in final output.

## Case 1: AI Resume / Interview Review Tool

User type:

- Has a personal AI tool or school/product project.
- The raw description overemphasizes features and interface functions.

Core problem:

- The project reads like a feature list, not an AI product judgment.
- It does not explain what AI detects, how output quality is judged, or what badcases were improved.

Safe direction:

- Reframe as an AI diagnosis / prompt optimization project.
- Define the value: compare user output against a job/task standard, identify hidden gaps, and generate actionable feedback.
- Add eval set, scoring rubric, hallucination control, and before/after prompt-chain iteration.

Reusable rule:

- Product value first, feature list second.

## Case 2: Intelligent Customer Service Annotation -> Evaluation Project

User type:

- Real exposure to customer service annotation, answer scoring, platform support, or ticket review.
- Worries that "I only did labeling" is too execution-level.

Core problem:

- Cannot claim algorithm optimization or full product ownership.
- But pure annotation undersells the AI product value.

Safe direction:

- Translate annotation into answer quality evaluation and badcase attribution.
- Build the chain:

```text
intent taxonomy -> eval set -> scoring rubric -> badcase attribution -> optimization proposal -> offline retest
```

Reusable rules:

- Separate online user chain from internal evaluation chain.
- User-side phenomena like repeated questions or manual transfer can be mapped back to intent, retrieval, answer generation, slot, or fallback problems.
- Do not invent AB test or traffic rollout if the user did not participate.

## Case 3: Internal RAG / Knowledge Assistant from Operations Materials

User type:

- Has FAQ, SOP, internal docs, knowledge assistant exposure, sales/support/operations materials.
- Wants to package it as a RAG project.

Core problem:

- "Built a knowledge base" is too shallow and common.
- Interviewers may ask about intent, chunking, retrieval, eval set, and multi-round handling.

Safe direction:

- Present the project as knowledge governance + RAG evaluation.
- Explain two chains:

```text
online chain: query -> intent/slot -> retrieval -> generation -> clarification/fallback
internal eval chain: eval set -> scoring -> badcase -> knowledge/prompt/retrieval iteration
```

Reusable rules:

- Chunk size has no universal answer; say baseline -> eval -> badcase attribution -> adjust.
- RAGAS or LLM judge is only a tool; product value lies in defining answer quality standards.
- Multi-round evaluation may require simulated users, not just single-turn cases.

## Case 4: Non-AI Background -> From-Scratch AI Project

User type:

- Has consulting, investment research, operations, campus advising, or domain expertise but lacks an obvious AI product project.

Core problem:

- Domain knowledge exists, but the project lacks product closure and proof.
- Internal-company scenarios may be risky because of access, data, or compliance questions.

Safe direction:

- Pick a controllable public or personal workflow where the user can create artifacts.
- Example structures:
  - AI interview training Agent
  - AI product intelligence/research Agent
  - consulting outline generation copilot
  - domain-specific RAG assistant with offline eval

Reusable rules:

- Avoid claiming internal deployment.
- Use public data, personal materials, anonymized samples, or manually constructed eval cases.
- A credible 3-7 day artifact package can be enough for interview defense: flowchart, eval set, rubric, badcases, prototype screenshots, and resume bullets.

### Finance / research-specific upgraded direction

Avoid shallow ideas:

- AI writes research reports
- financial RAG knowledge base
- stock prediction model
- generic market news summary

Prefer projects that make the research process more verifiable:

#### Evidence-chain audit Agent

Positioning:

> An Agent Team that audits whether research conclusions are supported by traceable evidence from public filings, earnings calls, news, industry data, or notes.

Architecture:

```text
user uploads report / research question
-> Claim Extractor Agent extracts key claims
-> Source Agent retrieves public materials
-> Evidence Agent matches each claim to evidence
-> Critic Agent checks contradiction, weak support, and over-attribution
-> Report Agent outputs evidence audit table
-> Judge scores claim grounding and risk handling
```

MECE rubric example:

```text
Research Evidence Quality
├── Claim extraction: key claims are complete and not fragmented
├── Evidence grounding: evidence truly supports the claim, not just related
├── Reasoning validity: causal chain is not over-stated
└── Risk handling: weak evidence, stale sources, and counter-evidence are flagged
```

Badcase progression:

- Shallow: key claim has no citation because output format did not require source. Modify report prompt to force source_id and source location.
- Deep: retrieved source is related but does not support the causal claim. Add Critic Agent to distinguish fact, inference, market opinion, and unsupported attribution.

Resume expression:

```text
设计投研观点证据链审计 Agent Team，将研究报告拆解为 Claim Extractor、Source Agent、Evidence Agent、Critic Agent 和 Report Agent，评估核心观点是否具备公开材料支撑。
构建【XX】条观点-证据 eval cases，按观点完整性、证据支撑度、因果链有效性和风险兜底四层 rubric 评分，并用 LLM-as-a-judge + 人工抽检定位弱证据和过度归因 badcase。
```

#### Research workflow Harness

Positioning:

> A research collaboration workflow that manages source trust, evidence cards, hypothesis tracking, report drafting, critique, and review feedback.

Key distinction:

- It is not personal knowledge management.
- It is not "AI writes a report".
- It is a workflow for evidence, hypothesis, verification, and human review.

Core nodes:

```text
Task Specification: define company/industry/question/time range
Context Selection: choose filings, news, notes, prior hypotheses
Evidence Card Generation: fact/source/time/trust/hypothesis
Hypothesis Tracking: support, contradiction, status, next check
Report Drafting: grounded draft with citations
Critic/Judge: check fact consistency, counter-evidence, overclaiming
Human Review: final judgment remains with analyst
Tracing: store source, retrieved context, output, score, badcase
```

Evaluation:

- citation hit rate
- fact consistency
- evidence coverage
- counter-evidence coverage
- source freshness
- traceability success rate
- human revision type distribution

Boundary:

- Do not claim investment recommendation, alpha, return prediction, internal deployment, or access to private data.
- Write "research support", "evidence audit", "offline evaluation", and "human review".

## Case 5: Technical / Scientific Model Project -> Product Translation

User type:

- Has ML, prediction, recommendation, scientific computing, or school thesis experience.

Core problem:

- The project is full of model names and metrics but lacks user problem, error cost, and product decision.

Safe direction:

- Translate technical work into decision support:
  - What decision does the model help?
  - Which error is more expensive?
  - Why the metric fits the business/scenario?
  - How does a downstream user trust or use the result?

Reusable rules:

- Do not hide technical depth, but do not let it replace product reasoning.
- Explain AUROC/recall/precision/Top-K through the decision being supported.
