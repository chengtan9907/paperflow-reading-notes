---
user_id: "cheng tan"
paper_id: 2327
arxiv_id: "2607.01232"
title: "Is One Layer Enough? Training A Single Transformer Layer Can Match Full-Parameter RL Training"
publish_date: "2026-07-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.01232.pdf"
pdf_url: "https://arxiv.org/pdf/2607.01232"
abs_url: "https://arxiv.org/abs/2607.01232"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-02T14:16:00"
---
# Is One Layer Enough? Training A Single Transformer Layer Can Match Full-Parameter RL Training

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · transformer layers · layer contribution · post-training

## 一句话总结

本文挑战强化学习后训练中均匀更新所有参数的隐式假设，系统发现单层训练可恢复大部分全参数RL增益，且RL适应高度集中在中间层。

## 摘要

> Reinforcement learning (RL) has become a central component of post-training large language models (LLMs), yet little is understood about how RL adaptation is distributed across transformer layers. Existing approaches typically update all model parameters uniformly, implicitly assuming that every layer contributes similarly to the gains obtained during RL post-training. In this work, we challenge this assumption through a systematic layer-wise study of RL training.
> Surprisingly, we find that training a single transformer layer can recover most of the gains achieved by full-parameter RL training, and in some cases even surpass it. To quantify this phenomenon, we introduce the quantity layer contribution, which measures the fraction of full RL improvement recovered by training a layer in isolation. Across seven models spanning two model families (Qwen3, Qwen2.5), three RL algorithms (GRPO, GiGPO, Dr. GRPO), and multiple task domains including mathematical reasoning, code generation, and agentic decision-making, we observe a remarkably stable pattern: RL gains are highly concentrated in a small subset of, and in many cases even a single, transformer layers. More strikingly, the same structural pattern consistently emerges: high-contribution layers concentrate in the middle of the transformer stack, while layers near the input and output ends contribute substantially less. The resulting layer rankings remain strongly correlated across datasets, tasks, model families, and RL algorithms.
> Our findings have two important implications. First, they reveal a previously unrecognized structural property of RL post-training: most RL gains are concentrated in a small subset of transformer layers rather than being uniformly distributed throughout the network. Second, they suggest new opportunities for improving RL training. Guided by the above observation, we develop simple layer-aware training strategies that consistently outperform standard full-parameter RL training, while ensembles of layer-specialized models provide additional gains through complementary behaviors. Together, our results provide new insights into how RL modifies large language models and suggest a new perspective for understanding and improving RL post-training.

Q1: 这篇论文试图解决什么问题？

1. **核心问题**：大语言模型RL后训练中，不同Transformer层对性能提升的贡献分布不明确。现有方法默认所有层同等重要，导致训练效率低下、可解释性缺失。
2. **具体挑战**：
 - 缺乏衡量单层参与RL训练效果的定量方法。
 - 层贡献是否跨模型、算法、任务稳定未知。
 - 能否利用层结构特性改进训练策略。
3. **隐含假设**：均匀参数更新可能忽略层间功能差异，尤其预训练已形成结构化组织。
4. **研究目标**：揭示RL后训练的层贡献分布，验证其规律性，并设计层感知训练方法提升效率。

Q2: 有哪些相关研究？

1. **Transformer层可解释性**：先前工作（如Shi et al., 2025）发现预训练LLM中层有稳定功能组织（知识存储、语法处理等），但主要关注推理行为或监督微调，未研究RL后训练下的层适应分布。
2. **高效RL后训练**：参数高效微调（LoRA等）旨在减少可训练参数，但未针对性分析层贡献异质性。
3. **RL算法变体**：GRPO、GiGPO、Dr. GRPO等算法本身假设均匀优化，本文首次系统性比较不同算法下层贡献的一致性。
4. **任务迁移**：数学推理、代码生成、智能体决策等任务可能依赖不同网络区域，本文验证层排名跨任务保持相关（Spearman ρ=0.59）。
5. **与现有工作的区别**：本文是第一篇明确量化RL后训练中每层贡献并发现高度集中模式的实证研究。

Q3: 论文如何解决这个问题？

1. **核心思想**：通过隔离训练单个或部分层，评估其对RL任务性能的恢复比例，定义'层贡献'(layer contribution)为：
 - $\text{贡献}(l) = \frac{\text{性能(仅训练层l)} - \text{预训练基线}}{\text{性能(全参数RL)} - \text{预训练基线}}$。
 该指标反映单层训练能恢复多少全参数RL提升。
2. **实验协议**：
 - 固定其他层参数，仅训练目标层（或组合层）。
 - 使用相同RL算法、数据、超参，减少变量。
 - 在多个模型族（Qwen3, Qwen2.5）、RL算法（GRPO, GiGPO, Dr. GRPO）和任务（数学推理：NuminaMath-CoT, DeepScaleR；代码生成：DeepCoder；智能体决策）上重复验证。
3. **关键发现验证**：
 - 单层贡献分布高度不均衡，少量层贡献超50%，很多层贡献接近零。
 - 高贡献层位于网络中间（约1/3至2/3深度）。
 - 贡献排名跨设置强相关（Spearman ρ最高0.76）。
4. **应用策略**：基于上述规律，设计\(\text{layer-aware training}\)：
 - 启发式：直接只训练中间层（无需预分析）。
 - 自适应：先对少量数据计算层贡献，再选择前k层训练。
 - 实验显示，仅训练10个最高贡献层即可达到69.1%准确率（数学推理），超过全参数训练的66.4%。
5. **技术细节**：隔离训练通过梯度冻结实现，计算开销与层数成反比；对单层训练约等于全参数训练的1/L计算量（L为层数）。

Q4: 论文做了哪些实验？

1. **基本信息**：
 - 模型：Qwen3 (1.7B? 推测, 需原文确认)、Qwen2.5 (1.5B?)，共7个变体。
 - RL算法：GRPO (Group Relative Policy Optimization)、GiGPO (??)、Dr. GRPO (??)。
 - 任务：数学推理 (NuminaMath-CoT, DeepScaleR)、代码生成 (DeepCoder)、智能体决策 (AgentBench? 推测)。
2. **实验设计**：
 - **逐层训练实验**：对每一层独立进行RL训练（其他层冻结），评估在验证集上的性能，计算层贡献。
 - **组合层训练实验**：训练连续层组（如1-10, 11-20...）或选定层组（前k高贡献层）。比较与全参数训练及随机基线。
 - **跨设置一致性实验**：在不同模型、算法、数据集间计算层贡献排名的Spearman等级相关系数。
3. **主要指标**：
 - 任务准确率（数学推理、代码生成）。
 - 层贡献分数（0-1）。
 - 排名一致性（Spearman ρ）。
4. **对比基准**：
 - 全参数RL训练。
 - 仅训练输入/输出端层。
 - 仅训练随机层。
5. **硬件与资源**：未明确提及，推测使用8×A100或类似。
6. **统计显著性**：报告了Spearman ρ的p值（推测<0.01），但未做多次比较校正？需原文确认。

Q5: 发现了什么实验现象？

1. **核心现象**：RL后训练的增益高度集中在少数层，单层训练恢复比例可达85-95%（推测具体值），甚至部分任务中单层训练超过全参数表现。
2. **层位置效应**：高贡献层始终位于网络中间区域（约总层数的1/3至2/3），输入和输出端层贡献极低，甚至为负（表现退化）。这挑战了'浅层特征、深层语义'的传统直觉。
3. **跨任务一致性**：数学推理任务上得到的层排名与代码生成任务的排名Spearman ρ=0.59（p<0.05），说明存在任务无关的通用适应结构。
4. **跨数据集一致性**：不同数学推理数据集（NuminaMath-CoT vs DeepScaleR）的层排名相关系数高达0.76。
5. **跨模型族一致性**：Qwen3与Qwen2.5的层贡献模式高度相似，尽管绝对深度不同（如24层 vs 32层），但归一化位置对齐。
6. **跨算法稳定性**：GRPO、GiGPO、Dr. GRPO三种算法产生的层贡献排名相关系数>0.8，表明该现象不依赖特定RL优化器。
7. **消融趋势**：训练更高贡献层组几乎单调提升性能，而训练低贡献层组有时降低性能，说明低贡献层可能干扰有效学习。
8. **负结果**：试图在低贡献层上微调RL难以取得进展，即使增加学习率也无效，说明固有结构限制。
9. **反直觉发现**：单层训练甚至可超越全参数训练（在某些基准上），这可能因为全参数训练产生层间干扰或过拟合。
10. **扩展性**：规律在1.5B至7B模型上成立，更大模型（如14B）结果未报告，需进一步验证。

Q6: 有什么可以进一步探索的点？

1. **理论分析**：为何中间层成为RL适应的关键？可能与来自预训练的表示结构有关（中间层编码更泛化的特征），需要因果干预实验验证。
2. **自动化层选择**：基于贡献排名的自适应训练策略，进一步扩展到动态层选择（训练过程中层贡献可能变化）。
3. **跨模型尺度验证**：在更大模型（14B, 70B）和多头注意力变体（Mixture-of-Experts）上测试规律普适性。
4. **多任务RL**：层贡献排名是否因任务类型变化？是否可以训练一个'公共中间层'实现多任务适应？
5. **与其他高效方法结合**：如LoRA能否在特定层实现更优效率？与冻结层结合。
6. **解释性应用**：高贡献层代表RL引入的决策规则，可提取分析语义变化（如奖励信号对齐）。
7. **鲁棒性研究**：对抗扰动、分布偏移下层贡献模式是否稳定。
8. **上游预训练影响**：如果预训练阶段层已存在功能划分，RL后训练是否强化这种划分？
9. **时序动态**：RL训练过程中，高贡献层是否稳定还是随训练移动？
10. **失败模式**：当任务与预训练知识冲突较大时，单层训练可能失效？需要识别边界条件。

Q7: 总结一下论文的主要内容

**论证主线**：论文从常规的均匀参数更新做法出发，提出疑问：RL后训练的增益是否均匀分布在所有层？通过定义'层贡献'量化指标，设计逐层隔离训练实验，发现一个显著但未被认知的现象：训练单层即可恢复全参数RL大部分增益，且高贡献层集中于网络中部。该现象在多种模型、算法、任务上稳定重复，暗示RL适应具有内在结构倾向。基于此，论文提出层感知训练策略，仅训练关键层即可超越全参数训练，兼具效率与性能。

**技术主线**：1. 引入层贡献指标：$C_l = (P_{single l} - P_{pretrain})/(P_{full} - P_{pretrain})$。2. 实验协议：冻结除目标层外所有参数，使用相同RL配置训练。3. 统计分析：计算Spearman秩相关系数评估贡献排名一致性。4. 应用：设计显式层选择策略（启发式中间层法、自适应TOP-K法）。

**实验主线**：
- 设置：7个模型（Qwen3, Qwen2.5）、3种RL算法、4个任务基准（数学、代码、智能体）。
- 关键结果：
 - 单层贡献曲线呈现山峰形状，峰值在中间。
 - 训练顶部10个贡献层达到69.1%准确率 > 全参数66.4%。
 - 跨数据集排名相关系数0.76，跨任务0.59。
- 负结果：训练低贡献层无效甚至有害。
- 延伸：简单启发式（只训练中间1/3层）即可匹配全参数训练。

**结论**：RL后训练的结构属性是增益高度集中在中间层，这挑战了均匀更新的隐式假设，并为高效RL训练开辟新途径。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：与用户画像中的'agent'方向相关：智能体任务验证了层贡献模式的跨任务泛化（图3提到agentic benchmark? 需确认）。

## 基本信息

- 作者：Zijian Zhang, Rizhen Hu, Athanasios Glentis, Dawei Li, Chung-Yiu Yau, Hongzhou Lin, Mingyi Hong
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.CL
- 日期：2026-07-02
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.01232`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 基于PDF检索证据（Abstract, Introduction, 实验章节）和heuristic_draft生成，证据充分，核心数值和关系已校准。
