---
user_id: "cheng tan"
paper_id: 9135
arxiv_id: "2608.23256v1"
title: "Is Next-Chunk Reasoning RL Really Better than SFT? Revisiting Training Strategies under no-CoT Data"
publish_date: "2026-08-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.23256v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.23256v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:05:44"
---
# Is Next-Chunk Reasoning RL Really Better than SFT? Revisiting Training Strategies under no-CoT Data

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：next-chunk reasoning RL · no-CoT data · mixed SFT · reinforcement learning with verifiable rewards

## 一句话总结

本文通过受控对比研究发现，对于缺乏显式思维链（no-CoT）数据，简单的混合监督微调（Mixed SFT，单阶段联合训练 no-CoT 与长思维链数据）相比 next-chunk 推理强化学习（NCR）在后续 RLVR 微调后能达到更高的性能上限，且训练计算量少 60 倍以上，并指出预 RLVR 准确率并不能预测后 RLVR 表现。

## 摘要

> Recent work proposes next-chunk reasoning RL for leveraging no-CoT data---corpora such as worked solutions and textbook derivations that contain reasoning-rich content but lack explicit chain-of-thought annotations. The method trains a model to generate implicit reasoning traces and rewards them by their ability to predict the next chunk of text. While promising, existing evaluations primarily compare against conventional SFT baselines, leaving open whether the gains come from the RL formulation itself or from more effectively exposing the model to no-CoT data. We address this question with a controlled study of next-chunk reasoning RL and a simple but previously overlooked alternative: Mixed SFT, a single supervised fine-tuning stage that jointly trains on no-CoT and long-CoT data. Despite its simplicity, Mixed SFT achieves a clearly higher post-RLVR performance ceiling than next-chunk reasoning RL while requiring over 60 times less training compute. The advantage is consistent across in-domain mathematical reasoning and out-of-domain reasoning tasks. Moreover, we show that higher pre-RLVR accuracy does not necessarily translate into higher post-RLVR accuracy, highlighting the need to evaluate no-CoT training strategies in the context of the full post-training pipeline.

Q1: 这篇论文试图解决什么问题？

本文试图解决的核心问题是：现有 next-chunk 推理强化学习（NCR）方法在利用 no-CoT 数据方面声称优于传统 SFT，但已有比较设计存在混淆变量，无法回答其收益究竟来自 RL 公式本身，还是仅仅因为该方法让模型更充分地接触了 no-CoT 数据。具体来说，已有工作通常将 NCR 与只在 no-CoT 数据上训练的 SFT 比较，而忽略了一个同样自然且有竞争力的基线——同时使用 no-CoT 和长 CoT 数据的联合 SFT（即 Mixed SFT）。此外，论文也关注后训练管线中评估位置的影响：若仅在中间检查点（pre-RLVR）评估，可能会得出误导性结论，因为预训练阶段的准确率与经过 RLVR 微调后的最终准确率并不呈单调正相关。更深层的问题是，no-CoT 数据如何被组织进训练（例如是否与长 CoT 数据混合、混合顺序）可能比“是否包含 no-CoT 数据”本身带来更大的影响，而这一点在已有文献中尚未被系统研究。

Q2: 有哪些相关研究？

相关研究可大致分为三个脉络：
1. no-CoT 数据的利用：no-CoT 数据（如作业解答、教科书推导）富含推理过程却无显式思维链注释。朴素 SFT 可能破坏模型原有的推理格式（引用 Chu et al., 2025; Matsutani et al., 2025; Fang et al., 2026），因此需要更精细的训练策略。
2. next-chunk 推理强化学习（NCR）：此类方法将 no-CoT 文本转化为可优化的 RL 任务——模型先生成推理轨迹，再依据预测后续文本块的能力获得奖励。例如 RPT（Dong et al., 2025）将 next-token prediction 重新构想为 RL 可优化任务。NTR、NSR 是本文纳入比较的两种具体变体（推测为 next-token reasoning 和 next-sentence reasoning，证据未明确展开）。
3. SFT 与 RL 的后训练比较：已有研究通常在 RLVR（带可验证奖励的强化学习）之前或之后评估模型性能，但本文指出这种比较缺乏对中间检查点评估偏差的关注。本文引入的 Mixed SFT 直接挑战了 NCR 的显著性，并建议将评估放在完整后训练管线末端进行。

Q3: 论文如何解决这个问题？

论文采用统一的受控实验框架来回答研究问题。从同一个预训练基础模型出发，在统一的 RLVR 预算条件下，比较五种训练策略：NTR（next-token reasoning RL）、NSR（next-sentence reasoning RL）、Sequential SFT（先 no-CoT SFT 再长 CoT SFT，或反之）、Mixed SFT（单阶段联合训练 no-CoT 与长 CoT 数据）以及 Reasoning SFT（仅在长 CoT 上训练）。所有策略在获得各自的中间检查点后，均接入相同的 RLVR 微调阶段，并在域内（数学推理）与域外（其他推理任务）基准上评估最终性能。论文的核心技术贡献是显式引入 Mixed SFT 作为缺失基线，并通过 pre-RLVR 与 post-RLVR 的双阶段评估，分离训练策略本身的效果与后续 RL 微调的效果。此外，论文通过中间检查点的系统性分析，揭示单一检查点评估可能带来的偏差，并用机制分析解释为何 Mixed SFT 尽管 pre-RLVR 精度最低，却能在 post-RLVR 达到最高上限。

Q4: 论文做了哪些实验？

根据摘要和证据片段，论文开展了以下实验：
- 在六个（或至少六个）任务/基准上，从同一预训练基础模型出发，以统一 RLVR 预算比较 NTR、NSR、Sequential SFT、Mixed SFT 与 Reasoning SFT。
- 分别在 RLVR 前后（pre-RLVR 与 post-RLVR）评估各策略的性能，覆盖域内数学推理和域外推理任务。
- 比较训练计算成本，重点标注 Mixed SFT 与 NCR 的算力差异（超过 60 倍）。
- 进行中间检查点评估，系统性分析检查点位置对结论的影响。
- 开展了机制分析（证据片段提到“Analytically, we uncover the mechanisms behind…”），具体内容未在摘要中披露，推测涉及对模型推理隐空间或输出分布的量化分析。
由于文献元数据未提供完整实验部分，具体基准名称、数据集来源、超参数设置和消融细节均无法从当前证据中确认，需查阅原文。

Q5: 发现了什么实验现象？

论文中揭示的关键实验现象包括：
1. Mixed SFT 的反直觉表现：尽管 Mixed SFT 在 pre-RLVR 阶段准确率最低，但经过 RLVR 微调后其性能上限最高，说明低预训练精度并不妨碍后续 RL 提升。
2. Mixed SFT 对 NCR 的全面优势：在域内数学推理和域外推理任务上，Mixed SFT 均达到更高的 post-RLVR 性能，且训练计算量减少 60 倍以上，挑战了 NCR 的必要性。
3. pre-RLVR 准确率与 post-RLVR 准确率之间不存在单调正相关，较高的 pre-RLVR 精度可能带来更差的最终结果，因此中间检查点评估会产生系统性偏差。
4. no-CoT 数据在 SFT 中的组织方式（如与长 CoT 混合还是顺序训练）对最终结果影响显著，其重要性不亚于是否包含 no-CoT 数据。
这些现象共同指向一个结论：评估 no-CoT 训练策略必须在完整后训练管线的末端进行，且简单的数据混合策略可以超越精心设计的 RL 方法。

Q6: 有什么可以进一步探索的点？

基于本文的发现，可以进一步探索的方向包括：
- 机制详解：深入分析 Mixed SFT 为何能带来更高的 post-RLVR 上限，例如在表征层面、隐式推理能力或 RL 训练稳定性上的影响。
- 条件扩展：研究 NCR 在哪些特定条件下（如极长 no-CoT 语料、复杂的多步骤推理任务）仍可能优于 Mixed SFT。
- 混合比例与顺序：系统地改变 no-CoT 与长 CoT 数据的混合比例、课程顺序，寻找最优数据配方。
- 更大规模与更多基座：在更大的模型规模、不同预训练数据分布和多种基座模型上验证结论的普适性。
- 跨模态迁移：将 Mixed SFT 思想推广到视觉、代码等具有类似 no-CoT 结构数据的领域。
- 评估协议设计：提出更鲁棒的后训练评估协议，避免中间检查点偏差，并建立 pre/post RLVR 性能关系的理论解释。
- 计算效率分析：更细致地剖析 60 倍计算差异的来源，是否有进一步压缩 RL 方法计算开销的途径。

Q7: 总结一下论文的主要内容

本文针对一个实际而重要的问题——如何有效利用 no-CoT 数据（如解题步骤、教科书推导）进行推理模型训练——展开了系统性研究。近年来，next-chunk 推理 RL（NCR）被提出用于此类数据：模型先生成隐式推理轨迹，再依据轨迹预测下一文本块的能力获得奖励。然而，现有评测大多只将 NCR 与仅使用 no-CoT 数据的传统 SFT 对比，这使得 NCR 的优势来源不明：是 RL 的公式本身，还是仅仅因为 NCR 让模型更充分利用了 no-CoT 数据？

作者设计了一套受控实验来回答这个问题。他们从同一个预训练基础模型出发，在统一的 RLVR 预算下，比较了五种策略：NTR（next-token reasoning RL）、NSR（next-sentence reasoning RL）、Sequential SFT（两阶段分别训练 no-CoT 和长 CoT 数据）、Mixed SFT（单阶段联合训练两类数据）以及 Reasoning SFT（仅长 CoT）。实验覆盖了域内数学推理和域外推理任务，并同时记录 pre-RLVR 与 post-RLVR 阶段的性能。

最核心的结果是：尽管 Mixed SFT 在 pre-RLVR 阶段精度最低，但在 post-RLVR 阶段它达到了最高的性能上限，并且在所有测评任务上一致领先于 NCR。更值得注意的是，Mixed SFT 的计算成本比 NCR 低超过 60 倍。这一结果有力地表明，NCR 的相对优势可能来自对比基线的选择不当，而非 RL 公式本身的优越性。

进一步的观察揭示了评估协议中的陷阱：更高的 pre-RLVR 准确率并不会带来更高的 post-RLVR 准确率，因此仅在中间检查点进行评测会产生系统性偏差。作者还发现，SFT 中 no-CoT 数据的组织方式（混合训练还是顺序训练）对最终结果影响巨大，甚至不亚于是否包含该数据。据此，他们提出应在完整后训练管线末端评估 no-CoT 训练策略，并给出了机制上的初步解释。

总的来说，本文为 no-CoT 数据训练策略提供了一次重要的视角修正：简单、高效的 Mixed SFT 应作为新方法的标准基线；同时，论文也提醒社区在比较训练方法时，必须考虑后续 RL 微调阶段的交互效应，而不能只依赖中间检查点的结果。这一发现对推理模型的后训练设计具有直接指导意义。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文是一个典型的“重新审视基线”式系统性工作，其受控比较设计、统一预算设置和完整管线评估思想，对智能体（agent）训练策略的比较同样适用。

## 基本信息

- 作者：Yinhao Tang, Youqing Fang, Yanan Sun, Jiangning Liu, Ziyi Wang, Xun Zhao, Weiming Zhang, Bin Liu, Kuikun Liu, Wenwei Zhang, Kai Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.23256v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据，但命中的片段主要来自摘要和引言，未能覆盖完整的实验与结论部分，因此部分细节为合理推断或推测。
