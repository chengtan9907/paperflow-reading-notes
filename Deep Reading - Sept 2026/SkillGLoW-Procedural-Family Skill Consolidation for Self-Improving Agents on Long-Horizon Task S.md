---
user_id: "cheng tan"
paper_id: 10450
arxiv_id: "2609.02217v1"
title: "SkillGLoW: Procedural-Family Skill Consolidation for Self-Improving Agents on Long-Horizon Task Streams"
publish_date: "2026-09-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.02217v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.02217v1"
generation_provider: "heuristic"
generation_model: "PaperFlow template"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-05T01:35:43"
---
# SkillGLoW: Procedural-Family Skill Consolidation for Self-Improving Agents on Long-Horizon Task Streams

> ★★★★☆ 推荐阅读 · 模型 heuristic/PaperFlow template

🏷 关键词：across four · admits prior · agents increasingly · aggregated procedural · alfworld evidence · library · pool · global

## 一句话总结

LLM agents increasingly self-improve by writing and reusing textual skills, kept either as one global document or as a flat pool of per-task entries, though most of the evidence…

## 摘要

> LLM agents increasingly self-improve by writing and reusing textual skills, kept either as one global document or as a flat pool of per-task entries, though most of the evidence comes from domains with structurally similar tasks. On long-horizon workloads where each task demands a different solution, the two forms fail in opposite ways: the document collapses into generic discipline, while the pool inflates and its entries stay bound to the instance that wrote them. We argue the missing unit of reuse is the solving procedure shared by a cluster of related tasks, and build SkillGLoW (Global-Local Weave) around it: the local skills a task writes from its own execution are aggregated into procedural families and compressed into de-instantiated global priors, while the instance detail they hold is regenerated per task rather than stored; a commit gate admits a prior only when real execution shows it does not degrade the deployed library. Across four benchmarks (mathematical reasoning, terminal automation, software repair, and embodied control) and three models, the priors gain 17.2 points (hard) over the no-skill baseline on average, with positive gains in all 12 continual-improvement runs, and 18.0 with local regeneration, while the library holds one prior per procedural family, 3.6x more compact than the per-task pool. Under the same protocol GLoW leads a published single-document optimizer on 15 of 21 cells. Unmodified, the library lifts success on unseen ALFWorld tasks from 73.9% to 83.9%, evidence that what transfers is procedure rather than task memory.

Q1: 这篇论文试图解决什么问题？

LLM agents increasingly self-improve by writing and reusing textual skills, kept either as one global document or as a flat pool of per-task entries, though most of the evidence… On long-horizon workloads where each task demands a different solution, the two forms fail in opposite ways: the document collapses into generic discipline, while the pool…

Q2: 有哪些相关研究？

当前自动解析没有稳定提取出 Related Work 的完整脉络。可先从论文引言、相关工作章节和引用线索核对它主要对比了哪些方法。

从当前证据可见，论文的定位至少包括：Building on this observation, we propose SkillGLoW (Global–Local Weave;；failures point to a missing unit of reuse, one that sits between the single document and the single entry.；As base models and agent harnesses mature, LLM agents are moving from short-horizon, closed tasks toward longer-horizon, more complex environments (Yao et al.。

Q3: 论文如何解决这个问题？

Building on this observation, we propose SkillGLoW (Global–Local Weave; failures point to a missing unit of reuse, one that sits between the single document and the single entry.

Q4: 论文做了哪些实验？

As base models and agent harnesses mature, LLM agents are moving from short-horizon, closed tasks toward longer-horizon, more complex environments (Yao et al. Shinn et al.

Q5: 发现了什么实验现象？

As base models and agent harnesses mature, LLM agents are moving from short-horizon, closed tasks toward longer-horizon, more complex environments (Yao et al. Shinn et al.

Q6: 有什么可以进一步探索的点？

- the solving procedure shared by a cluster of tasks.
- \- We propose GLoW, which adopts the procedural family as this unit and gates long-term updates by real execution (§3.1–§3.5).
- 优先核对语义命中的全文片段：Abstract。

Q7: 总结一下论文的主要内容

LLM agents increasingly self-improve by writing and reusing textual skills, kept either as one global document or as a flat pool of per-task entries, though most of the evidence…

LLM agents increasingly self-improve by writing and reusing textual skills, kept either as one global document or as a flat pool of per-task entries, though most of the evidence… On long-horizon workloads where each task demands a different solution, the two forms fail in opposite ways: the document collapses into generic discipline, while the pool…

Building on this observation, we propose SkillGLoW (Global–Local Weave; failures point to a missing unit of reuse, one that sits between the single document and the single entry.

As base models and agent harnesses mature, LLM agents are moving from short-horizon, closed tasks toward longer-horizon, more complex environments (Yao et al. Shinn et al.

主要贡献包括：Building on this observation, we propose SkillGLoW (Global–Local Weave;；failures point to a missing unit of reuse, one that sits between the single document and the single entry.；As base models and agent harnesses mature, LLM agents are moving from short-horizon, closed tasks toward longer-horizon, more complex environments (Yao et al.。

需要注意的边界包括：the solving procedure shared by a cluster of tasks.；\- We propose GLoW, which adopts the procedural family as this unit and gates long-term updates by real execution (§3.1–§3.5).。

与用户画像的关系：从全文语义检索命中的片段看，相关信息主要落在 Abstract 部分。；这篇论文和你当前画像里的方向有直接重合：智能体（权重 0.10）, 生成（权重 0.10）。；如果你更看重系统性工作，可以重点看它如何组织任务设定、实验协议和整体框架。。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在 Abstract 部分。

## 基本信息

- 作者：Ao Yan, Xin Zhang, Jiawei Du, Joey Tianyi Zhou
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-09-02
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：heuristic / PaperFlow template
- arXiv ID：`2609.02217v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 生成式精读补充本次未返回，当前内容仍按精读模板基于已拿到的摘要、元数据和可用 PDF 片段生成。
