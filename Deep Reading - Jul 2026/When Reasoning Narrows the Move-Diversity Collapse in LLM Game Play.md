---
user_id: "cheng tan"
paper_id: 5403
arxiv_id: "2607.19523"
title: "When Reasoning Narrows the Move: Diversity Collapse in LLM Game Play"
publish_date: "2026-07-23"
pdf_url: "https://arxiv.org/pdf/2607.19523"
abs_url: "https://arxiv.org/abs/2607.19523"
generation_provider: "heuristic"
generation_model: "PaperFlow template"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-23T17:20:27"
---
# When Reasoning Narrows the Move: Diversity Collapse in LLM Game Play

> ★★★★☆ 推荐阅读 · 模型 heuristic/PaperFlow template

🏷 关键词：collapse llm · diversity collapse · optimal actions · action · sft

## 一句话总结

We study this question in a controlled suite of deterministic board games based on tic-tac-toe variants, where optimal actions are exactly computable and diversity can be…

## 摘要

> Supervised fine-tuning (SFT) is widely used to adapt large language models to downstream tasks, but its effect on behavioral diversity in sequential decision-making remains under-explored. We study this question in a controlled suite of deterministic board games based on tic-tac-toe variants, where optimal actions are exactly computable and diversity can be measured directly. Across state-level evaluation, arena gameplay, and training trajectories, we find that reasoning-mode generation frequently suppresses action diversity without uniformly improving action accuracy. Furthermore, standard SFT improves accuracy but often induces premature diversity collapse, which exceeds what is minimally required by the accuracy-diversity tradeoff. We then show that action augmentation, which trains on all optimal actions per state rather than a single demonstrated action, would partially mitigates this effect. Our results identify narrow-support imitation as a source of policy collapse in LLM decision-making and suggest that preserving action support during SFT is important for maintaining exploratory behavior.

Q1: 这篇论文试图解决什么问题？

Supervised fine-tuning (SFT) is widely used to adapt large language models to downstream tasks, but its effect on behavioral diversity in sequential decision-making remains under-explored. We study this question in a controlled suite of deterministic board games based on tic-tac-toe variants, where optimal actions are exactly computable and diversity can be measured directly.

Q2: 有哪些相关研究？

当前自动解析没有稳定提取出 Related Work 的完整脉络。可先从论文引言、相关工作章节和引用线索核对它主要对比了哪些方法。

从当前证据可见，论文的定位至少包括：We study this question in a controlled suite of deterministic board games based on tic-tac-toe variants, where optimal actions are exactly computable and diversity can be…；Supervised fine-tuning (SFT) is widely used to adapt large language models to downstream tasks, but its effect on behavioral diversity in sequential decision-making remains…；Across state-level evaluation, arena gameplay, and training trajectories, we find that reasoning-mode generation frequently suppresses action diversity without uniformly…。

Q3: 论文如何解决这个问题？

Supervised fine-tuning (SFT) is widely used to adapt large language models to downstream tasks, but its effect on behavioral diversity in sequential decision-making remains under-explored.

Q4: 论文做了哪些实验？

Across state-level evaluation, arena gameplay, and training trajectories, we find that reasoning-mode generation frequently suppresses action diversity without uniformly improving action accuracy. Furthermore, standard SFT improves accuracy but often induces premature diversity collapse, which exceeds what is minimally required by the accuracy-diversity tradeoff.

Q5: 发现了什么实验现象？

Across state-level evaluation, arena gameplay, and training trajectories, we find that reasoning-mode generation frequently suppresses action diversity without uniformly improving action accuracy. Furthermore, standard SFT improves accuracy but often induces premature diversity collapse, which exceeds what is minimally required by the accuracy-diversity tradeoff.

Q6: 有什么可以进一步探索的点？

- 文中未明显展开局限性，阅读时建议重点核对数据覆盖范围、评测设置和泛化边界。
- 本次未成功抓取 PDF，方法与实验细节部分仍应以原文为准。
- 当前建议先读摘要，再回到原文按图表和章节标题定位方法与实验细节。

Q7: 总结一下论文的主要内容

We study this question in a controlled suite of deterministic board games based on tic-tac-toe variants, where optimal actions are exactly computable and diversity can be…

Supervised fine-tuning (SFT) is widely used to adapt large language models to downstream tasks, but its effect on behavioral diversity in sequential decision-making remains under-explored. We study this question in a controlled suite of deterministic board games based on tic-tac-toe variants, where optimal actions are exactly computable and diversity can be measured directly.

Supervised fine-tuning (SFT) is widely used to adapt large language models to downstream tasks, but its effect on behavioral diversity in sequential decision-making remains under-explored.

Across state-level evaluation, arena gameplay, and training trajectories, we find that reasoning-mode generation frequently suppresses action diversity without uniformly improving action accuracy. Furthermore, standard SFT improves accuracy but often induces premature diversity collapse, which exceeds what is minimally required by the accuracy-diversity tradeoff.

主要贡献包括：We study this question in a controlled suite of deterministic board games based on tic-tac-toe variants, where optimal actions are exactly computable and diversity can be…；Supervised fine-tuning (SFT) is widely used to adapt large language models to downstream tasks, but its effect on behavioral diversity in sequential decision-making remains…；Across state-level evaluation, arena gameplay, and training trajectories, we find that reasoning-mode generation frequently suppresses action diversity without uniformly…。

需要注意的边界包括：文中未明显展开局限性，阅读时建议重点核对数据覆盖范围、评测设置和泛化边界。；本次未成功抓取 PDF，方法与实验细节部分仍应以原文为准。。

与用户画像的关系：这篇论文和你当前画像里的方向有直接重合：生成（权重 0.10）。；如果你更看重系统性工作，可以重点看它如何组织任务设定、实验协议和整体框架。；若你当前关注生物或科学应用，建议额外留意论文中的任务场景、数据来源和落地边界。。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：这篇论文和你当前画像里的方向有直接重合：生成（权重 0.10）。

## 基本信息

- 作者：Junyi Sha, Renfei Tan, David Simchi-Levi
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-23
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：heuristic / PaperFlow template
- arXiv ID：`2607.19523`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 生成式精读补充本次未返回，当前内容仍按精读模板基于已拿到的摘要、元数据和可用 PDF 片段生成。
