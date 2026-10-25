---
user_id: "cheng tan"
paper_id: 14445
arxiv_id: "2609.38426v1"
title: "LoopVL: Recurrent Visual Intelligence"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Sept 2026/2609.38426v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.38426v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-12T13:03:25"
---
# LoopVL: Recurrent Visual Intelligence

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：loop transformer · recurrent vision-language model · visual token evolution · visual aha moment

## 一句话总结

LoopVL 提出将 Loop Transformer 的循环计算范式扩展到视觉-语言模型：通过 Module-Loop 与 Model-Loop 两种共享模块循环方式，反复更新统一的多模态隐状态，使视觉 token 在循环中持续演化，从而在更少参数下取得接近或超越更大稠密非循环模型的多模态理解与视觉推理性能，并观察到跨循环视觉注意力突变的 Visual Aha Moments 现象。

## 摘要

> We introduce LoopVL to study whether Loop Transformers can be effectively extended to vision- language models. LoopVL combines Module-Loop and Model-Loop computation to iteratively update a unified vision-language state through shared modules. We train LoopVL from scratch through language pre-training, multimodal training, and post-training. LoopVL outperforms a range of similarly sized and larger non-recurrent models on multimodal understanding and visual reasoning benchmarks. We also observe Visual Aha Moments in LoopVL, characterized by pronounced shifts in visual attention across loops. LoopVL provides practical evidence for recurrent vision-language modeling and offers an intuitive perspective on how shared parameters can support deeper multimodal computation over continuously evolving visual-language states.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：循环 Transformer（Recurrent Transformer）已在纯语言模型中被证明可以用共享参数实现深计算，但在视觉-语言多模态场景中是否同样有效，目前缺乏系统性验证。具体而言，论文面对以下子问题：(1) 语言领域的循环深度范式（如 Universal Transformer、HRM-Text 等）能否自然迁移到视觉 token 与文本 token 共存的统一多模态状态上；(2) 视觉 token 在循环计算中是否会持续演化、如何演化，是否只是被“重复过一遍”而没有产生实质性的状态更新；(3) 循环共享参数带来的参数效率优势能否转化为真实的多模态基准性能提升，而不是以牺牲视觉细粒度理解、幻觉抑制、图表与数学推理为代价；(4) 循环计算在视觉-语言模型中是否会出现类似语言模型“顿悟（Aha Moment）”的可解释动态，即注意力在多次循环之间发生明显重分配，以及这种重分配是否对应更高层的证据聚合行为。论文还隐含回应一个工程问题：在多模态模型中，视觉 token 数量通常远大于文本 token，直接在视觉 token 上做多层循环会显著增加计算量，因此需要设计 token 压缩/筛选策略，而 LoopVL 选择保留“一个投影特征”而非逐 token 筛选，与 TokenPacker、FastV 等方法的取舍不同。

Q2: 有哪些相关研究？

相关工作主要分布在四条线上。(1) 循环 Transformer 的起源与复兴：Universal Transformer（Dehghani et al., 2019）最早提出共享参数的循环深度；后续工作（Csordás et al., 2024）证明循环结构可以在不按比例增加参数的前提下增加有效计算深度；最近出现循环深度语言模型（Geiping et al., 2025; Zhu et al., 2025）以及分层推理模型（Wang et al., 2025, 2026），这些模型展示共享参数可以随着隐状态演化执行不同计算。(2) 循环计算能力的理论支撑：一系列研究表明重复计算可以支持上下文学习、算法学习和复杂推理，使较浅的共享参数模型在特定任务上逼近更深的非共享架构（Giannou et al., 2023; Yang et al., 2024; Gatmiry et al., 2024; Saunshi et al., 2025）；循环架构已开始扩展到十亿参数规模的语言模型（Gao et al., 2026），并开始关注循环深度、层次组织和额外测试时计算的影响。(3) 视觉 token 压缩与视觉-语言模型中视觉特征处理：相关方法通过减少 token 数量来降低计算成本，但可能削弱局部细节，因此提出 locality-enhanced projector；FastV 则利用浅层注意力估计 token 重要性并提前移除视觉 token。LoopVL 的策略与这些方法不同：它保留一个投影后的视觉特征并让视觉 token 在整个循环计算中持续演化，而不是在早期层丢弃或压缩。(4) 多模态训练范式：LoopVL 的训练流程借鉴了现代 VLM 的三阶段方案（语言预训练、多模态继续预训练、指令/推理后训练），其文本侧骨干基于 HRM-Text 框架，多模态继续预训练使用 OneVision-1.5 Mid-Training-85M 等数据。整体上，现有循环 Transformer 工作几乎都集中在语言模型，视觉-语言循环建模缺乏系统实验，这正是本文填补的空白。

Q3: 论文如何解决这个问题？

LoopVL 的解决方案分为架构设计、训练流程与分析方法三部分。(1) 架构上，模型组合 Module-Loop 和 Model-Loop 两种计算。Module-Loop 指在模块内部多次循环执行共享层，Model-Loop 指在模型层面反复应用同一组共享模块；二者共同对统一的视觉-语言状态进行迭代更新。文本侧架构基于 HRM-Text 框架构建，视觉 token 与文本 token 在同一隐状态空间中共同演化。(2) 关键设计是让视觉 token 在整个循环过程中持续演化：编码器特征最初只描述单个图像块（patch-level 局部信息），经过多轮循环后逐步整合为与问题相关的全局证据。这与传统的“浅层融合后即固定”的 VLM 不同，也与 FastV 等在早期层删除视觉 token 的做法不同；LoopVL 保留一个投影特征并让视觉状态反复参与共享模块计算。(3) 训练流程：从零开始训练，依次经过语言预训练（Stage 1）、多模态训练（Stage 2）、后训练（Stage 3）。多模态阶段使用 OneVision-1.5 Mid-Training-85M 等数据，涵盖一般视觉问答、文本丰富图像、图表、数学与短答案任务；数据准备中包括基于问题文本的去重和长响应过滤（超出序列长度预算的文本丰富样本会被移除）。(4) 后训练聚焦数学推理等 reasoning-oriented 任务。为防止循环加深导致反向传播开销过大，默认使用 selective backpropagation（选择性反向传播）路线；作者还训练了一个去掉 visual anchor 和 query-conditioned visual gate 的对照模型，用于验证 Visual Aha Moments 现象不依赖这些机制。

Q4: 论文做了哪些实验？

论文在 16 个 benchmark family（展开 POPE 类别和 MMK12 科目后共 23 个评分行）上进行系统评估，覆盖通用理解、幻觉、数学和科学四类能力。主评测使用贪心生成，根据任务类型要求模型输出选项字母、yes/no 或不超过 32 token 的短答案；部分数学与推理任务在显式引导 chain-of-thought 时表现相当或略高，而在可直接作答的短答案任务上显式 CoT 反而降低准确率。评测工具使用 VLMEvalKit 和各 benchmark 官方代码，指标遵循官方定义（例如 ChartQA 使用 relaxed accuracy）。实验组织如下：5.1 节报告总体基准结果，将 LoopVL-1B 与一系列紧凑 VLM（可能包括同规模及更大模型）对比；5.2 节研究训练时循环配置（recurrence configurations）的影响；5.3 节研究推理时调度（inference-time schedules），即不同循环次数/调度策略对性能的影响；5.4 节分析 Visual Aha Moments，量化不同循环轮次间视觉注意力分布的突变；5.5 节研究跨循环 token 相关性（cross-loop token relevance），观察 token 重要性如何在循环间重新分配；5.6 节研究视觉注意力分配（visual-attention allocation），包括注意力质量、熵等空间分布指标；5.7 节研究视觉状态细化与答案形成（visual-state refinement and prediction formation），通过视觉 token 权重和归一化分布计算 attention mass 与 coverage，判断被关注的图像块是否包含回答问题所需的证据。此外还进行了机制消融：改变反向传播路径（selective backpropagation 与否）以及移除 visual anchor 和 query-conditioned visual gate 后重新从零预训练，检验 Visual Aha Moments 的稳健性。需要指出，受限的检索证据未能给出每个 benchmark 的具体数值、完整 baseline 列表和参数量对照表，这些细节需回原文 Table 2 等确认。

Q5: 发现了什么实验现象？

实验观察到的主要现象包括：(1) 总体性能上，LoopVL-1B 在多模态理解和视觉推理上强于多个规模相近甚至更大的非循环模型，同时在幻觉、图表理解和科学推理上保持竞争力；这说明循环共享参数带来的深度优势可以转化为实际多模态性能收益。(2) Visual Aha Moments：跨循环轮次之间视觉注意力出现显著且集中的迁移，这是论文的核心观察；该现象在默认 selective backpropagation 路线下成立，在更换反向传播路径后依然存在，甚至在移除 visual anchor 与 query-conditioned visual gate 后仍然存在，说明该现象并非由特定训练技巧或门控机制引入。(3) 注意力熵分析：第二个循环周期（second cycle）中注意力熵总体更低，表明注意力分布更集中、聚焦更明确；但熵并不是随模块调用单调下降，而是在模块调用之间波动。这暗示循环计算不是简单的“越算越聚焦”，而是先重定位再逐步巩固的交替过程。(4) 跨循环 token 相关性会发生重分配，即不同循环轮次中同一 token 的作用发生变化，支持“共享参数在不同状态上演化出不同功能”的观点。(5) 注意力分配与答案形成的关系：通过 attention mass 与 coverage 分析，被关注的图像块是否包含回答参考问题所需证据，决定了答案能否形成；这表明视觉状态细化与预测形成直接相关。(6) 调度敏感性：不同推理时调度对结果有影响，且训练时循环配置也会影响最终表现，说明循环深度与调度方式是敏感超参数。(7) 在受限后训练数据下，扩展推理偶尔会出现重复 token 或重复推理片段，打断推理过程，导致无法完成有效解答——这是循环推理模型当前的一个负结果/失败模式。

Q6: 有什么可以进一步探索的点？

可以进一步探索的方向包括：(1) 更广泛的后训练覆盖：在现有数学推理基础上扩展到科学推理、指令跟随和 agentic 任务求解，并大幅增加后训练数据的体量与多样性；引入更丰富的偏好与推理监督信号，提升生成稳定性、推理可靠性和通用能力。(2) 更通用的 Model-Loop 架构对照：当前由于计算限制，没有训练一个更通用 Model-Loop 架构的独立 VLM 作为匹配对照，无法完全确证 Visual Aha Moments 是否只出现在特定架构组合中；未来应做完整对照实验。(3) 更大规模的循环 VLM：论文提到需要研究“能够自然容纳视觉 token 计算”的循环框架并扩展到更大模型，即探索什么规模的循环结构能同时保持可扩展性和视觉 token 计算效率。(4) 视觉状态演化的机制解释：当前分析区分了调度敏感性、注意力集中度、跨循环相关性重分配、状态更新和最终读出对齐等维度，未来可以从理论或更细粒度机制上解释为什么循环能带来 Aha Moments，以及它和语言模型中的 reasoning Aha Moment 是否同源。(5) 视觉 token 压缩与保留策略的权衡：LoopVL 选择保留投影特征并全量演化，与 TokenPacker 类局部性增强压缩、FastV 类早期重要性筛选形成不同取舍；未来可研究在不同视觉 token 预算下循环更新的最优策略。(6) 测试时计算的进一步利用：推理时调度实验表明 test-time loop 数量可调，未来可探索自适应循环终止、按难度分配循环次数等动态调度。(7) 与智能体、AI for Science 等应用的结合：循环视觉状态更新机制可能适合需要多轮观察-推理交替的智能体任务，以及需要细粒度视觉证据迭代的科学图像理解任务。

Q7: 总结一下论文的主要内容

LoopVL 是第一个系统研究 Loop Transformer（循环 Transformer）能否有效迁移到视觉-语言模型的工作。论文的出发点是一个明确空白：尽管循环架构在语言模型领域已经复兴——从 Universal Transformer 到循环深度 LLM 再到分层推理模型——但已有研究几乎都集中在纯语言任务上，循环计算对视觉 token 演化的作用、对多模态理解的影响都没有被直接验证。LoopVL 的架构把 Module-Loop 与 Model-Loop 结合在一起：共享模块被反复施加到统一的视觉-语言隐状态上，使模型的有效计算深度远大于物理层数，而参数不按深度比例增长。与常见的 VLM 视觉 token 处理方式（如一次性投影后固定、或早期重要性筛选后删除）不同，LoopVL 让视觉 token 在每轮循环中持续参与计算并演化：编码器特征最初描述局部图像块，经过多轮共享模块处理后逐步形成与问题相关的全局证据。训练上，LoopVL 从零开始，顺序经过语言预训练、多模态预训练（使用 OneVision-1.5 Mid-Training-85M 等数据，包含一般视觉问答、文本丰富图像、图表、数学和短答案任务）和后训练（重点为数学推理），数据准备包含问题文本去重和长响应过滤。评测上，模型在 16 个 benchmark family、展开后 23 个评分行上对比了多组紧凑 VLM，结果显示 LoopVL-1B 在多模态理解和视觉推理上超过规模相近甚至更大的非循环模型，在幻觉、图表理解和科学推理上也保持竞争力；显式 CoT 在数学与推理任务上略有帮助或持平，但在可直接作答的短答案任务上反而降低准确率。机制分析是论文的另一主线：论文定义了 Visual Aha Moments 现象——跨循环轮次间视觉注意力发生明显迁移与集中；通过改变反向传播路径（selective backpropagation）和移除 visual anchor/query-conditioned visual gate 的从零预训练对照，证明该现象对训练细节稳健；注意力熵分析显示第二循环周期熵更低但非单调下降；attention mass 与 coverage 分析把注意力分配与答案证据是否充分联系起来。局限方面，作者明确承认后训练数据量有限导致扩展推理偶尔产生重复 token/重复推理片段；没有训练更通用 Model-Loop 架构的匹配对照模型；视觉 token 计算如何随模型规模扩展仍需研究。整体而言，论文同时提供了架构设计、三阶段训练配方、跨 16 个 benchmark 家族的实证结果和一套循环注意力分析工具，主张“循环即更深的多模态计算”这一范式判断。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：论文与你画像中的智能体（权重 0.10）、AI for Science（权重 0.10）、生成（权重 0.10）三个方向直接相关：循环视觉状态更新适合多轮观察-推理交替的 agentic 任务，视觉证据迭代聚合适合科学图像理解，循环生成框架是生成模型的新兴方向。

## 基本信息

- 作者：Zhe Qian, Ziyang Gong, Zhongxing Xu, Hehan Li, Zhonghua Wang, Fei Luo, Mingxuan Wang, Xue Yang, Shiwei liu, Yanbiao Ma, Junchi Yan, Jungong Han
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-09-29
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.38426v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了检索到的 81 个 PDF 语义片段（覆盖 abstract、introduction、results、conclusion），并优先采用 field_evidence_map 对应的证据锚点；由于片段多为截断文本，具体数值与完整 baseline 信息缺口已在对应字段标出。
