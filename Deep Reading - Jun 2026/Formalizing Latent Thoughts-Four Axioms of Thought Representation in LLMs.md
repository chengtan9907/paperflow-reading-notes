---
user_id: "cheng tan"
paper_id: 542
arxiv_id: "2606.27378"
title: "Formalizing Latent Thoughts: Four Axioms of Thought Representation in LLMs"
institution: "Fard Lab (University of British Columbia / Okanagan)"
publish_date: "2026-06-29"
pdf_path: "/Users/mario/Downloads/projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.27378.pdf"
pdf_url: "https://arxiv.org/pdf/2606.27378"
abs_url: "https://arxiv.org/abs/2606.27378"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-29T15:19:30"
---
# Formalizing Latent Thoughts: Four Axioms of Thought Representation in LLMs

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：latent thought representation · large language models · axiomatic evaluation · reasoning

## 一句话总结

本文提出了一个由因果性、最小性、可分离性和稳定性四个公理组成的公理化评估框架，用于独立于下游任务准确率地衡量大语言模型（LLM）中潜在思维表示（Latent Thought Representations）的质量。

## 摘要

> We introduce an axiomatic evaluation framework for latent thought representationsMay in LLMs, comprising metrics that are independent of downstream benchmark
> 7 scores and reveal representational failures that benchmark accuracy masks. Existing
> evaluations conflate representation quality with model capacity. Therefore, failures
> cannot be attributed to the representation rather than to the model that processes
> it. We formalize four functional axioms (Causality, Minimality, Separability, and
> Stability) and define a quantitative measure for each, computed directly on the
> representation independently of downstream accuracy. We audit open-weight
> LLMs across 23 reasoning tasks (e.g., Spatial Reasoning, Factual QA). We find[cs.CL]
> that no candidate satisfies all four axioms simultaneously, that the representations
> distinguish task type reliably but cannot distinguish between two questions within
> the same task, and that the representations encode little information beyond what
> is already present in the input embedding. The failure is consistent across dense,
> reasoning-distilled, and RL-trained model families, indicating that the gap is
> structural rather than a property of model size or training procedure. Code: https:
> //fard-lab.github.io/formalize-thoughts.

Q1: 这篇论文试图解决什么问题？

### 核心挑战：表示质量与模型能力的混淆
目前的 LLM 推理研究正逐渐从显式的离散 Token（如 CoT）转向连续的潜在向量表示。然而，评估这些“潜在思维”的方法存在严重缺陷：
1. **下游指标的误导性**：现有的评估几乎完全依赖于下游任务的准确率。如果模型在某个任务上表现不佳，无法判断是因为内部思维表示（Thought Representation, TR）本身质量差，还是因为后续的解码/处理层能力不足。
2. **缺乏功能性定义**：目前缺乏对“什么是有效的思维表示”的形式化定义。现有的优化目标多为启发式代理（如步骤计数、Token 预算或对显式 CoT 的模仿），而非基于功能性要求。
3. **表示坍缩的隐患**：先前的探测研究发现，不同的推理路径在早期层可能会坍缩为单一解释，但下游准确率却保持不变。这表明准确率掩盖了表示层的失效。

### 论文试图解决的具体问题
- 如何在不依赖下游任务得分的情况下，独立衡量潜在思维表示的内在质量？
- 一个有效的思维表示必须具备哪些功能属性？
- 现有的主流 LLM（包括经过强化学习训练的推理模型）在这些属性上的表现究竟如何？

Q2: 有哪些相关研究？

### 相关研究脉络与对比
1. **连续推理表示 (Continuous Reasoning Representations)**：
 - 早期工作如 *Implicit CoT* [66] 和 *Thought Propagation* [27] 尝试将推理步骤压缩进向量空间。
 - 近期研究如 *Coconut* [31] 和 *Latent Reasoning* [9, 81] 报告了在复杂基准上的准确率提升，但其评估仍局限于下游任务。
2. **表示探测与解释性 (Probing & Interpretability)**：
 - 现有研究 [57] 发现推理 Token 在模型内部会发生坍缩，这为本文提出的“可分离性”公理提供了动机。
 - 传统的探测方法通常需要训练额外的分类器，而本文的框架旨在直接在源模型上运行，无需重新训练。
3. **信息论评估 (Information-Theoretic Evaluation)**：
 - 本文引入了信息瓶颈（Information Bottleneck, IB）原则来衡量“最小性”，这与之前在深度学习表示学习中的理论研究相呼应，但首次将其应用于 LLM 的潜在思维评估。

Q3: 论文如何解决这个问题？

### 四大功能公理及其量化指标
本文提出了一个无需重新训练、直接作用于源 LLM 的评估协议：

1. **因果性 (Causality)**：
 - **定义**：思维表示必须是生成后续输出的因果驱动力。
 - **度量**：计算使用候选 TR 诱导的输出分布与源模型原始输出分布之间的 KL 散度 ($D_{KL}$)。散度越低，因果一致性越强。

2. **最小性 (Minimality)**：
 - **定义**：表示应仅保留预测输出所需的输入信息，剔除无关噪声。
 - **度量**：基于信息瓶颈（IB）残差 $\Delta IB$。通过测量表示对输出的预测能力与对输入信息的压缩程度之间的平衡来评分。

3. **可分离性 (Separability)**：
 - **定义**：对于不同的输入问题，表示应当是可区分的。
 - **度量**：计算同一任务内不同问题的表示之间的余弦相似度或欧氏距离。如果不同问题的表示高度重合，则发生了“表示坍缩”。

4. **稳定性 (Stability)**：
 - **定义**：对于语义相同但表述不同的输入（Paraphrase），表示应保持一致。
 - **度量**：测量输入发生微小扰动或改写时，表示空间的变动程度。

### 实验设置
- **模型覆盖**：Llama-3 (8B/70B)、DeepSeek-R1 (32B)、Sky-OR1 (32B) 等，涵盖了密集模型、MoE、推理蒸馏和 RL 训练模型。
- **任务集**：使用 BBEH (Big-Bench Hard) 中的 23 个推理任务，包括空间推理、事实问答等。

Q4: 论文做了哪些实验？

### 实验设计与基准对比
1. **候选表示 (Candidate TRs)**：
 - **OE (Output Embedding)**：最后一层的隐藏状态。
 - **IE (Input Embedding)**：输入的初始嵌入（作为信息基准）。
 - **RV (Random Vector)**：随机向量（作为无信息基准）。
 - **LIT/ST/STN/LT**：各种迭代思考（Iterative Thinking）产生的中间表示。

2. **评估协议**：
 - 在 23 个任务上分别计算四个公理的得分。
 - 使用自助法（Bootstrap）进行显著性检验。
 - 重点对比 RL 训练模型（如 DeepSeek-R1）与普通密集模型在表示质量上的差异。

3. **任务类型**：
 - 涵盖逻辑推理、常识推理、数学和语言理解，以验证公理的通用性。

Q5: 发现了什么实验现象？

### 关键实验发现与反直觉结果
1. **普遍的表示坍缩 (Representational Collapse)**：
 - **现象**：模型在下游任务上可能得分很高，但其思维表示在“可分离性”上表现极差。具体表现为：表示能够可靠地识别任务类型（例如知道这是个数学题），但**无法区分同一任务下的两个具体问题**。
 - **结论**：这意味着模型在处理具体实例前，表示已经过早地收敛到了任务模板上。

2. **因果性缺失**：
 - 实验发现，没有任何一种候选思维表示能持续超越输入嵌入（IE）的因果贡献。这表明目前的潜在思维表示并没有比原始输入提供更多的因果信息。

3. **RL 训练并未改变结构性缺陷**：
 - 尽管 DeepSeek-R1 等模型通过强化学习显著提升了推理准确率，但在公理化评估中，它们的表示依然表现出与普通模型相似的失效模式（如最小性不足、可分离性差）。这说明**推理能力的提升可能更多来自于模型对表示的利用能力，而非表示本身质量的根本改变**。

4. **指标间的张力**：
 - 观察到“因果性”与“最小性”之间存在权衡。为了获得更高的因果驱动力，表示往往保留了过多的输入噪声，违反了最小性原则。

Q6: 有什么可以进一步探索的点？

### 可探索的研究方向
1. **新型表示构造**：目前的潜在思维表示（如软思维、隐藏状态）均未能通过公理测试，需要探索能够更好平衡因果性与最小性的新构造方法。
2. **公理化训练目标**：将这四个公理直接作为损失函数的一部分，引导模型在训练阶段生成更高质量的思维表示，而非仅仅优化预测准确率。
3. **动态表示深度**：研究模型是否可以根据任务难度动态调整表示的复杂度，以满足不同场景下的公理要求。
4. **跨模态扩展**：将该框架扩展到多模态模型中，评估视觉-语言推理中的潜在表示质量。

Q7: 总结一下论文的主要内容

本文针对大语言模型中日益流行的“潜在思维表示”提出了首个系统性的公理化评估框架。作者指出，现有的基于下游任务准确率的评估方法存在严重的“黑盒”问题，无法揭示模型内部表示的真实质量。为此，论文定义了因果性、最小性、可分离性和稳定性四个核心公理，并设计了相应的量化度量指标。

通过对 Llama-3、DeepSeek-R1 等多种主流模型的深度审计，研究揭示了一个令人担忧的现状：现有的潜在思维表示在功能上是不完整的。最显著的问题是“表示坍缩”，即模型内部无法有效区分同类任务下的不同实例，且这些表示相对于原始输入嵌入并没有提供显著的额外增益。即使是经过专门推理强化的 RL 模型，在这些底层表示属性上也未表现出质的飞跃。这一发现挑战了当前“连续向量可以完美替代离散 CoT”的假设，为未来开发更透明、更高效的推理模型指明了方向：我们需要关注表示本身的结构化质量，而不仅仅是最终的输出分数。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：对于关注 LLM 内部推理机制（Chain-of-Thought vs. Latent Thought）的研究者具有极高参考价值。

## 基本信息

- 作者：Fahd Seddik, Fatemeh Fard
- 机构：Fard Lab (University of British Columbia / Okanagan)
- 来源：arxiv
- 主题/分类：cs.CL, cs.LG
- 日期：2026-06-29
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2606.27378`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，重点提取了论文提出的四个公理定义、实验中的表示坍缩发现以及对 DeepSeek-R1 等模型的审计结果。
