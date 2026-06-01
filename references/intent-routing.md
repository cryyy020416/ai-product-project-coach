# Intent Routing

Use this first. The skill must support both full reports and flexible consulting Q&A.

## Routing rule

Classify the user's request into one primary mode, then answer in that mode. If the request contains multiple intents, handle the user's explicit question first and then add the next recommended step.

## Mode 1: Full Report

Signals:

- User uploads resume and asks for a完整方案、完整报告、帮我规划 AI 项目、帮我转 AI 产品。
- User wants a deliverable document.

Output:

- Use `output-template.md`.
- Read judgment principles, harness logic, assessment framework, project patterns, and relevant cases.
- If resume facts are insufficient, ask up to 5 high-impact questions before drafting.

Do:

- Diagnose resume materials.
- Pick 1 main project and 1 backup.
- Include bottom logic, architecture, eval design, badcases, resume bullets, boundaries, and actions.

## Mode 2: Project Direction

Signals:

- User says they have no AI project.
- User asks for a non-generic idea.
- User gives a major/background but little project material.

Output:

- Give 1 strong direction and 1-2 backup directions.
- Do not pretend the idea is already resume-ready.
- Include why it fits, what evidence to build in 3-7 days, and what not to claim.

Minimum structure:

```text
Recommended direction
Why it fits
Project bottom logic
Architecture choice
3-7 day evidence plan
Resume expression draft
Boundary reminders
```

## Mode 3: Project Iteration

Signals:

- User has a webpage, demo, personal AI tool, RAG, prompt project, Agent, or product prototype but gets no interviews.
- User asks how to优化迭代项目.

Output:

- First diagnose why the project currently looks weak.
- Usually the issue is: feature list without AI decision, no eval set, no badcase, no metric口径, no interview defense.
- Then provide an iteration plan.

Minimum structure:

```text
Current likely problem
What AI product value is missing
How to rebuild the chain
Eval set and MECE rubric
Badcase-driven iteration
Resume bullet rewrite
```

## Mode 4: Resume Diagnosis

Signals:

- User uploads resume and asks why no interviews, what is wrong, how to改简历.
- User may not ask for a project yet.

Output:

- Diagnose before rewriting.
- Classify experiences as deep-dive, auxiliary, compress/delete, or not suitable.
- Point out whether the first 15 seconds show AI product signals.

Minimum structure:

```text
Overall diagnosis
Strongest AI product signal
Weakest signal / biggest risk
Experience classification
What to move forward / compress / rewrite
Project opportunities
```

Avoid:

- Full resume rewrite unless asked.
- Generic "increase product thinking" advice.

## Mode 5: AI PM Q&A

Signals:

- User asks what AI product managers do.
- User asks how much they need to know about RAG, Agent, model, prompt, evals, coding.
- User asks conceptual questions about AI product work or interviews.

Output:

- Answer directly.
- Use concrete product work examples.
- Translate technical depth into "what a PM must be able to decide/evaluate", not engineer-level implementation.

Recommended framing:

```text
You do not need to be the engineer, but you must know each node's input, output, evaluation standard, and failure mode.
```

Common topics:

- RAG: query rewrite, chunking, metadata, retrieval, answer grounding, fallback.
- Agent: task specification, context selection, tool routing, state, tracing, verification.
- Prompt: prompt chain, evidence constraint, long-context decay, judge prompt.
- Evals: eval set source, MECE rubric, LLM-as-a-judge, manual calibration, badcase loop.

## Mode 6: Interview Defense

Signals:

- User asks how to讲项目、面试会不会被问穿、追问怎么答.

Output:

- Give likely interviewer questions and defensible answers.
- Emphasize boundaries and real participation.
- Prepare 2 badcases and 1 metric口径.

Minimum structure:

```text
Core story
3 likely questions
Defensible answers
Badcase examples
What not to say
```

## Narrow answer policy

If the user asks a narrow question, do not force a full report. Answer the narrow question well, then offer the next useful artifact, such as:

- "如果你要继续，我下一步可以基于你的简历做项目方向筛选。"
- "如果你把项目链接/简介贴来，我可以按 eval + badcase 逻辑帮你迭代。"

## Uploaded resume policy

If a resume is attached or pasted:

- Read it as the primary source.
- Do not assume missing internships or project facts.
- If the user asks a specific question, answer that question using resume evidence.
- If the user asks for a full plan, run Full Report mode.
