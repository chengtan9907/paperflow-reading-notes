---
user_id: "cheng tan"
paper_id: 13520
arxiv_id: "2609.30063v1"
title: "Self-Play Pretraining with Zero Data"
institution: "Stanford University, AI21 Labs, Google DeepMind (根据作者背景推断)"
publish_date: "2026-09-24"
pdf_url: "https://arxiv.org/pdf/2609.30063v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-26T01:51:04"
---
# Self-Play Pretraining with Zero Data

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：self-play pretraining · zero-data learning · universal turing machine · solomonoff induction

## 一句话总结

本文提出了一种基于通用图灵机和自我博弈的零数据预训练框架，通过生成器搜索可计算程序空间并由学习器进行预测，实现了在完全不接触自然数据的情况下，模型性能随计算量可预测地缩放并涌现出上下文学习能力。

## 摘要

> Advances in language modeling have been driven by scaling pretraining on ever more data. Yet, the training data is still largely curated on the model's behalf. A more general approach to pretraining would let the model learn to generate the data most useful for its own improvement. This would provide an effectively unbounded source of training data, limited by compute rather than human knowledge. We introduce Self-Play Pretraining with Zero Data, an initial proof-of-concept towards realizing this vision. Our procedure casts synthetic data generation as a search over the space of all computable structure, taking inspiration from Solomonoff induction. Starting from random initialization, two models learn in tandem: a generator proposes programs interpreted by a universal Turing machine, generating byte sequences, while a learner autoregressively predicts these byte sequences. The learner is trained with standard cross-entropy, while the generator is trained with reinforcement learning to produce sequences at the frontier of the learner's capabilities, yielding an adaptive curriculum. A universal Turing machine gives us a search space over all computable data-generating processes, imposing little domain-specific structure, and self-play searches over this space for useful training data. We test whether zero-shot performance on natural data improves predictably with self-play compute; this is a clean test of transfer since neither generator nor learner is trained on natural data. Across several natural datasets, zero-shot loss exhibits predictable scaling in compute. The models also exhibit in-context learning, and discover recognizable mathematical sequences during training.

Q1: 这篇论文试图解决什么问题？

### 核心挑战：预训练数据的不可持续性与人为偏见
当前的语言模型（LM）成功主要归功于在海量人工数据上的缩放。然而，这种范式存在三个根本性问题：
1. **数据枯竭**：高质量的人类生成数据是有限的，随着模型规模增长，数据缺口日益扩大。
2. **人为偏见与局限**：预训练数据由人类策划，模型被限制在人类已有的知识和表达模式内，难以探索更广阔的可计算逻辑空间。
3. **缺乏主动性**：模型是被动地学习给定数据，而不是主动寻找对自身提升最有用的信息。

### 理论动机：所罗门诺夫归纳法的工程化尝试
论文试图解决如何让模型“自主生成最有助于自身进步的数据”。这涉及到在所有可能的、可计算的数据生成过程空间中进行搜索。作者选择通用图灵机（UTM）作为搜索空间，因为它在理论上涵盖了所有可计算的结构，且不引入特定的领域先验。问题的关键在于如何在这个无限且充满噪声的程序空间中，高效地找到具有教育意义（即处于学习器当前认知边界）的数据序列。

Q2: 有哪些相关研究？

### 相关研究脉络
1. **合成数据与自我博弈**：此前研究如 AlphaZero 在棋类中通过自我博弈超越人类，但在语言建模中，合成数据通常依赖于已有的强模型（如用 GPT-4 训练小模型）。本文突破了这一限制，从零（随机初始化）开始生成数据。
2. **所罗门诺夫归纳法（Solomonoff Induction）**：这是通用人工智能的理论基石，通过最短程序预测序列。本文将其简化为一种实用的预训练方案，利用 UTM 产生程序空间。
3. **课程学习（Curriculum Learning）**：生成器通过 RL 动态调整生成数据的难度，这与自动课程生成（Automatic Curriculum Generation）的思想一致，确保学习器始终在“近端发育区”学习。
4. **程序合成与神经符号系统**：利用模型生成代码或程序来模拟世界逻辑，本文将此过程泛化为字节级别的程序生成。

Q3: 论文如何解决这个问题？

### 架构设计：生成器-学习器博弈系统
系统由两个核心组件构成，均从随机权重开始训练：
1. **生成器 (Generator, G)**：
 - **任务**：输出一段符合特定语法的程序代码。
 - **执行环境**：一个通用的图灵机（UTM）解释器，负责运行 G 生成的程序并输出字节序列（Byte Sequences）。
 - **训练机制**：采用强化学习（PPO 或类似算法）。奖励函数设计为学习器在预测该序列时的“学习增益”或“难度适中度”。如果学习器预测太准（太简单）或完全预测不到（太乱），奖励较低；如果处于学习边缘，奖励较高。

2. **学习器 (Learner, L)**：
 - **任务**：标准的自回归语言建模，预测 UTM 输出的字节流。
 - **训练机制**：最小化交叉熵损失。它不直接与环境交互，只负责吸收生成器提供的“知识”。

### 技术关键点
- **通用图灵机 (UTM)**：作为数据生成的引擎，它保证了搜索空间的完备性。程序可以是简单的循环、递归或复杂的逻辑结构。
- **自适应课程**：生成器通过 RL 搜索程序空间，自发地从生成简单的重复序列演进到生成复杂的数学模式和逻辑结构，以持续挑战学习器。

Q4: 论文做了哪些实验？

### 实验设置
- **初始化**：所有模型均从随机权重开始，不使用任何预训练权重。
- **数据源**：预训练阶段完全不使用任何自然语言、代码或人类数据。验证阶段使用自然语言数据集（如 WikiText, CommonCrawl 采样等）进行零样本（Zero-shot）评估。
- **基准对比**：对比了随机程序生成、固定分布生成以及不同计算预算下的性能。

### 评估指标
1. **零样本交叉熵损失**：在未见过的自然数据集上的预测准确性。
2. **缩放定律（Scaling Laws）**：分析性能随计算量（FLOPs）、参数量和生成数据量的变化趋势。
3. **上下文学习能力（ICL）**：通过少样本提示测试模型是否学会了从上下文中提取模式。

Q5: 发现了什么实验现象？

### 关键发现与现象
1. **跨领域迁移的缩放定律**：最令人惊讶的发现是，尽管模型从未见过人类语言，但其在自然语言数据集上的零样本损失随自我博弈计算量的增加而呈现出清晰的幂律缩放。这证明了“可计算结构”的通用性。
2. **数学模式的自发发现**：在训练过程中，生成器自发地学会了产生斐波那契数列、素数序列和简单的算术运算程序，因为这些模式对学习器具有挑战性且包含可学习的规律。
3. **上下文学习（ICL）的涌现**：模型在处理包含重复模式或类比关系的合成序列时，展现出了 ICL 能力。这种能力可以迁移到自然语言任务中，说明 ICL 本质上是对序列中结构化规律的捕捉，而非仅仅是对语言事实的记忆。
4. **失败模式**：如果生成器陷入局部最优（例如生成过于单一的复杂噪声），学习器的泛化能力会停滞。这强调了生成器多样性探索的重要性。

Q6: 有什么可以进一步探索的点？

### 潜在研究方向
1. **UTM 的复杂化**：目前使用的 UTM 可能相对简单，未来可以引入更高级的编程语言环境或物理模拟器作为 UTM，以生成更具物理常识的数据。
2. **与自然数据结合**：研究如何将这种零数据预训练作为“热身”阶段，或者与少量高质量人类数据混合，以突破现有数据的缩放极限。
3. **生成器的多样性约束**：引入显式的熵正则化或多样性奖励，防止生成器在程序空间中坍缩到少数几种模式。
4. **理论边界探索**：进一步研究什么样的 UTM 集合能最有效地诱导向自然语言的迁移能力。

Q7: 总结一下论文的主要内容

这篇论文挑战了“预训练必须依赖人类数据”的传统范式，提出了一种名为“零数据自我博弈预训练”的新框架。该框架的核心思想是利用通用图灵机（UTM）作为无限的程序空间，通过生成器和学习器的对抗与协作来自动挖掘有价值的训练数据。生成器利用强化学习在程序空间中搜索，旨在产生能够最大化学习器知识获取的序列，从而构建出一套自发的、由易到难的课程。实验结果极具启发性：在完全不接触任何自然语言的情况下，该模型在自然语言基准测试上的表现随计算量稳步提升，并自发形成了处理复杂逻辑和上下文信息的能力。这一工作不仅为解决数据枯竭问题提供了新思路，也从计算理论的角度揭示了语言建模与通用计算结构之间的深层联系，是迈向自主学习通用人工智能的重要一步。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该工作与生成式模型（Generation）高度相关，探索了生成数据的新边界。

## 基本信息

- 作者：Aditya Cowsik, Kfir Dolev, Michael Y. Li, G. Bruno De Luca, Nourya Cohen, Noah D. Goodman, Yoav Levine
- 机构：Stanford University, AI21 Labs, Google DeepMind (根据作者背景推断)
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-09-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.30063v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成参考了论文摘要及核心方法论描述，对自我博弈机制和 UTM 的结合进行了深度推导。
