# Output Template

Use this structure for the final deliverable. Keep it specific to the user's real material. Do not include sections that are impossible to answer; if material is thin, say what evidence must be built first.

```markdown
# AI 项目落地方案文档

## 一、总判断

- 最推荐项目方向：
- 为什么这个方向成立：
- 当前最大风险：
- 不建议主打的方向：

### 项目底层逻辑

```text
从【具体功能点/用户任务】出发
-> 选择【AI 技术方案：Prompt / RAG / Workflow / Sub-agent / Agent Team / LLM-as-a-judge 等】
-> 设计【评测集 + MECE 评分标准】
-> 跑评测集，定位【链路节点】的问题
-> 针对低分维度迭代【prompt / metadata / query rewrite / context / routing / judge / fallback】
-> 用同一评测集复测，观察【目标指标】提升
```

## 二、候选项目筛选

| 经历/素材 | 判断 | 可 AI 化方向 | 风险 |
| --- | --- | --- | --- |
|  | 深挖 / 辅助 / 压缩 / 不建议 |  |  |

## 三、推荐项目落地方案

### 1. 项目一句话定位

> 面向【用户】在【场景】中的【痛点】，设计【AI 产品/工作流】，通过【核心 AI 环节】实现【可验证价值】。

### 2. 真实业务基础

- 用户是谁：
- 场景是什么：
- 原始流程/材料：
- 用户真实参与点：
- 可以补做的证据：

### 3. 用户、场景、痛点

| 维度 | 内容 |
| --- | --- |
| 用户 |  |
| 场景 |  |
| 核心痛点 |  |
| 为什么需要 AI |  |
| 必须人工确认的环节 |  |

### 4. AI 介入环节

```text
原始输入
-> Task Specification：把用户目标拆成结构化任务
-> Context Selection：选择材料/历史/记忆/工具
-> AI/系统节点 1：Prompt / RAG / query rewrite / tool call / Agent
-> AI/系统节点 2：生成 / 评估 / 反证 / 重排 / 压缩
-> Verification：规则校验 / LLM-as-a-judge / 人工抽检
-> 人工确认/兜底
-> Tracing + Badcase 回流
```

说明每个节点的输入、输出、评价标准和失败表现。

### 5. 产品链路与技术架构

架构选择：

- 固定 Workflow / Sub-agent / Agent Team / 混合形态：
- 为什么选它：
- 为什么不用更复杂/更简单的形态：

- 第一步：
- 第二步：
- 第三步：
- 第四步：

Harness 映射：

| Harness 节点 | 本项目怎么体现 | 输入 | 输出 | 失败表现 |
| --- | --- | --- | --- | --- |
| Task Specification |  |  |  |  |
| Context Selection |  |  |  |  |
| Tool / Agent Routing |  |  |  |  |
| State / Tracing |  |  |  |  |
| Verification / Evals |  |  |  |  |
| Human-in-the-loop |  |  |  |  |

### 6. 评测集与自动化评估设计

#### 6.1 评估目的与流程

评估目的：

- 拟合线上真实情况 / 内部迭代 / 边界压测 / 方案对比 / 链路定位：

评估流程：

```text
准备 eval cases
-> 跑 baseline 链路
-> 按 MECE rubric 打分
-> 找到低分维度
-> 查看 tracing 定位链路问题
-> 修改链路
-> 用同一评测集复测
```

#### 6.2 评测集来源与取舍

评测集来源：

| 来源 | 优点 | 风险 | 本项目怎么用 |
| --- | --- | --- | --- |
| 真实样本 | 贴近实际分布 | 隐私/偏常规问题 |  |
| 历史/公开材料 | 可展示、可复核 | 不一定贴合本人经历 |  |
| 人工构造边界 case | 能压测风险 | 可能不够真实 |  |
| AI 生成 case | 便宜、覆盖快 | 容易假 | 只做初稿，需人工筛 |

#### 6.3 MECE 评分标准

评分标准必须按一级维度组织，不要散点罗列。

| 一级维度 | 二级指标 | 怎么打分 | 低分说明什么 | 对应链路 |
| --- | --- | --- | --- | --- |
| 内容/任务正确性 |  | 0/1/2 或 1-5 分 |  |  |
| 证据/上下文支撑 |  | 0/1/2 或 1-5 分 |  |  |
| 用户可用性/行动性 |  | 0/1/2 或 1-5 分 |  |  |
| 风险兜底/人工确认 |  | 0/1/2 或 1-5 分 |  |  |

#### 6.4 自动化评估执行方式

- 第一版人工标注样本数：
- LLM-as-a-judge 是否使用：
- Judge prompt 如何校准：
- 抽样人工复核比例：
- tracing 表字段：

```text
case_id / input / expected_focus / retrieved_context / output / score_by_dimension / badcase_type / chain_node / revision_note
```

### 7. Badcase 与优化策略

#### Badcase A：基础问题

- 低分维度：
- 用户/业务现象：
- 问题表现：
- tracing 看到什么：
- 链路归因：
- 修改哪个节点：
- 优化动作：
- 复测指标：
- 预期分数变化：

#### Badcase B：进阶问题

- 低分维度：
- 用户/业务现象：
- 问题表现：
- tracing 看到什么：
- 链路归因：
- 修改哪个节点：
- 优化动作：
- 复测指标：
- 预期分数变化：

### 8. 3-7 天可补做材料

P0：

- [ ] 

P1：

- [ ] 

P2：

- [ ] 

### 9. 面试追问防御

#### Q1：

答：

#### Q2：

答：

#### Q3：

答：

## 四、简历表达方向

### 项目标题

【项目名称】

### 背景句

针对【用户/场景】中【问题】，设计【AI 项目】。

### 可替换 bullet

- 【AI 技术方案 + 产品决策】：例如 Prompt chain / RAG / query rewrite / Agent Team / Sub-agent / LLM-as-a-judge / tracing，【为什么这样设计】。
- 【评测与自动化评估】：构建【XX】条 eval cases，按【MECE rubric】评估【目标指标】，用【人工评分/LLM-as-a-judge】执行批量评测。
- 【Badcase 迭代】：针对【低分维度】定位【链路节点】问题，优化【prompt / metadata / context / routing / judge / fallback】，复测后【XX 指标】提升。
- 【边界与兜底】：设计【人工确认/权限/拒答/引用校验】机制，避免【幻觉/过度归因/编造数据】。

## 五、边界提醒

可以写：

- 

谨慎写：

- 

不能写：

- 

## 六、行动优先级

| 优先级 | 动作 | 预计耗时 | 验收标准 |
| --- | --- | --- | --- |
| P0 |  |  |  |
| P1 |  |  |  |
| P2 |  |  |  |
```

## Quality checklist

Before finalizing, verify:

- The project lands in a concrete scenario.
- The user can truthfully explain the business basis.
- Evaluation and badcases are present.
- Evaluation explains purpose, sample sources, MECE rubric, automation method, and tracing.
- Badcases start from low score dimensions and lead to chain modifications plus retest metrics.
- Resume bullets name concrete AI technical solutions, not only feature design.
- Data placeholders include口径.
- Boundary reminders prevent overclaiming.
- Actions can be completed, not just understood.
