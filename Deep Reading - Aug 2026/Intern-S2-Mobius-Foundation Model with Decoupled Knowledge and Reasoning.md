---
user_id: "cheng tan"
paper_id: 8103
arxiv_id: "2608.14290"
title: "Intern-S2-Mobius: Foundation Model with Decoupled Knowledge and Reasoning"
institution: "Shanghai AI Laboratory"
publish_date: "2026-08-17"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.14290.pdf"
pdf_url: "https://arxiv.org/pdf/2608.14290"
abs_url: "https://arxiv.org/abs/2608.14290"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-18T02:17:27"
---
# Intern-S2-Mobius: Foundation Model with Decoupled Knowledge and Reasoning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：foundation model · decoupled knowledge and reasoning · transformer alternative · inference efficiency

## 一句话总结

本文提出 Mobius-v0 架构，通过将知识存储（共享 FFN）与推理计算（多个 Self-Attention Reasoner）解耦，以隐藏状态为缓存和载体实现迭代组合推理，在 7B 从头训练和 35B 继续预训练两个规模上验证了其以更少训练数据达到相近下游性能、并以近 4 倍端到端推理加速超越同等参数 Transformer 的可行性。

## 摘要

> We introduce Mobius-v0, an architecture that comprises a globally shared Memory (FFN) that stores knowledge vectors and multiple Reasoners (Self-Attn) that iteratively achieve compositional reasoning. Using hidden states as cache and carrier, reasoners repeatedly query memory for required knowledge-vectors, while the knowledge is transmitted back to reasoning operators. Through this knowledge-reasoning-separation architecture, Mobius achieves better knowledge compression and reasoning efficiency. Built upon Mobius-v0 architecture: 1) Our 7B model trained-from-scratch achieves similar downstream score as a 7B Transformer baseline with 62.6% of baseline's training data. 2) Our Intern-S2-Mobius, continually-pretrained from Qwen3.5-35B, achieves similar downstream score while delivering nearly 4x end-to-end inference speedup.

Q1: 这篇论文试图解决什么问题？

论文关注的是基础模型发展中的核心瓶颈问题：如何在参数规模和硬件限制之间取得平衡，并让模型具备持续吸收新知识和技能（自我进化）的能力。论文认为，当前 Transformer 架构并不满足自我进化的前提（证据片段 5.1 明确指出这一立场）。具体而言，传统 Transformer 将知识存储（FFN）与推理计算（Attention）紧密耦合，导致知识压缩效率低、推理路径固定且成本高，且难以在不遗忘旧知识的前提下持续注入新知识。另外，随着模型规模扩大，硬件需求急速增长，需要更高效的架构来提升单位参数和单位推理计算的价值。论文的核心问题是：能否通过架构层面的知识-推理解耦，同时改善知识压缩、推理效率和持续学习潜力。另外，在科学发现等需要跨领域组合泛化的场景中，现有模型在程序性问题求解上表现良好，但难以提出颠覆性科学假设，这也需要架构上的突破（来自 5.3 片段）。

Q2: 有哪些相关研究？

根据语义检索命中的片段，相关工作主要涉及三个方向：（1）注意力机制与架构设计：4.3 节讨论了 Mobius 与两大架构方向的关系——注意力机制（4.3.1）和硬件软件协同设计（5.4）。论文指出，它不是直接降低注意力复杂度，而是提高每次注意力操作提取的信息密度和价值，通过解耦知识来改变信息交互成本。（2）硬件与软件协同设计：5.4 节提到模型趋于更大参数规模，同时逼近物理硬件极限，论文的立场是当前 Transformer 架构不符合自我进化的前提，因此需要从架构层面重新设计信息流。（3）科学发现与推理：5.3 节提到当代 AI 在代码和数学上已很熟练，科学基础模型擅长程序性问题求解，但难以形成颠覆性科学假设；这暗示了通过解耦架构促进更多知识交互、提升组合泛化的动机。需要说明的是，由于未获取完整论文，这里只能基于有限片段概括，完整的 Related Work 可能还包括更多 Transformer 变体、持续学习、MoE 等工作，但目前证据不足。

Q3: 论文如何解决这个问题？

论文提出 Mobius-v0 架构，核心思想是解耦知识存储与推理计算。具体而言：（1）全局共享一个 Memory 模块（实现为 FFN），用于存储知识向量；（2）设置多个 Reasoner（实现为 Self-Attention），每个 Reasoner 负责组合推理；（3）以隐藏状态作为缓存和载体，Reasoners 反复查询 Memory 中所需的知识向量，并将知识传递回推理算子，形成迭代式组合推理。这种设计使信息流从 Transformer 中固定的逐层传递变为更灵活的激活路径，同时引入更动态的潜空间迭代，从而提升推理效率（来自 1.1 片段）。训练策略上，论文先在 7B 规模从头训练验证架构有效性，再选择从开源基础模型 Qwen3.5-35B 继续预训练（即 Intern-S2-Mobius），以避开从头预训练的高昂成本，在更大规模上验证性能。整体上，论文通过架构创新和两阶段训练验证，展示了知识-推理分离带来的效率和持续学习潜力。

Q4: 论文做了哪些实验？

基于摘要和检索片段，论文进行了以下实验：（1）7B 规模从头预训练对比实验：训练一个 7B 的 Mobius 模型，与 7B Transformer 基线比较，结果显示 Mobius 仅用基线 62.6% 的训练数据就达到相近的下游任务分数。（2）35B 规模持续预训练实验：从 Qwen3.5-35B 开始，切换为 Mobius 架构继续训练得到 Intern-S2-Mobius，与同等参数的 Transformer 对比，在达到相近下游分数的同时，实现近 4 倍端到端推理加速。（3）自进化（Self-Evolving）相关分析实验（5.1 节）：验证 Mobius 具有初步的知识-推理分离特性，在持续学习方面比 Transformer 表现出更大潜力，但真实自我进化场景尚未完全验证。（4）科学发现案例研究（5.3 节 / 附录 D）：涉及链式推理，但具体实验设置不清。需要说明的是，由于未获取完整论文，具体数据集、任务列表和基线细节缺失，这些只能基于现有证据的合理推断。

Q5: 发现了什么实验现象？

实验中观察到的关键现象包括：（1）效率优势来源：推理效率提升主要源于两方面——推理时更灵活的激活路径，以及更动态的潜空间迭代（来自 1.1 片段）。这暗示 Mobius 并非通过减少注意力计算量，而是通过提高每次操作的信息密度来获益。（2）数据效率：7B 从头训练时，用 62.6% 的训练数据达到基线同等分数，说明知识-推理解耦可能提高了知识压缩效率，使参数能被更有效地利用。（3）推理加速：35B 规模下近 4 倍端到端加速，且性能相近，说明架构优势在大规模下依然成立。（4）持续学习潜力：初步证据表明 Mobius 具备知识-推理分离特征，持续学习能力优于 Transformer，但尚未在真实自进化场景中验证（5.1 片段）。这些现象之间可能存在张力：例如训练数据减少与推理加速是否相互促进，以及“更好的持续学习”是否以牺牲其他能力为代价，文中未提供充分消融。

Q6: 有什么可以进一步探索的点？

基于检索片段和论文讨论，进一步探索方向包括：（1）真实世界自进化场景验证：论文明确表示，Mobius 能否在真实自进化场景中表现更好，需要基础模型、智能体系统和环境的联合设计（5.1 片段）。这为 Agent 方向提供了交叉切入点。（2）科学发现应用：5.3 节强调，让更多知识同时交互、增加跨域组合泛化的可能性，Mobius 可能为科学假设生成带来新机会，但目前只是展望，需要具体实验。（3）架构进一步优化：如何将知识-推理分离与 MoE、稀疏注意力等结合；如何设计更高效的 Memory 读取与 Reasoner 交互机制。（4）持续学习机制：探索在 Mobius 上加入真正的知识增量更新（如动态扩展 Memory 而不遗忘）。（5）硬件协同设计：针对 Mobius 的硬件实现与算子优化，利用其灵活的激活路径进一步提升实际计算效率。（6）规模扩展与训练策略：探索从头训练更大规模 Mobius 模型，以及与混合专家（MoE）等方法的结合。

Q7: 总结一下论文的主要内容

论文针对当前基础模型的两大瓶颈——参数规模扩展带来的硬件压力，以及 Transformer 架构在持续学习和自我进化方面的结构性局限——提出了一种全新的解耦架构 Mobius-v0。该架构将知识存储与推理计算分离：全局共享的 Memory（FFN）负责存储知识向量，多个 Reasoner（Self-Attention）负责迭代组合推理。隐藏状态作为两者之间的缓存和载体，Reasoners 反复查询 Memory 并接收知识回传，从而在潜空间中完成更动态的迭代推理。这种设计改变了 Transformer 的固定深度信息流，优化了激活路径，使每次注意力操作能提取并传递更高密度的信息。为验证架构有效性，论文进行了两个规模的实验：7B 模型从头预训练，在仅使用 Transformer 基线 62.6% 训练数据的情况下达到相近下游分数；35B 规模的 Intern-S2-Mobius 从 Qwen3.5-35B 继续预训练，在性能相近的同时实现近 4 倍的端到端推理加速。这两个结果共同支持知识-推理分离在压缩知识、提升推理效率上的优势。论文还讨论了该架构在持续学习和自我进化方面的潜力，认为其初步具备知识-推理分离特性，有潜力超越 Transformer 在持续学习中的表现，但真实自进化场景还需要与智能体系统和环境共同设计。此外，作者展望了在科学发现场景中，该架构可通过让更多知识同时交互来增强跨域组合泛化，助力颠覆性科学假设的提出。整体而言，论文不仅给出了一种可落地的架构替代方案，也重新定义了知识存储与推理计算的职责边界，为基础模型的发展提供了新思路。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文的知识-推理解耦思想与当前主流 MoE、稀疏注意力等架构优化形成对照，值得在架构设计参考。

## 基本信息

- 作者：Kai Chen, Jifeng Ding, Ning Ding, Jiaye Ge, Lixin Gu, Yicheng Gu, Qipeng Guo, Ermo Hua, Haian Huang, Haozheng Hou, Jie Hou, Xiangyu Hong, Che Jiang, Minxi Jin, Cheng Liang, Dahua Lin, Dawei Liu, Kuikun Liu, Chengqi Lv, Haijun Lv, Han Lv, Ningsheng Ma, Biqing Qi, Jianmin Qian, Shiya Su, Youbang Sun, Huanze Tang, Zhongbo Tian, Hanjing Wang, Rui Wang, Ting Wang, Yi Wang, Baiting Wu, Jun Xu, Bowen Yang, Hui Wang, Weida Wang, Haochen Ye, Jiashuo Yu, Shan Yu, Xiaoyi Yu, Qirui Zeng, Qi Zhang, Ming Zhang, Wenwei Zhang, Bowen Zhou, Xinyu Zhou
- 机构：Shanghai AI Laboratory
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-17
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.14290`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本报告参考了 PDF 语义检索证据，但证据来自摘要和少量章节片段，信息不完整；因此部分分析基于合理推断，并在相应位置标注。
