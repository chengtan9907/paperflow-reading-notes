---
user_id: "cheng tan"
paper_id: 14396
arxiv_id: "2609.37659v1"
title: "Are In-Context Images Worth 10 Dimensions?"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.37659v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.37659v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-07T01:33:31"
---
# Are In-Context Images Worth 10 Dimensions?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：in-context learning · large vision language models · mechanistic interpretability · dimensionality reduction

## 一句话总结

该论文发现大型视觉语言模型中存在跨图像分类任务共享的低维判别几何（Shared Discriminative Geometry, SDG），并理论化地证明线性自注意力可通过逐步梯度下降实现主成分投影式的降维，进而解释早期层对视觉表征的压缩机制。

## 摘要

> There has been significant work on understanding the In-Context Learning capabilities of Large Language Models, especially on the induction circuit. For a few-shot classification task, the induction circuit leverages linear representations of each labeled example in-context in order to classify an unlabeled query. However, few works focus on how those linear representations are built in the first place. Leveraging the expressivity of the vision modality compared to text, we uncover a Shared Discriminative Geometry (SDG) inside Large Vision Language Models (LVLMs). It is a low-dimensional space, shared across all image classification tasks, in which in-context images are compressed into linearly separable representations later used to perform classification. We observe that this is the result of the model performing a dimensionality reduction of vision representations in early layers. In order to explain this phenomenon: (1) We show analytically that linear self-attention can perform a dimensionality reduction by projecting in-context data onto its principal components, with each layer implementing one gradient descent step toward this objective. (2) We provide evidence that trained LVLMs reduce the dimensionality of vision representations in early layers via a similar mechanism.

Q1: 这篇论文试图解决什么问题？

论文要解决的核心问题是：在大语言模型中，induction circuit 依赖每个带标签的上下文样例的线性表征来完成少样本分类，但学界对“这些线性表征最初是如何构建的”研究甚少。传统研究集中在文本模态的 ICL 机制，对视觉模态中表征形成过程缺乏关注。论文试图回答三个具体问题：(1) 上下文图像在线性层面如何被压缩成低维、线性可分的判别表征；(2) 这种压缩发生在 LVLM 的哪些层、以何种方式实现；(3) 该机制是否可用一个可解析的理论回路（理论上的线性注意力回路）来解释。此外，作者发现基于相似度的分类在文本 token 上预测类别标签的效果远差于视觉 token，这一反直觉现象也构成问题起点：图像表征的形成并非简单地从输入视觉 token 转移到文本 token。

Q2: 有哪些相关研究？

相关工作包括三个方向：(1) LLM 的 In-Context Learning 研究，尤其是 Brown et al. (2020) 首次观察到 ICL 作为 LLM 的新兴能力，以及 Olsson et al. (2022) 提出的 induction circuit（归纳回路）机制，即模型通过线性表征比较上下文样例并执行分类；(2) LVLM 上的 ICL 扩展，如 Alayrac et al. (2022) 在多模态视觉-文本任务上观察到 ICL；(3) 机器学习的可解释性（Mechanistic Interpretability），目标是找到模型内部实现的算法回路。此外还涉及少样本学习中的原型网络（Prototypical Networks, Snell et al.）等经典方法作为对比框架。论文的定位是：在 induction circuit 已被充分研究的背景下，研究线性表征的“形成”过程，尤其是视觉模态中的表征形成。

Q3: 论文如何解决这个问题？

论文的解决路径分为理论建构与实证验证两部分。理论上，作者构建了一个线性非因果自注意力回路的数学模型，证明该回路能够执行降维操作：将上下文数据投影到其主成分方向，且每一层等价于朝该目标执行一步梯度下降。作者利用满秩初始化和行列式动力学排除收敛到低秩驻点的可能性，并证明误差 Eℓ 趋于 0，且在 Eℓ 足够小后具有指数收敛速率。实证上，作者在 LVLM（涉及 Qwen3.5 系列等模型）中进行少样本图像分类实验，提出并验证 SDG 概念：跨所有图像分类任务共享的低维线性可分空间。通过消融实验（如剪除 SDG 前 15 个维度导致准确率下降 12.9%，增强这些维度使准确率提升 3.9%，而剪除随机子空间影响可忽略）验证 SDG 的必要性；并通过识别与理论 OV 回路匹配的注意力头（约 2% 的头）进行剪枝实验，发现剪除这些头会损害降维效果，暗示 LVLM 实现了与理论回路相似的电路。论文还分析了上下文图像表示在两阶段过程中的形成。

Q4: 论文做了哪些实验？

论文的实验围绕少样本上下文图像分类展开：(1) SDG 存在性验证：在多种图像分类任务（如 Flowers 数据集）上检验跨任务共享的低维线性可分空间；(2) 维度需求分析：测量不同 LVLM 层上维持线性类别可分性所需维度随层数增加而减少，验证早期层降维假设；(3) 消融实验：剪除 SDG 前 15 个维度、增强这些维度、剪除随机子空间，对比准确率变化；(4) 注意力头剪枝实验：识别与理论 OV 回路匹配的注意力头（约 2%），剪除后观察降维效果受损；(5) 文本 token 与视觉 token 分类性能对比：验证相似度分类在文本 token 上预测类别标签效果远差于在视觉 token 上的表现；(6) 使用了包含幻觉标注的任务（如提供图像和文本声明，让模型判断幻觉或非幻觉，评测 200 个生成任务）和 NaturalBench 图像-标题匹配版本等评测基准。

Q5: 发现了什么实验现象？

实验揭示的主要现象：(1) 反直觉发现：基于相似度的分类在文本 token 上预测类别标签远差于视觉 token，说明线性图像表征形成不是从视觉 token 到文本 token 的简单转移；(2) 跨层维度收缩：随 LVLM 层数增加，保持线性类别可分性所需维度数下降，支持早期层执行降维的观点；(3) SDG 的必要性：剪除 SDG 前 15 个维度导致准确率下降 12.9%，增强则提升 3.9%，随机子空间剪除几乎无影响，说明 SDG 维度是模型分类能力的关键载体；(4) 头剪枝效应：仅剪除约 2% 与理论 OV 回路匹配的注意力头（从视觉 token 到其他上下文图像视觉 token 的注意力）就会损害降维，提示训练后的 LVLM 中实现了与理论回路相似的机制；(5) 在两个额外评测集（幻觉分类、NaturalBench 图像-标题匹配）上验证了任务的泛化性（基于检索证据中提及的评测内容，需注意这些证据来自引用片段，具体数值需要原文确认）。

Q6: 有什么可以进一步探索的点？

未来工作方向：(1) 验证同一机制是否泛化到纯文本 LLM 以及其他非分类任务；(2) 研究如何学习额外的几何结构以及这些几何如何交互（学习不同的几何表征空间）；(3) 探索 Table 2 中展示的方向，作为测试时修改和监控模型在上下文中学习内容的工具；(4) 更深入地研究论文提出的理论回路（theoretical circuit），包括其与真实模型的对应关系；(5) 扩展理论框架，解释 LVLM 如何近似线性非因果注意力（因为理论假设与真实注意力存在差距）；(6) 改进聚合方式，例如平均池化作为所有 token 聚合的假设需要更精细的处理。

Q7: 总结一下论文的主要内容

论文以“上下文图像是否值得 10 个维度”为问题切入，系统研究了 LVLM 中少样本上下文图像分类所依赖的线性表征的形成机制。全文论证主线为：先指出 induction circuit 依赖的线性表征如何构建是一个未解问题，再提出 SDG（跨任务共享的低维判别几何）的概念并验证其存在与必要性，然后通过理论证明（线性自注意力可做主成分投影式降维，每层实现一步梯度下降）和实验验证（早期层降维、特定注意力头剪枝效应）解释其形成机制，最后讨论局限与未来方向。技术主线上，作者构建了线性非因果自注意力回路的理论模型，利用满秩初始化和行列式动力学证明误差收敛。实验主线上，作者在多种图像任务上测量跨层线性可分性维度需求、执行 SDG 维度消融、识别并剪除 2% 匹配理论 OV 回路的注意力头，从正反两方面证实 LVLM 早期层以类似机制降维。结论强调，这种机制不是从输入视觉 token 到文本 token 的简单线性传递，而是一个主动的、有结构性的压缩过程。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文是视觉-语言模型可解释性与机制理解方向的前沿工作，聚焦在 ICL 的 induction circuit 之前一环节——线性表征的构建，与当前生成方向（权重 0.10）相关

## 基本信息

- 作者：Adhemar de Senneville, Xavier Bou, Jérémy Anger, Rafael Grompone, Gabriele Facciolo
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.LG
- 日期：2026-09-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.37659v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的证据片段（Introduction/Conclusion/References 共 8 条），并结合 heuristic_draft 与元数据进行了补全和校准；部分实验数值与评测细节来自检索片段，需要读者核对原文确认。
