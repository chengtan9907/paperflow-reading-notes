---
user_id: "cheng tan"
paper_id: 8
arxiv_id: "2606.14694v1"
title: "AdaSR: Adaptive Streaming Reasoning with Hierarchical Relative Policy Optimization"
publish_date: "2026-06-12"
pdf_path: "/Users/mario/Downloads/projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.14694v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.14694v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
report_version: "2026-06-04-v5"
daily_note_path: "/Users/mario/Downloads/projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-15T11:50:04"
---
[[Daily Note - Jun 2026]]

- PDF：[https://arxiv.org/pdf/2606.14694v1](https://arxiv.org/pdf/2606.14694v1)

# AdaSR: Adaptive Streaming Reasoning with Hierarchical Relative Policy Optimization

- PDF: https://arxiv.org/pdf/2606.14694v1
- 原文: https://arxiv.org/abs/2606.14694v1

> ★★☆☆☆ 按需阅读 · 约 20 分钟 · 模型 openai-compatible/gemini-3-flash-preview · 证据 PDF 全文 + 元数据

🏷 关键词：streaming reasoning · hierarchical reinforcement learning · policy optimization · adaptive computation

## 一句话总结

AdaSR 提出了一种自适应流式推理框架，通过分层相对策略优化（HRPO）使模型能够在输入流过程中动态分配计算资源并进行实时推理。

## 摘要

> 大型推理模型通常遵循“先读后想”的范式，即在观察到完整输入后才开始推理。然而，在音频、视频流或交互式智能体等现实场景中，信息是持续到达的。AdaSR 框架打破了这一限制，允许模型在输入流进行时同步推理，并在流结束时进行最终审议。该框架的核心是分层相对策略优化（HRPO），它解决了现有流式方法过度依赖预设轨迹监督模仿的问题。HRPO 能够优化模型“何时思考”以及在不同阶段分配多少计算量，从而在推理准确性和延迟之间取得更好的平衡。实验表明，AdaSR 在动态环境下表现出更强的灵活性和更高的推理效率。

Q1: 这篇论文试图解决什么问题？

该论文试图解决传统大语言模型在处理动态流式输入时的局限性。主要问题包括：
1. 范式冲突：现有的“先读后想”（Read-then-think）范式要求模型在获取完整上下文后才开始推理，这在实时流（如语音、视频、传感器数据）中会导致不可接受的延迟。
2. 推理时机矛盾：过早推理可能因信息不足导致误导性结论，而过晚推理则浪费了处理时间。模型需要具备判断何时介入推理的能力。
3. 现有方法的灵活性不足：目前的流式推理方法大多依赖于对预构建轨迹的监督模仿学习（SFT），这限制了模型在面对未见过的复杂推理路径时的泛化能力和自适应调整空间。
4. 计算资源分配：在流式输入的不同阶段，如何科学地分配计算资源（Token 生成量）以平衡局部理解与全局决策，是一个尚未解决的挑战。

Q2: 有哪些相关研究？

相关研究主要分为以下几类：
1. 推理模型范式：以 Wei 等人（2022）为代表的思维链（CoT）研究，确立了静态上下文下的推理基础。
2. 流式处理与实时系统：包括 Gu 等人（2017）和 Ma 等人（2018）在同声传译和实时视频分析领域的工作，这些研究强调了在部分观察下做出反应的重要性。
3. 强化学习与策略优化：如 PPO 等算法在对齐模型行为方面的应用。AdaSR 借鉴了这些思路，但针对流式推理的分层特性进行了改进。
4. 现有流式推理尝试：如 StreamingThinker（Tong 等人，2025a），虽然初步实现了边读边想，但仍受限于监督学习的框架，缺乏策略层面的动态优化。

Q3: 论文如何解决这个问题？

AdaSR 框架通过以下核心技术实现自适应流式推理：
1. AdaSR 架构：设计了一个支持在输入流过程中生成中间推理步骤（Local Reasoning）并在流结束后进行最终总结（Final Deliberation）的架构。模型可以根据当前已接收到的片段自主决定是否进行推理。
2. 分层相对策略优化（HRPO）：这是本文的核心算法。HRPO 将推理过程分解为多个层次，通过引入相对奖励机制，优化模型在每个流片段后的思考深度。它包含了解耦裁剪（Decoupled Clipping）和动态采样技术，以处理流式注意力机制下的概率偏移。
3. 动态计算分配：通过 HRPO 学习一个策略，使模型能够根据输入信息的复杂度动态调整生成的 Token 数量，从而在保证准确率的前提下最小化冗余计算。
4. 训练机制：利用强化学习（RL）而非单纯的 SFT，使模型能够在探索中发现更优的推理时机和路径，增强了对动态环境的适应性。

Q4: 论文做了哪些实验？

论文进行了多维度的实验验证：
1. 数据集：主要在 GSM-symbolic (P2/P1) 和 MetaMathQA 等数学推理数据集上进行测试，这些数据集被改造为流式输入格式。
2. 实验设置：模拟真实阅读速度（如 150 词/分钟），评估模型在不同输入流速下的表现。
3. 基准对比：将 AdaSR 与传统的 SFT 流式模型以及非流式的静态推理模型进行对比。
4. 关键指标：评估指标包括推理准确率（Accuracy）、总生成长度（Total Generation Length）以及残余延迟（Residual Latency）。
5. 消融实验：研究了 HRPO 中 λ 参数（控制局部与全局平衡）对结果的影响，验证了分层优化和解耦裁剪技术的有效性。结果显示 AdaSR 在保持高准确率的同时，显著降低了流结束后的等待时间。

Q5: 有什么可以进一步探索的点？

尽管 AdaSR 取得了进展，但仍有以下方向值得探索：
1. 多模态扩展：将 AdaSR 应用于更复杂的视频流或多模态交互场景，处理视觉和音频信息的同步推理。
2. 极端流速适应：研究在极快或极慢流速下，模型如何保持推理逻辑的连贯性。
3. 奖励函数细化：目前主要依赖最终答案的正确性，未来可以引入更细粒度的过程奖励（Process-based Reward），以进一步优化中间推理步骤的质量。
4. 硬件加速与部署：针对流式推理的特殊注意力模式，优化 KV 缓存管理和推理引擎，以实现真正的低延迟部署。

Q6: 总结一下论文的主要内容

这篇论文介绍了 AdaSR，一种旨在解决动态流式环境下大语言模型推理效率问题的框架。传统的“先读后想”模式在处理实时信息流时存在高延迟和灵活性差的问题。AdaSR 允许模型在接收输入流的同时进行思考，并通过新颖的 HRPO 算法优化推理时机和计算量分配。HRPO 通过分层强化学习解决了流式推理中策略偏移和奖励分配的难题。实验结果证明，AdaSR 能够根据输入信息的节奏自适应地调整推理策略，在 GSM-symbolic 等任务上实现了准确率与延迟的更优权衡。该工作为构建能够实时响应、边听边想的智能体提供了重要的技术支撑。

## PDF 证据定位

- 研究背景：
  Abstract | score=1.168 | r-driven systems must they largely rely on supervised imitation of preconstructed trajectories, which limits their flexreact before an event is fully observed (Gu et al.…
  Abstract | score=1.139 | costly, encourage latency-aware computation allocasince each partial observation may require a cortion. Experiments show that AdaSR achieves responding local reasoning…
  Abstract | score=1.117 | rchican reasoning models think, update, and respond cal reasoning process, we introduce Hierarchical Relative Policy Optimization (HRPO), under continuously evolving…
- 核心方法：
  References | score=0.941 | cy, the reference policy, checkpointing intact, while changing only the parts and the updated actor. A subtle issue is that the that define rollout semantics and token…
  Abstract | score=0.926 | ing (Wei et al., 2022): they receive an input, generate intermediate reasoning steps, and finally proLarge reasoning models typically follow a readduce an answer. This paradigm…
  References | score=0.886 | s under partial observations, ception loss and entropy regularization, enabling and performs final deliberation after the stream GRPOor DAPO-style training to better support…
- 主要结果：
  References | score=0.878 | λ controls the local-global balance in HRPO. Rows are grouped by ablation theme, and evaluation reports accuracy and total generation length on GSM-symbolic P2/P1 and MetaMathQA.…
  References | score=0.863 | under streamtroducing decoupled clipping and dynamic saming observations (Yao et al., 2023; Hu and Clune, pling, together with practical techniques such as 2023; Gim et al.…
  References | score=0.863 | ed credit assignment problem, assigning advantages over streaming, deep-reasoning, and global token ranges to better optimize adaptive reasoning under partial and evolving…
- 局限性：
  References | score=0.860 | e sliced from the positions immediately preceding each response token, and the resulting log-probabilities are written back into the fixed response-width tensors expected by…
  References | score=0.841 | tream and reducing residual latency after the final input segment arrives. Following StreamingThinker (Tong et al., 2025a), we set the input reading speed to 150 words per…
  References | score=0.839 | age assignment, and actor upKL coefficient False date, ensuring that all policy-gradient quantities Max prompt length 8192 tokens are computed under the same streaming…
- 相关性：
  Abstract | score=1.247 | r-driven systems must they largely rely on supervised imitation of preconstructed trajectories, which limits their flexreact before an event is fully observed (Gu et al.…

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：对于关注智能体（Agent）实时交互的开发者，该论文提供的流式推理范式具有直接参考价值。

## 基本信息

- 作者：Junlong Tong, Wenqi Xu, Yingqi Fan, Anhao Zhao, Xuan Lu, Yang Tan, Xiaoyu Shen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-12
- 推荐级别：**按需阅读**
- 预计阅读时间：约 20 分钟
- 解析来源：PDF 全文 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2606.14694v1`

## 代码与资源

- 代码：暂未发现公开链接
- 数据：暂未发现公开链接
- 项目主页：暂未发现公开链接
- 原文 PDF：https://arxiv.org/pdf/2606.14694v1
- arXiv：https://arxiv.org/abs/2606.14694v1
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，重点提取了 HRPO 算法的数学动机、流式推理的架构设计以及在 GSM-symbolic 数据集上的实验表现。信息涵盖了从背景到局限性的全维度分析。
- 方法证据锚点：References | score=0.941 | cy, the reference policy, checkpointing intact, while changing only the parts and the updated actor. A subtle issue is that the that define rollout semantics and token…
- 结果证据锚点：References | score=0.878 | λ controls the local-global balance in HRPO. Rows are grouped by ablation theme, and evaluation reports accuracy and total generation length on GSM-symbolic P2/P1 and MetaMathQA.…
