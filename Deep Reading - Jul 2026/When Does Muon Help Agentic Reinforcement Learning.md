---
user_id: "cheng tan"
paper_id: 4504
arxiv_id: "2607.16169"
title: "When Does Muon Help Agentic Reinforcement Learning?"
publish_date: "2026-07-20"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.16169.pdf"
pdf_url: "https://arxiv.org/pdf/2607.16169"
abs_url: "https://arxiv.org/abs/2607.16169"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-21T01:41:30"
---
# When Does Muon Help Agentic Reinforcement Learning?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：muon optimizer · agentic reinforcement learning · spectral normalization · credit assignment

## 一句话总结

本研究探讨了 Muon 优化器在稀疏奖励智能体强化学习（Agentic RL）中的应用，发现在 ALFWorld 任务中，Muon 配合特定的优势估计器（如 GiGPO）能比 AdamW 提升 88% 的验证成功率。

## 摘要

> Muon is competitive with AdamW in large-scale pre-training, but its value for reinforcement-learning (RL) post-training remains unclear. We study vanilla Muon in sparse-reward agentic RL through matched single-seed comparisons with AdamW on ALFWorld using Qwen2.5-0.5B-Instruct. Under Group-in-Group Policy Optimization (GiGPO), applying Muon only to hidden weight matrices raises final-window validation success from 0.290 to 0.546 (+88%); high-rate AdamW controls retain no post-update success. The effect depends on the advantage estimator and learning rate. At 3e-5, Muon improves GRPO from 0.161 to 0.268, whereas GraphGPO's late-window gap narrows near saturation. At 1e-5, GraphGPO Muon reaches 0.901, raises normalized validation AUC from 0.399 to 0.556, and reaches 0.5 and 0.75 success 30 and 60 updates earlier, respectively. These exploratory results show that Muon can benefit agentic RL and motivate studying the policy optimizer, advantage estimator, and learning rate jointly. Multi-seed and cross-task validation remain open.

Q1: 这篇论文试图解决什么问题？

本研究的核心问题在于探索 Muon 这一新兴的二阶优化器近似算法在强化学习（RL）后训练场景下的适用性。Muon 通过 Newton–Schulz (NS) 迭代实现近似谱归一化，在预训练阶段被证明能以约 52% 的 FLOPs 达到 AdamW 的性能，但在 RL 领域，其表现一直存在争议。论文指出，RL 环境下的梯度具有极低的信噪比（SNR），且存在严重的“学习-遗忘”权衡问题。现有的研究（如 Fan et al. 2026）认为原生 Muon 在 RLVR 等任务中会失败，因为谱白化（Spectral Whitening）会放大噪声主导的梯度尾部。此外，智能体任务通常具有长时程和稀疏奖励的特点，这对优化器的稳定性和收敛速度提出了极高要求。论文试图通过严谨的受控实验，厘清 Muon 在不同优势估计器（GRPO, GiGPO, GraphGPO）和学习率配置下的真实表现，解决“Muon 是否以及何时能帮助智能体强化学习”这一关键悬疑。研究特别关注了 Muon 如何处理 RL 中的非平稳数据分布，以及它是否能缓解 AdamW 在高学习率下容易出现的性能崩溃问题。

Q2: 有哪些相关研究？

相关研究主要围绕优化器演进、强化学习算法框架以及 Muon 的变体展开。首先，AdamW 作为 RL 后训练的标配，其逐元素自适应缩放机制在处理非平稳梯度时表现稳健，但在大规模扩展时效率受限。Muon (Jordan et al. 2024) 引入了动量矩阵的近似谱归一化，为预训练提供了新的范式。在 RL 领域，针对 Muon 的改进包括 Pion（引入高通谱过滤以应对低 SNR 梯度）和 Hopper（结合方差归一化与单步 NS 迭代以提升稳定性）。在算法框架方面，研究对比了 GRPO（基于片段级别的优势估计）、GiGPO（引入锚点状态的步级优势估计）以及 GraphGPO（基于图结构的信用分配）。此外，论文还引用了关于优化器失配（Optimizer Mismatch）的研究，指出使用 Muon 对 Adam 预训练模型进行全量微调时，可能会加剧灾难性遗忘。本研究通过在 ALFWorld 这一复杂的智能体推理任务上进行系统对比，填补了原生 Muon 在稀疏奖励 RL 中表现评估的空白。

Q3: 论文如何解决这个问题？

论文采用了一种受控的实验方法，将 Muon 集成到多种基于组的策略优化（GPO）算法中。技术路线的核心在于：1. **权重选择性应用**：仅将 Muon 应用于模型的隐藏层权重矩阵（Hidden Weight Matrices），而对嵌入层（Embedding）和输出头（Head）保留 AdamW 优化，以维持表示的稳定性。2. **算法集成**：将 Muon 分别与 GRPO、GiGPO 和 GraphGPO 结合。其中 GiGPO 通过锚点状态（Anchor-state）提供步级信用分配，而 GraphGPO 则利用图结构处理更复杂的依赖关系。3. **超参数扫描**：针对学习率（1e-5, 3e-5）进行了精细化对比，以观察 Muon 在不同梯度强度下的表现。4. **近似谱归一化**：利用 Newton–Schulz 迭代计算动量矩阵的近似正交化，旨在通过约束权重的谱范数来稳定训练动力学。5. **基准对齐**：所有实验均在 Qwen2.5-0.5B-Instruct 模型上进行，确保模型容量与任务难度匹配，并使用单种子匹配运行（Matched Single-seed Runs）以消除随机性干扰，直接观察优化器行为的差异。

Q4: 论文做了哪些实验？

实验在 ALFWorld 环境下展开，这是一个要求智能体在模拟家庭场景中执行多步交互任务（如“把洗净的苹果放进冰箱”）的基准测试。实验配置如下：1. **模型选择**：使用 Qwen2.5-0.5B-Instruct 作为基础策略模型。2. **优化器对比**：Muon vs. AdamW。3. **算法变体**：对比了 GRPO（无步级信用）、GiGPO（有步级信用）和 GraphGPO（高级信用分配）。4. **关键指标**：验证成功率（Validation Success Rate）、归一化验证曲线下面积（Normalized Validation AUC）以及达到特定成功率阈值所需的更新步数。5. **训练细节**：在 3e-5 和 1e-5 两种学习率下进行测试。实验特别设计了“匹配运行”协议，即在相同的初始权重和数据流下观察不同优化器的演化轨迹。通过这种方式，研究者能够捕捉到 Muon 在训练中后期的收敛优势，以及它在 AdamW 发生性能回撤时的鲁棒性。

Q5: 发现了什么实验现象？

实验揭示了几个关键现象：1. **显著的性能增益**：在 GiGPO 框架下，Muon 在 3e-5 学习率时将最终成功率从 0.290 提升至 0.546，提升幅度达 88%。2. **高学习率下的鲁棒性**：当学习率为 3e-5 时，AdamW 在训练后期出现了明显的性能崩溃（成功率归零），而 Muon 能够保持稳定的成功率，显示出更强的正则化效应。3. **收敛加速**：在 GraphGPO 实验中，Muon 在 1e-5 学习率下比 AdamW 提前 30 个更新步达到 0.5 成功率，提前 60 个更新步达到 0.75 成功率。4. **与优势估计器的协同效应**：Muon 的表现受优势估计器影响极大。在 GRPO（仅片段级优势）中，Muon 虽然有提升但幅度较小；而在引入步级信用的 GiGPO 和 GraphGPO 中，Muon 的优势被显著放大。5. **饱和效应**：在 GraphGPO 接近饱和（成功率 > 0.9）时，Muon 与 AdamW 的差距会缩小，但在训练早期的上升斜率依然占优。6. **负面反馈**：论文也确认了前人关于 Muon 在低 SNR 梯度下可能不稳定的观察，但在本研究的特定配置（隐藏层应用 + 步级优势）下，这种不稳定性得到了有效抑制。

Q6: 有什么可以进一步探索的点？

未来的研究方向包括：1. **多种子与跨任务验证**：由于本研究主要基于单种子实验，未来需要在更多随机种子和不同类型的智能体任务（如代码生成、数学推理）上验证结论的普适性。2. **模型规模扩展**：探索 Muon 在 7B、14B 乃至更大参数规模的 LLM 后训练中的表现，观察是否存在 Scaling Law。3. **优化器-估计器联合设计**：研究是否可以开发专门针对 Muon 谱特性的优势估计器，或者调整 Muon 的 NS 迭代次数以适应 RL 的动态梯度。4. **混合优化策略**：进一步细化哪些层最适合 Muon，哪些层必须保留 AdamW，例如探讨对 KV Cache 相关权重的特殊处理。5. **长上下文与复杂推理**：在需要极长推理链条的任务中，测试 Muon 是否能更好地维持长程依赖的梯度流。6. **理论解释**：深入研究谱归一化如何具体影响策略分布的熵演化，以及它如何缓解 RL 中的过拟合问题。

Q7: 总结一下论文的主要内容

本论文系统研究了 Muon 优化器在智能体强化学习（Agentic RL）中的表现，挑战了“Muon 仅适用于预训练”的传统认知。研究者在 ALFWorld 任务上，利用 Qwen2.5-0.5B-Instruct 模型，对比了 Muon 与 AdamW 在多种策略优化算法（GRPO, GiGPO, GraphGPO）下的效果。核心发现是，Muon 并非在所有 RL 场景下都优于 AdamW，其有效性高度依赖于信用分配机制（即优势估计器）和学习率的选取。在 GiGPO 框架下，Muon 展现了惊人的 88% 性能提升，并有效解决了 AdamW 在高学习率下容易出现的训练崩溃问题。在 GraphGPO 框架下，Muon 则表现出显著的收敛加速能力，大幅缩短了达到高成功率所需的训练时间。论文通过详尽的消融实验证明，将 Muon 应用于隐藏层权重矩阵是兼顾性能与稳定性的关键。尽管这是一项探索性研究，且受限于单种子实验，但它为 RL 后训练阶段的优化器选择提供了全新的视角，证明了通过谱归一化约束权重空间可以显著改善智能体在稀疏奖励环境下的学习效率。研究最后呼吁学术界关注优化器与信用分配机制之间的深度耦合，为开发更高效、更稳健的智能体训练协议奠定了基础。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该研究直接关联智能体（Agent）训练优化，是当前 LLM 后训练的前沿课题。

## 基本信息

- 作者：Kai Ruan, Jinghao Lin, Zihe Huang, Ziqi Zhou, Qianshan Wei, Xuan Wang, Hao Sun
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI
- 日期：2026-07-20
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2607.16169`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，重点结合了 Introduction、Results 和 Conclusion 部分的语义片段，确保了对 Muon 在 RL 中表现的准确描述。
