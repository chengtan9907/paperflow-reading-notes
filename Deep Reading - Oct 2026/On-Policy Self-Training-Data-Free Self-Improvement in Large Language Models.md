---
user_id: "cheng tan"
paper_id: 17933
arxiv_id: "2610.32770"
title: "On-Policy Self-Training: Data-Free Self-Improvement in Large Language Models"
publish_date: "2026-10-26"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.32770.pdf"
pdf_url: "https://arxiv.org/pdf/2610.32770"
abs_url: "https://arxiv.org/abs/2610.32770"
generation_provider: "heuristic"
generation_model: "PaperFlow template"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-26T12:14:05"
---
# On-Policy Self-Training: Data-Free Self-Improvement in Large Language Models

> ★★★★☆ 推荐阅读 · 模型 heuristic/PaperFlow template

🏷 关键词：on-policy self-distillation · question-answer pairs · achieving consistent · across multiple · across text · domains · without · capabilities

## 一句话总结

We find that self-distillation on fully synthetic privileged information enables a model to improve across multiple domains while retaining its capabilities on domains it is not…

## 摘要

> On-policy self-distillation enables post-training foundation models without relying on stronger external teachers. However, existing approaches rely on ground-truth question-answer pairs, human-curated problem sets, or other supervised signals. In this work, we reformulate on-policy self-distillation as a self-training framework, driven by self-generated question-answer pairs and privileged reasoning demonstrations. We push supervision to the limit: guided solely by coarse, high-level topic descriptions, the model synthesizes its own curriculum of queries and privileged reasoning traces without access to any human-curated prompts or answers. We find that self-distillation on fully synthetic privileged information enables a model to improve across multiple domains while retaining its capabilities on domains it is not self-trained on. We demonstrate the generality of this framework across text and multimodal domains, achieving consistent self-improvement on standard language benchmarks and Visual Question Answering. Our findings demonstrate that foundation models possess rich latent capabilities that can be amplified without external task supervision or curated problem distributions.

Q1: 这篇论文试图解决什么问题？

removes the need for a stronger teacher by constructing teacher and student from the same parameters under asymmetric conditioning (Zhao et al., 2026a; Shenfeld et al., 2026a).

Q2: 有哪些相关研究？

当前自动解析没有稳定提取出 Related Work 的完整脉络。可先从论文引言、相关工作章节和引用线索核对它主要对比了哪些方法。

从当前证据可见，论文的定位至少包括：ication first estimates whether a sample is well-formed, and the consensus gate then approximates whether it is informative for the current student.；Pseudo-Verification.；Even without any MMLU topics, MMLU improves by 5.1 points, so part of the gain is not specific to the seeded domain.。

Q3: 论文如何解决这个问题？

ication first estimates whether a sample is well-formed, and the consensus gate then approximates whether it is informative for the current student. Pseudo-Verification.

Q4: 论文做了哪些实验？

Even without any MMLU topics, MMLU improves by 5.1 points, so part of the gain is not specific to the seeded domain. ‡Our re-implementation.

Q5: 发现了什么实验现象？

Even without any MMLU topics, MMLU improves by 5.1 points, so part of the gain is not specific to the seeded domain. ‡Our re-implementation.

Q6: 有什么可以进一步探索的点？

- Limitations.
- We introduced On-Policy Self-Training (OPST), which post-trains a model on self-generated problems and privileged information, seeded only by coarse topic descriptors.
- 优先核对语义命中的全文片段：Method / Introduction。
- 先看 Introduction，确认论文到底在补哪一块空白。
- 再看 Method / Approach，把核心模块、训练目标和输入输出关系理清。
- 重点看 Experiments / Results，确认提升来自哪里，以及 baseline 是否公平。

Q7: 总结一下论文的主要内容

We find that self-distillation on fully synthetic privileged information enables a model to improve across multiple domains while retaining its capabilities on domains it is not…

removes the need for a stronger teacher by constructing teacher and student from the same parameters under asymmetric conditioning (Zhao et al., 2026a; Shenfeld et al., 2026a).

ication first estimates whether a sample is well-formed, and the consensus gate then approximates whether it is informative for the current student. Pseudo-Verification.

Even without any MMLU topics, MMLU improves by 5.1 points, so part of the gain is not specific to the seeded domain. ‡Our re-implementation.

主要贡献包括：ication first estimates whether a sample is well-formed, and the consensus gate then approximates whether it is informative for the current student.；Pseudo-Verification.；Even without any MMLU topics, MMLU improves by 5.1 points, so part of the gain is not specific to the seeded domain.。

需要注意的边界包括：Limitations.；We introduced On-Policy Self-Training (OPST), which post-trains a model on self-generated problems and privileged information, seeded only by coarse topic descriptors.。

与用户画像的关系：从全文语义检索命中的片段看，相关信息主要落在 Method / Introduction 部分。；这篇论文和你当前画像里的方向有直接重合：生成（权重 0.10）。；如果你更看重系统性工作，可以重点看它如何组织任务设定、实验协议和整体框架。。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在 Method / Introduction 部分。

## 基本信息

- 作者：Leon Sick, Lukas Hoyer, Yannick Strümpler, Christian Reisswig, Timo Ropinski, Federico Tombari
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.LG
- 日期：2026-10-26
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：heuristic / PaperFlow template
- arXiv ID：`2610.32770`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 生成式精读补充本次未返回，当前内容仍按精读模板基于已拿到的摘要、元数据和可用 PDF 片段生成。
