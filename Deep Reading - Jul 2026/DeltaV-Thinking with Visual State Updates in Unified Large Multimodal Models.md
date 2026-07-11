---
user_id: "cheng tan"
paper_id: 3165
arxiv_id: "2607.08434"
title: "DeltaV: Thinking with Visual State Updates in Unified Large Multimodal Models"
publish_date: "2026-07-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.08434.pdf"
pdf_url: "https://arxiv.org/pdf/2607.08434"
abs_url: "https://arxiv.org/abs/2607.08434"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-11T00:10:19"
---
# DeltaV: Thinking with Visual State Updates in Unified Large Multimodal Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：unified large multimodal model · visual state update · interleaved multimodal reasoning · token efficiency

## 一句话总结

提出DeltaV，一种在统一大多模态模型中以视觉状态更新替代全图像生成的框架，显著减少视觉令牌冗余并提升多模态推理性能。

## 摘要

> Current Unified Large Multimodal Models (ULMMs) support interleaved multimodal reasoning through textual reasoning and intermediate visual states, but typically generate each visual state as a full image. This full-image generation paradigm introduces substantial visual-token redundancy and dilutes supervision on sparse yet reasoning-critical state transitions. We propose DeltaV, a ULMM that replaces full-image generation with visual updates. Conditioned on historical visual states, DeltaV incrementally predicts compact update tokens that capture the visual changes across reasoning steps, avoiding repeated modeling of unchanged content. To align the token budget of each update with the magnitude of visual change, DeltaV introduces a temporal similarity (TSIM) Router, which stops allocating tokens once the marginal reconstruction gain falls below a threshold. To support more diverse and generalizable reasoning, we further construct StructCoT, a large-scale interleaved multimodal reasoning dataset with 1.05M samples spanning 44 task domains. Experiments show that the visual-update paradigm reduces newly generated visual tokens by 55.6\% on average without compromising reconstruction fidelity, and improves multimodal reasoning by 3.3\% over full-image generation. Trained with StructCoT and large-scale multimodal data, DeltaV-2B further outperforms substantially larger open-source models by 8.4\% on in-domain multimodal reasoning evaluations and surpasses the comparable-scale Qwen3-VL-2B by 5.9\% on external multimodal reasoning and understanding benchmarks. Code, models, and StructCoT will be released at https://github.com/Pengjie-W/DeltaV.

Q1: 这篇论文试图解决什么问题？

论文试图解决统一大多模态模型（ULMM）在交织多模态推理中生成中间视觉状态时的效率与监督问题。具体而言：
1. 视觉令牌冗余：每个推理步骤生成完整图像，大量令牌用于编码不变的背景或重复内容，只有少数像素发生变化。这导致视觉令牌预算被浪费，限制了可处理的推理步长和图像分辨率。
2. 监督信号稀释：全图像生成使得损失函数主要惩罚整个图像的重建误差，而推理关键的变化部分只贡献微小信号。这使得模型难以将梯度归因于真正驱动推理的因果视觉变换，从而削弱了学习效果。
3. 缺乏多样性数据：现有交织视觉推理工作通常局限于特定场景（迷宫、谜题、视觉搜索），限制了模型的泛化能力。
因此，需要一种新的范式：将视觉推理建模为状态更新过程，仅生成变化部分，并配备自适应令牌分配机制，同时构建大规模多样化数据集。

Q2: 有哪些相关研究？

相关研究包括：
- 统一大多模态模型（ULMMs）：如Qwen-VL系列，能够处理交织的文本和图像输入输出，但采用全图像生成范式，存在上述冗余问题。
- 视觉思维链（Visual CoT）：如Gu et al. (2025), Li et al. (2025a,b), Yang et al. (2026b)等，在特定领域（迷宫、谜题、视觉搜索）中使用中间视觉状态进行推理，但场景受限，难以推广到一般任务。
- 多模态推理数据集：现有数据集如CLEVR、NLVR等，但缺乏大规模、多样化的交织推理样本。StructCoT的构建填补了这一空白，包含44个任务领域和1.05M样本。
- 令牌效率方法：在视觉语言模型中已有使用稀疏注意或图像压缩的方法，但针对推理过程中动态更新的令牌分配研究较少。DeltaV的TSIM Router是受时间相似性启发的自适应分配策略。

Q3: 论文如何解决这个问题？

DeltaV的解决方案包括三个核心组件：
1. 视觉更新范式（Visual Update Formulation）：将推理过程建模为视觉状态序列。初始状态Z0（如初始图像），每个后续步骤生成更新令牌ΔZt，而不是完整图像。更新令牌仅编码视觉变化，通过条件生成模型（如扩散解码器）从历史状态和文本查询中推断。这大幅减少令牌数量，并将监督信号集中在变化上。
2. 时间相似性路由器（TSIM Router）：控制每个更新步骤生成的令牌数量。它计算当前状态与历史状态之间的时间相似性（如基于特征相似度），当边际重建增益低于阈值时停止分配令牌。这样，令牌预算自动匹配视觉变化的幅度：静态步骤分配少量令牌，变化剧烈步骤分配较多令牌。
3. StructCoT数据集：一个大规模、多样化的交织多模态推理数据集，包含1.05M样本，覆盖44个任务领域（如视觉问答、图表推理、情景理解等）。数据构造使用自动流程，确保包含有意义的视觉状态更新。
整体框架基于自回归语言模型，但视觉状态更新通过专用解码器嵌入到序列中，实现无缝交织。

Q4: 论文做了哪些实验？

论文进行了以下主要实验：
1. 令牌效率比较：将DeltaV与全图像生成基线在相同配置下比较，测量新生成视觉令牌数量。结果显示，DeltaV平均减少55.6%的视觉令牌，同时保持重建保真度（通过FID或PSNR等指标，具体未在片段中给出）。
2. 多模态推理性能：在域内评估（StructCoT测试集）上，DeltaV-2B比全图像生成基线提升3.3%。
3. 与开源模型比较：在域内评估上，DeltaV-2B超出更大开源模型（如Qwen-VL-7B等）平均8.4%。在外部基准（如MMBench、SEED-Bench等，推测）上，DeltaV-2B超越同等规模的Qwen3-VL-2B 5.9%。
4. 消融研究：可能包括移除TSIM Router（使用固定令牌预算）、使用不同更新策略等，但片段中未详细给出。训练使用StructCoT和大规模多模态数据（具体数据量未提及）。

Q5: 发现了什么实验现象？

实验观察到以下现象：
1. 视觉更新范式显著降低令牌消耗：平均减少55.6%的新生成视觉令牌，且未带来重建保真度的下降（合理推断，因为更新模型更聚焦于变化）。
2. 推理性能提升：与全图像生成相比，DeltaV在域内推理指标上提升3.3%，表明聚焦于更新的监督更有效。
3. 自适应令牌分配有效：TSIM Router根据视觉变化自动调整令牌预算，在高变化步骤分配更多令牌，在静态步骤分配更少，从而在不牺牲性能的前提下优化效率。
4. 规模泛化：DeltaV-2B在域内评估上优于更大的开源模型，表明更新范式具有更好的数据效率和模型能力。
5. 数据集贡献：StructCoT的引入提升了模型的泛化能力，在44个领域表现良好。
（未提供负结果或失败案例，但可能在某些边缘场景如极稀疏更新时存在挑战，这需要原文确认。）

Q6: 有什么可以进一步探索的点？

论文未明确讨论未来方向，基于贡献可推测：
1. 扩展到更大模型和更高分辨率：当前DeltaV-2B仅2B参数，可探索更大规模以验证scaling效果。
2. 拓展到更多模态：如视频、3D、音频等，其中状态更新可能更高效。
3. 改进TSIM Router：研究更复杂的相似度度量或学习式的停止准则。
4. 结合强化学习：将更新令牌分配作为决策过程，或使用奖励信号优化令牌预算。
5. 应用于下游任务：如视觉推理链、机器人操作、交互式对话等。
6. 理论分析：研究更新范式的表示学习特性，解释为什么聚焦变化有助于推理。

Q7: 总结一下论文的主要内容

论文DeltaV提出了一种视觉更新范式，用于统一大多模态模型中的交织多模态推理。当前ULMMs在每个推理步骤生成完整中间图像，导致大量令牌冗余和监督稀释。DeltaV将其替换为增量视觉更新：模型基于历史状态预测紧凑的更新令牌，仅编码变化内容，并引入时间相似性（TSIM）路由器动态控制每个更新的令牌预算。为了支持多样化推理，作者构建了StructCoT数据集，包含1.05M样本，覆盖44个任务领域。实验表明，DeltaV在不牺牲重建保真度的前提下，减少55.6%的视觉令牌，并将推理性能提升3.3%超过全图像生成基线。训练的DeltaV-2B在域内评估中超越更大开源模型8.4%，在外部基准上超越同等规模Qwen3-VL-2B 5.9%。论文的主要贡献包括提出了一个高效的视觉更新框架、一个自适应令牌分配机制、一个大规模多模态推理数据集，以及全面的实验验证。局限性可能包括对初始视觉状态的敏感性和对复杂场景更新的处理能力（未经证实）。该工作为多模态推理提供了新的发展方向，特别是在计算效率和监督效率方面。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文核心是生成方向（权重0.1），特别是高效视觉生成与推理的结合。

## 基本信息

- 作者：Pengjie Wang, Linger Deng, Zujia Zhang, Shaojie Zhang, Zhenbo Luo, Pei Fu, Jian Luan, Xiang Bai, Yuliang Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.08434`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的证据片段（retrieved_evidence），并优先采用了field_evidence_map中对应字段的证据；对于未覆盖的细节，基于摘要和推理进行补充，并标注了推测或合理推断。
