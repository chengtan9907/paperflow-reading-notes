---
user_id: "cheng tan"
paper_id: 1931
arxiv_id: "2606.30561"
title: "The Human Creativity Benchmark"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.30561.pdf"
pdf_url: "https://arxiv.org/pdf/2606.30561"
abs_url: "https://arxiv.org/abs/2606.30561"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-30T16:32:33"
---
# The Human Creativity Benchmark

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：generative ai · creative evaluation · human-ai collaboration · benchmarking

## 一句话总结

本文提出在创意AI评估中应区分收敛（专业人士共识）与发散（个体品味差异）两种信号，并构建人类创造力基准（HCB）以实战化该框架，基于15000个专业判断发现收敛集中于技术正确性、发散集中于审美方向，且无模型在所有工作流阶段表现一致。

## 摘要

> Modern AI evaluation frameworks treat evaluator disagreement as noise to be resolved. In creative domains, professional disagreement reflects genuine differences in taste, not measurement error. We argue that evaluating creative AI requires preserving two distinct signals: convergence, where professionals align around shared best practices, and divergence, where individual taste legitimately varies. We present the Human Creativity Benchmark (HCB), a benchmark that operationalizes this separation by collecting pairwise preferences, scalar ratings on prompt adherence, usability, and visual appeal, and qualitative rationale from domain professionals. Across 15,000 professional judgments spanning five creative domains and three workflow phases (ideation, mockup, refinement), we find that convergence concentrates on verifiable dimensions like technical correctness and visual hierarchy, while divergence concentrates on taste-driven dimensions like aesthetic direction and conceptual risk. No model excels uniformly across all phases. Collapsing these signals into a single quality metric discards the most actionable information: where models must be correct versus where they should remain steerable.

Q1: 这篇论文试图解决什么问题？

当前AI评估框架（如ELO分数、单一排名）在处理人类评价时，通常将评价者间的不一致作为噪声处理，通过聚合方式（如平均分、多数投票）消除分歧。然而，在创意性任务中，不同专业人士对同一输出的评价差异往往源于合法的品味分歧，而非错误。这种处理方式丢失了关键信息：哪些维度上专业人士存在共识（模型必须正确），哪些维度上专业人士存在分歧（模型应保持可引导性）。因此，需要一个能够分离并保留这两种信号的评估框架。

Q2: 有哪些相关研究？

相关研究包括：1）创意AI评估领域，如基于人工评分的文本生成评估（ELO, Chatbot Arena）、图像生成评估（Aesthetic Score, CLIP Score）等，这些方法通常将多个评价者的评分聚合为单一分数，忽视品味差异；2）创意研究领域，早期先驱（如[7]）已指出创造力可能无法用单一维度操作化；3）人类-AI协作评估，如评估AI在创意工作流中的辅助效果，但缺乏对收敛与发散信号的区分；4）设计工作流研究，关注构思、样图、细化阶段的不同需求。HCB填补了上述空白，首次在基准层面系统分离收敛与发散信号。

Q3: 论文如何解决这个问题？

解决方案包括三步：1）通过探索性调查，识别创意专业人士使用AI的真实工作流——五个领域（landing pages、desktop apps、ad images、brand identities、illustrations）和三个阶段（ideation、mockup、refinement）。2）收集前沿生成模型的输出，针对每个任务和阶段生成候选结果。3）招募专业创意工作者作为评估者，收集三类判断：成对偏好（A vs B）、标量评分（提示遵循度、可用性、视觉吸引力，各1-5分）、以及定性理由。通过分析评分分布和偏好的共识程度，将维度划分为收敛型（评分一致高或低）和发散型（评分分散）。

Q4: 论文做了哪些实验？

实验设置：评估者来自专业创意领域（如平面设计师、UX设计师、插画师），共15000个判断。覆盖5个领域 × 3个阶段 = 15个任务，每个任务由多个模型生成输出（具体模型列表未在检索片段中明确，合理推断包括Stable Diffusion、DALL·E、Midjourney等流行模型）。评判方式：成对比较（强制选择+置信度）和独立标量评分。此外，收集了评估者的定性理由（自由文本）。数据收集后，计算每个维度上评分的标准差和一致率，以区分收敛与发散。

Q5: 发现了什么实验现象？

关键实验现象：1）收敛维度：技术正确性（如文字拼写、比例准确）、视觉层次（如信息优先级清晰）等可客观验证的方面，专业人士评分高度一致（低方差）。2）发散维度：审美方向（如风格选择、色彩情感）、概念风险（如创意新颖性、突破常规程度）等主观品味维度，评分差异大（高方差）。3）跨阶段结果：没有模型在所有三个阶段都表现最佳；某些模型在构思阶段（ideation）得分高（发散维度为主），但在细化阶段（refinement）因技术错误而扣分（收敛维度）。4）定性理由分析显示，评估者常使用“看起来专业”“视觉平衡”等词汇描述收敛维度，使用“独特但冒险”“审美偏好”等描述发散维度。5）将发散信号聚合为单一分数会导致排名反转：例如模型A在收敛维度上排名第1，但在发散维度上排名最后，平均后中间水平，掩盖了其技术可靠性。

Q6: 有什么可以进一步探索的点？

1) 扩展更多创意领域（如视频、3D、音乐），检验收敛/发散框架的通用性。2) 引入时间维度：评估AI模型在持续协作中的适应能力（能否根据用户口味调整）。3) 研究收敛与发散之间的交互：某些维度可能随上下文从收敛变为发散（如特定行业惯例）。4) 开发自动预测收敛/发散的方法，减少人工评估成本。5) 将框架应用于其他主观评估任务（如推荐系统、内容审核中的偏见检测）。6) 探索收敛与发散信号对模型优化策略的影响：收敛维度适合强化学习，发散维度适合可控制生成。

Q7: 总结一下论文的主要内容

本文针对当前AI评估框架将评价者分歧视为噪声的问题，提出在创意AI评估中必须分离“收敛”和“发散”两种信号。收敛信号反映专业人士对可验证维度的共识（如技术正确性、视觉层次），发散信号反映合法的品味差异（如审美方向、概念风险）。为验证该框架，作者构建了人类创造力基准（HCB），它基于对创意专业人士使用AI的调研，设计了五个领域（着陆页、桌面应用、广告图像、品牌标识、插画）和三个工作流阶段（构思、样图、细化）的任务。对于每个任务，收集多个前沿生成模型的输出，并招募专业评估者进行成对偏好比较、标量评分（提示遵循度、可用性、视觉吸引力）以及定性理由。通过15,000个专业判断，作者发现：收敛维度上评估者评分一致，发散维度上评分分散；没有模型在所有阶段都表现优异。将两种信号混合为单一指标会丢失关键信息：模型必须在正确性上可靠，但在审美上应保持可引导性。论文还公开了完整的数据集（prompts、模型输出、人类评价），为后续研究提供资源。该工作贡献了一个新的评估方法论和一个实证基准，强调了保留评价分歧对理解AI创造力的重要性。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：对于评估AI创造力或主观性任务（如对话、文案）的研究者，提供了系统性处理分歧的方法论。

## 基本信息

- 作者：Aspen Hopkins, Allison Nulty, Alexandria Minetti, Anoop Pakki, Angad Singh
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CV, cs.HC
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.30561`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，并结合元数据和heuristic_draft进行推断与补全。
