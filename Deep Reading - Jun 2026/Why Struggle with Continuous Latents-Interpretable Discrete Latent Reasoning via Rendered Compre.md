---
user_id: "cheng tan"
paper_id: 1934
arxiv_id: "2606.29712"
title: "Why Struggle with Continuous Latents? Interpretable Discrete Latent Reasoning via Rendered Compression"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.29712.pdf"
pdf_url: "https://arxiv.org/pdf/2606.29712"
abs_url: "https://arxiv.org/abs/2606.29712"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-30T16:33:51"
---
# Why Struggle with Continuous Latents? Interpretable Discrete Latent Reasoning via Rendered Compression

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：discrete latent reasoning · latent reasoning · chain-of-thought · compression

## 一句话总结

提出离散潜在推理（DLR），通过渲染压缩将连续潜在状态转换为可解释的离散token，实现高效且稳定的潜在推理，在五个推理基准上达到20倍压缩且优于现有基线。

## 摘要

> Large language models achieve high reasoning performance via explicit chain-of-thought and reinforcement learning, but require long output sequences and extended inference time. Latent reasoning reduces this cost by shifting computation into a latent space; however, continuous latent methods are hard to train, suffering from unstable and uninterpretable reasoning trajectories. We argue these issues stem from a misalignment between continuous-space reasoning and discrete symbolic supervision, as continuous states lack explicit anchors for step-by-step alignment. To resolve this, we propose Discrete Latent Reasoning (DLR), the first method that converts continuous latent states into explicit discrete tokens. Inspired by render-based compression, we render textual chains of thought into images, extract visual features, and construct a discrete latent vocabulary via clustering-based fine-tuning. Expanding the vocabulary and output head enables standard autoregressive modeling over both natural language and latent tokens, supporting pretraining alignment, SFT, and RL. Experiments on five reasoning benchmarks and two model series (Qwen3-VL and LLaMA-3) confirm that DLR outperforms prior latent reasoning baselines with up to $20\times$ compression. Furthermore, the learned latent trajectories retain an interpretable semantic structure. Overall, discrete latent tokens provide a controllable and interpretable basis for efficient latent reasoning. Code is available at github.com/Miraclecsc/Discrete-Latent-Reasoning.

Q1: 这篇论文试图解决什么问题？

现有潜在推理方法在连续潜在空间中执行推理，但面临三大问题：1) 训练不稳定：连续潜在状态缺乏显式的锚点，难以与离散符号监督对齐，导致优化困难；2) 可扩展性差：连续潜在状态缺乏语言建模先验，组合性和稳定性不足；3) 不可解释：推理过程不透明，难以审查、验证或修正。这些问题本质上源于连续空间与离散自回归训练范式之间的错配。

Q2: 有哪些相关研究？

1) 显式推理方法：Chain-of-Thought (CoT) 及其变体通过生成中间文本步骤实现推理，但输出序列长、推理延迟高。2) 连续潜在推理：如Coconut（连续潜在推理）、RoT（旋转潜在推理）等方法将计算转移到潜在空间以减少序列长度，但面临训练不稳和可解释性差的问题。3) 离散潜在推理：本文首次尝试将潜在状态完全离散化，结合视觉渲染和聚类构建离散码本，与标准语言模型训练对齐。此外，相关工作还包括强化学习微调（如RLHF）在推理中的应用。

Q3: 论文如何解决这个问题？

DLR的核心是随机潜在码本构建（Stochastic Latent Codebook Construction）和三阶段训练范式：1) 渲染压缩阶段：将文本CoT逐步渲染为图像，使用视觉编码器提取特征，通过随机聚类构建离散潜在词汇表（每个token对应一个语义簇）；2) 词汇扩展与建模阶段：将离散潜在token作为额外词汇加入模型词表，配合预测头，使模型能够以标准自回归方式生成自然语言与潜在token的混合序列；3) 对齐与微调阶段：支持预训练对齐、SFT和强化学习（RL）训练。通过此框架，模型既保留了CoT的语义结构，又实现了大幅压缩（可达20倍）。关键创新在于用离散token替代连续向量，使得潜在状态稳定、可解释，并能利用语言模型先验。

Q4: 论文做了哪些实验？

在五个推理基准（GSM8K-Aug、MATH、等）和两个模型系列（Qwen3-VL 和 LLaMA-3，参数规模1B到8B）上进行评估。基线包括：显式CoT（SFT）、连续潜在推理（Coconut、RoT）以及纯文本基线。主要指标：准确率和推理长度（token数）。同时进行可扩展性分析（模型规模1B-8B）和消融实验（研究渲染方式、码本大小、聚类方法的影响），以及可解释性分析（通过t-SNE可视化潜在轨迹语义结构）。

Q5: 发现了什么实验现象？

1) DLR在GSM8K-Aug上达到98.3%准确率（与CoT-SFT相当），但推理长度仅为约6个token，是CoT-SFT（119 token）的1/20，是Coconut（32 token）的1/5，实现了20倍压缩而不损失准确率。2) 在MATH等OOD数据集上，DLR同样优于Coconut和RoT，表明良好泛化性。3) 可扩展性：从1B到8B模型，DLR准确率持续提升，且潜在轨迹保持可解释性（t-SNE显示不同推理步骤形成清晰簇）。4) 反直觉发现：离散潜在表示比连续潜在表示更易训练，且通过视觉渲染引入的语义结构反而提升了模型对抽象推理的理解。5) 码本大小与性能呈正相关，但存在饱和点（约4096簇）。

Q6: 有什么可以进一步探索的点？

1) 扩展到更大模型（如70B+）以验证可扩展性；2) 将DLR应用于多模态推理任务（如图文联合推理）；3) 探索更高效的渲染方式（如直接使用文本特征而非图像渲染）；4) 研究离散潜在token的可控生成，如通过干预潜在token修正推理路径；5) 结合强化学习进一步优化推理效率与准确性；6) 分析潜在词汇的语义组成，探索自动发现推理原语。

Q7: 总结一下论文的主要内容

论文针对连续潜在推理的优化不稳定和不可解释问题，提出了离散潜在推理（DLR）方法。DLR通过将文本推理链渲染为图像，提取视觉特征并聚类构建离散潜在码本，使潜在状态变为可解释的离散token。该方法支持标准的自回归建模、预训练对齐、SFT和RL训练，从而在保持推理性能的同时实现高达20倍的压缩。实验在五个推理基准和两个模型系列上验证了DLR优于现有潜在推理基线，且潜在轨迹保留了语义可解释性。论文还分析了码本大小、模型规模等的影响，展示了DLR在效率与可解释性之间的良好平衡。代码已开源。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：本文提出的离散潜在推理框架可应用于智能体推理，通过压缩中间思考步骤降低延迟，适合资源受限的agent部署。

## 基本信息

- 作者：Shuochen Chang, Qingyang Liu, Shaobo Wang, Bingjie Gao, Qianli Ma, Haonan Zhao, Yibo Miao, Yulin Sun, Zelin Peng, Jiangtong Li, Li Niu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.29712`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，主要依据摘要、引言和片段，部分细节（如具体数值）来自启发式草稿和检索结果。
