---
user_id: "cheng tan"
paper_id: 6006
arxiv_id: "2607.26873v1"
title: "SERPO: Self-Evolving Rubric Policy Optimization for Open-Ended Test-Time Reinforcement Learning"
institution: "未在提供的信息中明确指出具体机构，但根据作者姓名推测为中国科研团队。"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.26873v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.26873v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:19:54"
---
# SERPO: Self-Evolving Rubric Policy Optimization for Open-Ended Test-Time Reinforcement Learning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：test-time reinforcement learning · open-ended generation · self-evolving rubrics · claim consensus

## 一句话总结

SERPO 提出了一种自演化评分细则策略优化框架，通过协同演化响应证据、查询特定的评分细则和策略参数，解决了开放式生成任务在测试时强化学习（TTRL）中缺乏标准答案和可靠奖励信号的难题。

## 摘要

> Test-time reinforcement learning (TTRL) enables language models to self-evolve at inference time without labeled feedback. Existing methods rely on answer voting and therefore do not extend naturally to open-ended generation, where valid responses cannot be mapped to a shared canonical answer. Without external reward models or stronger judges, adaptation must instead construct reliable rewards from the model's own outputs. We introduce SERPO (Self-Evolving Rubric Policy Optimization), which replaces answer voting with a closed loop that co-evolves response evidence, query-specific rubrics, and policy parameters. Good-Normal-Bad (G-N-B) response evolution organizes maximally separated rollouts into ordered archives; rubric evolution retains criteria that discriminate these archives; probabilistic criterion scoring converts verdict-token likelihoods into reward signals; and policy evolution optimizes the actor with the resulting signals. New actor rollouts then refresh both the archives and rubrics, closing the three-way evolution loop. Across two model configurations, two in-domain benchmarks, and four OOD benchmarks, SERPO improves HealthBench and ResearchQA by up to 20.63 and 20.31 points over the corresponding base models, raises the six-benchmark macro-average by up to 8.06 points, and supports OOD transfer and continued cross-benchmark evolution.

Q1: 这篇论文试图解决什么问题？

### 1. 测试时强化学习（TTRL）的局限性
测试时强化学习（TTRL）的核心在于利用模型在推理阶段生成的多个候选响应，通过某种形式的“自我一致性”（Self-Consistency）或“多数投票”来构建伪标签，进而微调模型。然而，这种范式在处理开放式生成（Open-Ended Generation）任务时面临根本性挑战：
- **缺乏标准答案（Canonical Answer）**：在数学或代码任务中，答案可以被精确匹配。但在健康咨询、研究问答等开放领域，有效的响应多种多样，无法通过简单的字符串匹配或数值比较来判断一致性。
- **奖励信号的稀疏与噪声**：现有的 TTRL 方法如果直接应用于开放式任务，往往会因为无法准确衡量响应质量而导致奖励信号充满噪声，甚至引发模型崩溃。

### 2. 外部奖励模型（RM）的依赖困境
虽然可以使用预训练的外部奖励模型或更强大的教师模型（如 GPT-4）作为评委，但这引入了新的问题：
- **静态性与分布偏移**：外部 RM 通常是静态的，无法适应测试时特定的分布变化。
- **成本与可访问性**：在推理阶段频繁调用大型外部模型会带来巨大的计算开销和延迟，且在某些隐私敏感或离线场景下不可行。

### 3. 核心科学问题
本文试图回答：在没有任何外部标注、没有标准答案、且不依赖更强外部评委的情况下，模型能否仅凭自身输出，通过构建一种动态的、可进化的评价机制，实现对开放式生成能力的自我提升？

Q2: 有哪些相关研究？

### 1. 测试时强化学习（TTRL）
TTRL 旨在使部署后的模型能够根据测试分布进行转导式学习（Transductive Learning）。早期工作如 ReST 和 Self-Correction 主要关注数学和逻辑推理，利用结果验证（Outcome Supervision）来引导学习。SERPO 将这一范式扩展到了缺乏明确验证器的开放式领域。

### 2. 自我改进与自我评判（Self-Correction & Self-Judging）
近年来，利用模型自身作为评委（LLM-as-a-Judge）的研究层出不穷。例如，Prometheus 和 Constitutional AI 探讨了如何利用预定义的原则或评分细则来引导模型。然而，这些方法通常使用固定的评分标准。SERPO 的创新之处在于“评分细则演化”（Rubric Evolution），即评分标准本身是根据当前模型的表现动态生成的。

### 3. 开放式生成的评估挑战
开放式生成的评估一直是一个难题。传统的 ROUGE 或 BLEU 指标无法捕捉语义深度。最近的研究倾向于使用 Claim-level 的验证。SERPO 吸收了这一思想，引入了“主张共识”（Claim-Consensus）目标，通过分解响应并寻找语义重叠来构建比整篇投票更细粒度的伪标签。

Q3: 论文如何解决这个问题？

### 1. 闭环演化框架（The Closed-Loop Framework）
SERPO 的核心是一个三位一体的演化循环，包含响应、评分细则和策略三个维度的同步更新：

#### A. 响应演化（Response Evolution）
- **采样与存档**：针对每个测试查询，模型采样多个响应。利用“主张共识”（Claim-Consensus）机制，将响应分解为独立的事实陈述，并计算不同响应间主张的重叠度。
- **G-N-B 组织**：根据共识得分，将响应组织成“优（Good）”、“良（Normal）”、“差（Bad）”三个存档。这种组织方式确保了训练数据具有最大的区分度。

#### B. 评分细则演化（Rubric Evolution）
- **动态准则生成**：模型被要求生成一组能够解释“为什么 G 存档的响应优于 B 存档”的评分准则。这些准则不是通用的，而是针对特定查询定制的。
- **准则筛选**：通过验证这些准则在当前存档上的区分能力，保留最具判别力的评分细则，剔除模糊或无效的准则。

#### C. 概率准则评分（Probabilistic Criterion Scoring）
- **软奖励信号**：不同于传统的 0/1 判定，SERPO 利用模型在判定 Token（如“Yes/No”）上的对数似然（Log-likelihood）来构建连续的奖励信号。这为策略优化提供了更平滑的梯度。

#### D. 策略演化（Policy Evolution）
- **优化算法**：使用生成的奖励信号，通过 PPO 或 DPO 等强化学习算法更新 Actor 模型的参数。更新后的模型将用于下一轮的采样，从而开启新的循环。

### 2. 技术创新点：主张共识（Claim-Consensus）
为了解决开放式任务中“投票”难的问题，SERPO 引入了基于判断的主张投票。它将响应分解为原子主张，并利用模型判断这些主张是否被其他响应支持。这种方法比整篇响应的投票更鲁棒，能捕捉到语义上的一致性而非字面匹配。

Q4: 论文做了哪些实验？

### 1. 实验设置
- **模型配置**：在两种不同规模的基础模型上进行了验证（具体模型名在摘要中未详述，通常为 Llama 或 Qwen 系列）。
- **基准测试**：
 - **领域内（ID）**：HealthBench（健康咨询）、ResearchQA（学术问答）。
 - **分布外（OOD）**：包含 4 个额外的 OOD 基准测试，涵盖科学、常识等领域。

### 2. 评估指标
- 采用标准性能指标（如准确率、F1 或专家评判得分）。
- 宏平均分（Macro-average）用于衡量跨任务的综合表现。

### 3. 对比基准（Baselines）
- **Base Model**：未经测试时优化的原始模型。
- **Response Voting**：传统的基于整篇响应投票的 TTRL。
- **External Judge**：使用外部强大模型（如 GPT-4）作为奖励来源的参考线。
- **Official Rubric**：使用人工编写的固定评分细则。

Q5: 发现了什么实验现象？

### 1. 性能大幅提升
- 在 HealthBench 上提升了 **20.63** 点，在 ResearchQA 上提升了 **20.31** 点。这证明了在缺乏标准答案的情况下，自演化评分细则的有效性。
- 六个基准测试的宏平均分提升了 **8.06** 点，显示了该方法的普适性。

### 2. 主张共识的优越性
- 实验显示，主张共识（Claim-Consensus）比传统的响应投票（Response Voting）性能提高了 **13%–28%**。这是因为主张分解能够找回被整篇投票忽略的语义一致性。

### 3. ID-OOD 反转现象（关键发现）
- **反直觉结果**：在 OOD 设置下，SERPO 的表现竟然超过了使用“外部评委+官方评分细则”的参考模型。虽然在领域内（ID）官方细则依然最强，但在面对未知分布时，模型自生成的动态细则展现了更强的迁移和适应能力。

### 4. 演化的持续性
- 实验观察到，随着演化循环的进行，模型性能呈现持续上升趋势，且能够支持跨基准的连续演化，没有出现明显的过拟合或性能退化。

Q6: 有什么可以进一步探索的点？

### 1. 评分细则的质量控制
目前评分细则由模型自生成，虽然有筛选机制，但仍可能存在幻觉或逻辑谬误。未来可以探索如何引入更强的逻辑约束或自洽性检查。

### 2. 计算开销优化
三位一体的演化循环涉及多次采样和推理，计算成本较高。如何实现更轻量级的演化，例如通过参数高效微调（PEFT）或更高效的采样策略，是实际部署的关键。

### 3. 多模态扩展
将 SERPO 框架扩展到多模态开放式任务（如视觉问答或多模态摘要），其中评分细则需要同时考虑文本和图像的一致性。

### 4. 长期记忆与知识遗忘
在持续演化过程中，如何确保模型在学习新分布知识的同时，不遗忘原有的通用能力，是一个值得研究的课题。

Q7: 总结一下论文的主要内容

本文提出了 SERPO，这是一种针对开放式生成任务的测试时强化学习（TTRL）新框架。传统的 TTRL 依赖于简单的一致性投票来产生奖励信号，这在处理如健康咨询或学术问答等没有标准答案的任务时显得力不从心。SERPO 的核心思想是“自演化”，它构建了一个闭环系统，让响应证据、评分细则（Rubrics）和模型策略三者协同进化。首先，它通过“主张共识”机制将模型生成的多个响应划分为优、良、差三个等级；接着，它引导模型生成能够区分这些等级的特定评分准则，并经过筛选保留高质量准则；随后，利用这些准则对响应进行概率化评分，产生连续的奖励信号；最后，通过强化学习算法优化模型。实验结果令人振奋，SERPO 在多个领域内和分布外基准测试中均取得了显著的性能提升，特别是在 OOD 场景下表现出了超越外部专家准则的适应性。这一工作为大语言模型在部署后的自主进化提供了一条无需人工干预、无需外部强模型辅助的新路径，具有重要的理论和应用价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文直接关联到生成（Generation）方向，特别是如何提升模型在开放式问答中的表现。

## 基本信息

- 作者：Jianze Wang, Kunwang Zheng, Ying Liu, Yu Cao, Qilong Zhang, Jinlong Chen, Hua Yang, Qianglong Chen
- 机构：未在提供的信息中明确指出具体机构，但根据作者姓名推测为中国科研团队。
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2607.26873v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，特别是关于 SERPO 框架的四个演化步骤、主张共识的改进效果以及 ID-OOD 反转现象的实验发现。
