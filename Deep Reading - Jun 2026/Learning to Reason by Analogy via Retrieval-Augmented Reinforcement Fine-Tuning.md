---
user_id: "cheng tan"
paper_id: 118
arxiv_id: "2606.13680v1"
title: "Learning to Reason by Analogy via Retrieval-Augmented Reinforcement Fine-Tuning"
publish_date: "2026-06-11"
pdf_path: "/Users/mario/Documents/Obsidian Vault/Daily Note/Daily Note 2026/arXiv - Jun 2026/2606.13680v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.13680v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
report_version: "2026-06-04-v5"
daily_note_path: "/Users/mario/Documents/Obsidian Vault/Daily Note/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-14T23:56:43"
---
[[Daily Note - Jun 2026]]

- PDF：[https://arxiv.org/pdf/2606.13680v1](https://arxiv.org/pdf/2606.13680v1)

# Learning to Reason by Analogy via Retrieval-Augmented Reinforcement Fine-Tuning

- PDF: https://arxiv.org/pdf/2606.13680v1
- 原文: https://arxiv.org/abs/2606.13680v1

> ★★☆☆☆ 按需阅读 · 约 20 分钟 · 模型 openai-compatible/gemini-3-flash-preview · 证据 PDF 全文 + 元数据

🏷 关键词：retrieval-augmented generation · reinforcement learning · analogical reasoning · mathematical reasoning

## 一句话总结

论文提出了检索增强强化微调（RA-RFT）框架，通过训练具备推理感知能力的检索器并结合强化学习，使语言模型能够通过类比学习解决复杂的数学推理问题。

## 摘要

> Retrieval-augmented generation (RAG) has become a standard mechanism for grounding language models in external knowledge, yet conventional retrieval based on lexical or semantic similarity is poorly suited for complex reasoning tasks: a semantically similar problem may demand an entirely different solution strategy, while a superficially different problem may share the same underlying reasoning pattern. We propose Retrieval-Augmented Reinforcement Fine-Tuning (RA-RFT), a post-training framework that teaches language models to reason by analogy. RA-RFT uses gold-relevance distillation to train a retriever that ranks contexts by expected reasoning benefit rather than semantic overlap, and then fine-tunes the policy model via reinforcement fine-tuning methods with retrieved analogous demonstrations, so the model learns to leverage reasoning traces under verifiable outcome rewards. We further analyze the diversity of retrieved contexts and find that reasoning-aware retrieval surfaces complementary solution strategies that provide distinct reasoning scaffolds for individual problems. Across challenging mathematical reasoning benchmarks, RA-RFT consistently outperforms standard reinforcement fine-tuning methods. For example, it improves AIME 2025 average@32 accuracy by 7.1 and 2.8 points over GRPO for Qwen3-1.7B and Qwen3-4B respectively -- suggesting that reasoning-aware retrieval is a complementary axis of improvement and orthogonal to advances in reward design or training curricula.

Q1: 这篇论文试图解决什么问题？

该研究试图解决大语言模型在复杂推理任务中面临的两个核心瓶颈：
1. 参数化知识的局限性：现有的强化学习微调（RLVR）方法（如 DeepSeek-R1 采用的方法）高度依赖模型自身的参数知识。当遇到预训练阶段未充分覆盖的推理模式（如特定的组合恒等式或数论技巧）时，模型很难通过自我采样发现正确路径，导致奖励信号稀疏，学习停滞。
2. 传统检索与推理的不匹配：标准 RAG 依赖词汇或语义相似度。然而，在数学推理中，表面描述相似的问题可能解法迥异，而表面无关的问题可能共享相同的底层逻辑结构。现有的检索器无法识别这种“推理效用”（Reasoning Utility），导致检索到的内容往往是干扰项而非助攻。
3. 类比推理能力的缺失：人类专家在解题时会调用结构相似的既往案例，而模型缺乏这种通过类比外部示例来构建解题支架（Scaffold）的机制。

Q2: 有哪些相关研究？

相关研究主要分布在三个领域：
1. 强化学习微调（RLVR）：如 DeepSeek-AI 和 OpenAI 的工作，通过可验证奖励优化思维链（CoT），但这些方法通常是闭环的，不利用外部知识。
2. 检索增强生成（RAG）：Lewis 等人开创了将外部文档引入生成过程的先河，但在推理任务中，如何定义“相关性”一直是难题。近期研究如 QuestA 尝试注入参考答案，但对模型泛化能力要求极高。
3. 类比推理（Analogical Reasoning）：Gentner 等人的经典理论指出，类比的核心在于结构映射而非属性匹配。本论文将这一认知科学理论引入 LLM 的后训练阶段，通过检索结构相似的解题轨迹来辅助推理。

Q3: 论文如何解决这个问题？

RA-RFT 框架分为三个关键阶段：
1. 金牌相关性蒸馏（Gold-Relevance Distillation）：为了定义什么是“对推理有用的检索”，研究者使用教师模型（如 GPT-4o）作为裁判。对于一个目标问题，从语料库中采样候选问题，观察将候选问题的解题轨迹加入 Prompt 后，学生模型解决目标问题的成功率是否提升。以此构建包含（目标问题, 候选示例, 推理效用标签）的数据集。
2. 推理感知检索器训练（Reasoning-Aware Retriever Training）：基于上述蒸馏数据，对预训练检索模型（如 Reason-ModernColBERT）进行对比学习微调。目标是让检索器学习识别那些能提供解题思路、公式变形或逻辑支架的示例，而非仅仅是文字相似的题目。
3. 检索增强强化微调（RA-RFT）：在强化学习阶段（采用 GRPO 算法），将检索到的类比示例及其推理轨迹作为上下文输入。模型在尝试解决目标问题时，学习如何从这些示例中提取有用的推理模式。通过可验证的答案正确性作为奖励信号，模型不仅学会了做题，还学会了如何“利用检索到的参考资料”来辅助做题。

Q4: 论文做了哪些实验？

实验设计严谨，涵盖了多个维度：
1. 模型与基准：使用 Qwen3-1.7B 和 4B 作为基础模型。测试集包括 AIME 24/25、HMMT25、BrUMO25 等极具挑战性的数学竞赛题目。
2. 核心对比：将 RA-RFT 与标准的 GRPO（无检索强化学习）以及 OPSD 等 SOTA 方法进行对比。结果显示，在 AIME 2025 上，RA-RFT 将 Qwen3-1.7B 的准确率从 66.4 提升至 73.5（+7.1），将 4B 模型的准确率从 66.9 提升至 69.7。
3. 学习效率分析：实验发现 RA-RFT 在训练初期的收敛速度明显快于 GRPO。虽然初始时模型可能会被检索内容干扰（Step 0 准确率较低），但一旦开始强化学习，检索提供的“解题支架”能显著缓解奖励稀疏问题。
4. 消融实验：验证了推理感知检索器优于传统的 BM25 或语义检索器，并证明了检索到的“推理轨迹”比单纯的“问题-答案对”更有助于模型学习类比。

Q5: 有什么可以进一步探索的点？

论文提出了几个值得进一步探索的方向：
1. 领域扩展：将类比推理框架从数学扩展到代码生成、法律推理或科学发现（AI for Science）等领域，这些领域同样存在强逻辑结构和丰富的外部知识库。
2. 检索语料库的动态演进：目前使用的是静态语料库，未来可以研究如何让模型在强化学习过程中动态更新或自我生成更高质量的检索示例。
3. 检索与生成的深度耦合：探索更紧密的集成方式，例如在思维链的每一步中间动态触发检索，而非仅仅在开头提供上下文。
4. 异构检索：结合不同类型的外部信息（如公理库、计算工具调用示例）来进一步增强模型的推理边界。

Q6: 总结一下论文的主要内容

这篇论文提出了 RA-RFT，一种旨在提升大语言模型类比推理能力的后训练框架。针对传统强化学习在处理未知推理模式时奖励稀疏的问题，以及传统检索方法无法捕捉推理逻辑相似性的缺陷，RA-RFT 创新性地引入了推理感知检索。通过金牌相关性蒸馏技术，训练检索器识别对解题真正有益的示例；随后在强化学习过程中，引导模型学习如何利用这些外部示例构建自己的思维链。在 Qwen3 系列模型上的实验证明，该方法在 AIME 等高难度数学基准上取得了显著的性能提升，且这种提升与奖励函数设计或课程学习是正交且互补的。该工作为 RAG 与 RL 的结合提供了新的范式，强调了“推理效用”在检索中的核心地位。

## PDF 证据定位

- 研究背景：
  Introduction | score=1.116 | problems, and (3) reinforcement fine-tuning with retrieved demonstrations, which injects retrieved reasoning traces into training prompts and optimizes the target model via RLVR.…
  Introduction | score=1.106 | ransfers. This motivates a retrieval paradigm that selects examples whose reasoning traces, i.e. step-by-step solution strategies are maximally informative for the target…
  Introduction | score=1.104 | the distilled relevance labels. The trained retriever provides reasoning-analogous traces which augment the reinforcement fine-tuning process. reasoning utility (Su et al.…
- 核心方法：
  Method | score=1.013 | Our framework consists of three stages: (1) gold-relevance distillation to construct reasoning-utility-based retrieval supervision, (2) reasoning-aware retriever training via…
  Method | score=1.010 | solved examples, the model’s effective success rate on challenging problems increases, yielding denser reward signals without modifying the reward function or training…
  Method | score=0.999 | learning (Section 3.3), which in turn supplies retrieved demonstrations for reinforcement fine-tuning of the target model (Section 3.4). Figure 2 summarizes the complete…
- 主要结果：
  Results | score=1.045 | f each subfigure. Notably, RA-RFT starts at a lower step-0 accuracy than GRPO across all benchmarks, as the base model is initially distracted by the unfamiliar retrieved…
  Results | score=1.040 | We first compare RA-RFT against standard RLVR baselines and state-of-the-art methods (Section 4.2), then conduct ablation studies that disentangle the contributions of retrieval…
  Results | score=1.032 | e KL penalty. We use 64 H100 (80GB) GPUs for training all model variants. We use the AdamW optimizer with a learning rate of 1 × 10−6. The maximum rollout length is 32,768…
- 局限性：
  Conclusion | score=1.165 | We introduce Retrieval-Augmented Reinforcement Fine-Tuning (RA-RFT), a post-training framework that teaches language models to reason by analogy. The key insight is that…
- 相关性：
  Introduction | score=1.181 | outcome reward. Retrieval-Augmented Generation for Reasoning. While retrieval-augmented generation is effective for knowledgeintensive tasks (Lewis et al., 2020; Guu et al.…
  Introduction | score=1.153 | solutions into hard problems (Li et al., 2026), stepwise hints from teacher models (Zhang et al., 2025a), or expert demonstrations in lieu of verifiers (Cai and Provilkov, 2025).…
  Introduction | score=1.152 | er RL setting, Goyal et al. (2022) augment embodied agents with neural retrieval over past trajectories, conditioning the policy on historical experience beyond the current…

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：该论文直接关联“生成”方向，特别是如何通过外部干预提升生成内容的逻辑严密性。

## 基本信息

- 作者：Zilin Xiao, Qi Ma, Chun-cheng Jason Chen, Xintao Chen, Avinash Atreya, Hanjie Chen, Vicente Ordonez
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-06-11
- 推荐级别：**按需阅读**
- 预计阅读时间：约 20 分钟
- 解析来源：PDF 全文 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2606.13680v1`

## 代码与资源

- 代码：暂未发现公开链接
- 数据：暂未发现公开链接
- 项目主页：暂未发现公开链接
- 原文 PDF：https://arxiv.org/pdf/2606.13680v1
- arXiv：https://arxiv.org/abs/2606.13680v1
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成深度参考了 PDF 检索到的 Introduction、Method 和 Results 部分的证据片段，特别是关于三阶段框架的描述和 AIME 实验数据的校准。
- 方法证据锚点：Method | score=1.013 | Our framework consists of three stages: (1) gold-relevance distillation to construct reasoning-utility-based retrieval supervision, (2) reasoning-aware retriever training via…
- 结果证据锚点：Results | score=1.045 | f each subfigure. Notably, RA-RFT starts at a lower step-0 accuracy than GRPO across all benchmarks, as the base model is initially distracted by the unfamiliar retrieved…
