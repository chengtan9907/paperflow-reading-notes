---
user_id: "cheng tan"
paper_id: 2500
arxiv_id: "2607.02508"
title: "From SRA to Self-Flow: Data Augmentation or Self-Supervision?"
publish_date: "2026-07-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.02508.pdf"
pdf_url: "https://arxiv.org/pdf/2607.02508"
abs_url: "https://arxiv.org/abs/2607.02508"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-06T12:49:49"
---
# From SRA to Self-Flow: Data Augmentation or Self-Supervision?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：representation alignment · diffusion transformer · data augmentation · self-supervision

## 一句话总结

本文通过引入Attention Separation实验，证明从SRA到Self-Flow的性能提升主要来自双时步调度带来的噪声维度数据增强，而非跨噪声层token交互的表征自增强机制，并进一步验证了Attention Separation本身的数据增强效果。

## 摘要

> Representation alignment has become an effective way to accelerate diffusion transformer training and improve generation quality. Recent self-alignment methods, such as SRA and Self-Flow, further remove the dependency on external pretrained encoders by constructing alignment within the diffusion model itself. However, the mechanism behind the improvement from SRA to Self-Flow, dual-time scheduling, remains under-examined: Self-Flow attributes its gain to interactions between tokens at different noise levels, where cleaner tokens help infer noisier ones. In this work, we revisit this explanation and ask whether the gain instead comes from data augmentation along the noise dimension. To disentangle these factors, we introduce Attention Separation, which preserves the same dual-timestep input as Self-Flow while blocking attention between tokens assigned to different noise levels. Surprisingly, removing such interaction does not degrade performance and can even improve it, suggesting that the improvement from SRA to Self-Flow mainly comes from data augmentation. Furthermore, We show that Attention Separation itself provides an augmentation effect by splitting a single image into multiple effective training parts to expand the training data. Based on these observations, we combine self-representation alignment with dual-timestep and attention-separation augmentation, and demonstrate the effectiveness of this design on ImageNet. Code: https://github.com/vvvvvjdy/SRA/tree/main/SiT-SRA\_DTS\_AS

Q1: 这篇论文试图解决什么问题？

（1）Self-Flow中双时步调度带来的性能提升，其根本原因究竟是跨噪声层token注意力交互实现更强自对齐，还是仅仅通过引入不同噪声水平输入起到了数据增强作用？为区分两者，需要设计实验隔离跨层交互的影响。（2）Attention Separation即阻断跨层交互的操作本身是否可以作为数据增强方法？如果阻断反而提升性能，则表明该方法具有独立价值。论文通过解决这两个问题，旨在更清晰地理解自对齐机制，并提出更高效的训练策略。

Q2: 有哪些相关研究？

表示对齐（Representation Alignment）方法如REPA通过强制模型中间特征与外部预训练编码器（如CLIP）对齐来加速扩散Transformer训练。自对齐方法SRA去除了外部编码器依赖，利用模型自身不同层特征进行对齐。Self-Flow在SRA基础上引入双时步调度（DTS）和跨层注意力交互，声称实现更强自对齐。数据增强在扩散模型中早有应用，如噪声条件增强、多尺度训练等，但本文关注的噪声维度数据增强将不同噪声水平视为训练样本多样性的来源。Attention Separation类似于注意力掩码操作，但本文首次将其系统性地用于诊断和增强。

Q3: 论文如何解决这个问题？

论文提出Attention Separation（AS）作为诊断工具：在Self-Flow的双时步输入设置中，通过注意力掩码只允许相同噪声级别的token互相交互，完全阻断跨级交互。如果跨级交互是改进关键，阻断应导致性能下降；若性能不变或提升，则证明增益来自双时步的数据增强效果。实验证明后者。基于此，论文进一步将AS用作数据增强：在单时步设置中，为不同token分配不同噪声级别并应用AS，强制模型学习多噪声视图。最终，将自对齐目标（SRA）、双时步调度（DTS）和AS整合为统一训练方案（SRA+DTS+AS），在ImageNet 256×256上评估。

Q4: 论文做了哪些实验？

在ImageNet 256×256图像生成任务上基于SiT架构进行实验。比较条件包括：SRA基线（仅自对齐）、Self-Flow基线（SRA+DTS）、双时步+Attention Separation变体（DTS+AS）、单时步+Attention Separation增强（SRA+AS）、以及最终组合方案（SRA+DTS+AS）。评价指标为FID和Inception Score（IS）。还对比了外部编码器对齐方法REPA。实验控制：除注意力掩码外其他配置完全一致，以隔离变量。

Q5: 发现了什么实验现象？

（1）在双时步设置中，添加Attention Separation阻断跨层交互后FID不升反降，直接否定了跨层注意力交互是性能提升的关键，支持数据增强假说。（2）独立使用Attention Separation（单时步+不同噪声分配）也能带来性能提升，表明其本身具有数据增强效果。（3）最终方案SRA+DTS+AS在FID和IS上全面优于Self-Flow，并与REPA持平或更优。（4）消融实验显示DTS的增益主要来自输入多样性，而非交互。（5）训练过程曲线表明AS增强在早期即带来优势并持续保持。

Q6: 有什么可以进一步探索的点？

（1）将Attention Separation增强扩展到其他任务（文本到图像、视频生成）和架构（U-Net、DiT变体）。（2）理论分析双时步增强和注意分离增强的统计正则化效果。（3）探索自适应噪声分配策略，根据内容复杂程度动态决定每个token的噪声级别。（4）将注意分离与其他训练技巧（如知识蒸馏、对抗训练）结合。（5）研究跨噪声层级交互是否在特定场景（如小样本、高分辨率）下仍有价值。（6）对自对齐信号进行分解分析，区分语义对齐与噪声信息对齐的作用。

Q7: 总结一下论文的主要内容

论文从质疑Self-Flow性能提升来源出发，设计Attention Separation实验，证明双时步调度的主要贡献是噪声维度的数据增强而非自对齐增强。进一步发现Attention Separation本身也可作为数据增强方法。最终构建了包含自对齐、双时步调度和注意分离增强的统一训练方案，并在ImageNet 256×256上验证了有效性。论证主线为：提出假说→设计诊断实验→验证假说→利用发现构建新方法。技术主线为：SRA自对齐目标 → DTS双时步输入 → AS注意分离掩码。实验主线为：对比SRA、Self-Flow、DTS+AS和最终方案，消融各组件贡献。论文的主要贡献在于澄清了自对齐方法的关键机制，并提供了诊断和增强工具。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：这篇论文直接针对生成领域中的扩散Transformer训练加速问题，与用户画像中的生成方向（权重0.1）高度相关。

## 基本信息

- 作者：Dengyang Jiang, Mengmeng Wang, Harry Yang, Jingdong Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.02508`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（retrieved_evidence），特别是Abstract和Introduction部分的片段，并结合元数据和启发式草稿进行补充。
