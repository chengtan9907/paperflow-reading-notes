---
user_id: "cheng tan"
paper_id: 18878
arxiv_id: "2610.37471"
title: "PerceptRL: Making Visual Perception Rewardable in MLLMs from Answers to Interactions"
publish_date: "2026-10-28"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.37471.pdf"
pdf_url: "https://arxiv.org/pdf/2610.37471"
abs_url: "https://arxiv.org/abs/2610.37471"
generation_provider: "heuristic"
generation_model: "PaperFlow template"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-28T11:38:54"
---
# PerceptRL: Making Visual Perception Rewardable in MLLMs from Answers to Interactions

> ★★★★☆ 推荐阅读 · 模型 heuristic/PaperFlow template

🏷 关键词：during post-training · perception agentic · perceptual errors · single-turn perception · visual perception · pipeline · tools · captions

## 一句话总结

—Fine-grained visual perception remains a bottleneck during post-training of multimodal large language models (MLLMs), where models first perceive images and then reason over the…

## 摘要

> —Fine-grained visual perception remains a bottleneck during post-training of multimodal large language models (MLLMs),
> where models first perceive images and then reason over the extracted information, so perceptual errors can limit the gains from
> reasoning-focused optimization. Although extensive image-caption pre-training improves perception, the influence of this supervision
> can be diluted as later training stages change the data mixture and objectives. Continuing to optimize captions during post-training is a
> natural response, but their subjective, information-dense nature makes verifiable reward design difficult. To address this, we present
> PerceptRL to strengthen perception during post-training at two complementary levels. Perception RL uses a multi-agent pipeline to
> extract verified error items from dense image captions and compose the corresponding visual facts into rewardable QA pairs: chain QA
> links correlated errors, while multiple-choice QA combines unrelated errors. A specialized route targets high-error domains with tailored
> verification, while a general route addresses perceptual errors beyond these domains through a domain-agnostic pipeline. This2026 error-driven construction provides fixed answer labels and rule-based rewards, making caption-derived perceptual supervision
> verifiable. Perception Agentic RL targets fine-grained and dense cases that single-turn Perception RL still fails to resolve, enabling
> multi-turn inspection with visual tools. We observed coordinate drift even in models trained to invoke tools: incorrect crops orOct annotations can distort later evidence and propagate errors. Coordinate Injection corrects spatial operations in cold-start trajectories,
> while a grounding-aware process reward supervises intermediate spatial fidelity alongside final-answer correctness. Training proceeds
> through shared cold-start SFT, single-turn Perception RL, and multi-turn Perception Agentic RL. Across seven benchmarks under a27
> shared tool-use protocol, the full pipeline achieves 71.6% average accuracy, exceeding the base model with the same tools and the
> strongest evaluated frontier VLM by 15.7 and 5.2 percentage points, respectively.

Q1: 这篇论文试图解决什么问题？

age on RealWorldCounting 14,109 9,783 Counting. Performance remains task-dependent: Gemini-3Color 3,710 377 Specialized Spatial 3,965 1,150 Flash leads on MMStar, BLINK, and ERQA.

Q2: 有哪些相关研究？

当前自动解析没有稳定提取出 Related Work 的完整脉络。可先从论文引言、相关工作章节和引用线索核对它主要对比了哪些方法。

从当前证据可见，论文的定位至少包括：hich the reward then scores;；no pre-collected trajectory is scale each coordinate by the corresponding image diused as a supervision target.；corresponding block.。

Q3: 论文如何解决这个问题？

hich the reward then scores; no pre-collected trajectory is scale each coordinate by the corresponding image diused as a supervision target.

Q4: 论文做了哪些实验？

corresponding block. We also update the #target records 1 to contain (b∗t,k, dt,k) and retain them as a comment on the ¯G = X Gt.

Q5: 发现了什么实验现象？

corresponding block. We also update the #target records 1 to contain (b∗t,k, dt,k) and retain them as a comment on the ¯G = X Gt.

Q6: 有什么可以进一步探索的点？

- onal caption errors.
- This format produces questions with unambiguous over the categories above.
- 优先核对语义命中的全文片段：Introduction。
- 先看 Introduction，确认论文到底在补哪一块空白。

Q7: 总结一下论文的主要内容

—Fine-grained visual perception remains a bottleneck during post-training of multimodal large language models (MLLMs), where models first perceive images and then reason over the…

age on RealWorldCounting 14,109 9,783 Counting. Performance remains task-dependent: Gemini-3Color 3,710 377 Specialized Spatial 3,965 1,150 Flash leads on MMStar, BLINK, and ERQA.

hich the reward then scores; no pre-collected trajectory is scale each coordinate by the corresponding image diused as a supervision target.

corresponding block. We also update the #target records 1 to contain (b∗t,k, dt,k) and retain them as a comment on the ¯G = X Gt.

主要贡献包括：hich the reward then scores;；no pre-collected trajectory is scale each coordinate by the corresponding image diused as a supervision target.；corresponding block.。

需要注意的边界包括：onal caption errors.；This format produces questions with unambiguous over the categories above.。

与用户画像的关系：从全文语义检索命中的片段看，相关信息主要落在 Introduction 部分。；这篇论文和你当前画像里的方向有直接重合：智能体（权重 0.10）。；如果你更看重系统性工作，可以重点看它如何组织任务设定、实验协议和整体框架。。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在 Introduction 部分。

## 基本信息

- 作者：Tongkun Guan, Zhibo Yang, Mingkun Yang, Haocheng Wang, Wei Shen, Xiaokang Yang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-10-28
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：heuristic / PaperFlow template
- arXiv ID：`2610.37471`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 生成式精读补充本次未返回，当前内容仍按精读模板基于已拿到的摘要、元数据和可用 PDF 片段生成。
