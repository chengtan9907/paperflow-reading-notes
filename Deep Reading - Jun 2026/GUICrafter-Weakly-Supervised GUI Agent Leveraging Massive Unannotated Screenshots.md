---
user_id: "cheng tan"
paper_id: 1937
arxiv_id: "2606.29705"
title: "GUICrafter: Weakly-Supervised GUI Agent Leveraging Massive Unannotated Screenshots"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.29705.pdf"
pdf_url: "https://arxiv.org/pdf/2606.29705"
abs_url: "https://arxiv.org/abs/2606.29705"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-30T16:35:02"
---
# GUICrafter: Weakly-Supervised GUI Agent Leveraging Massive Unannotated Screenshots

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：gui agent · weakly-supervised learning · visual grounding · curriculum learning

## 一句话总结

提出 GUICrafter，一种利用大规模未标注截图进行弱监督训练的 GUI 代理，通过两阶段课程学习框架显著减少对昂贵人工标注的依赖。

## 摘要

> Data, as the fundamental substrate of modern intelligence, has greatly driven the development of current foundation models. Naturally, researchers aim to extend this paradigm to the domain of GUI agents, hoping to build strong GUI agents through a similar paradigm. However, GUI agent data cannot be directly harvested from the internet, making it costly and difficult to collect at scale. As a result, current GUI agents suffer from poor cross-device generalization and limited visual grounding ability for fine-grained GUI elements. As an attempt to address data challenge in GUI agents, we propose GUICrafter, a weakly-supervised GUI agent leveraging massive unannotated screenshots to substantially reduce the reliance on expensive human annotations. GUICrafter explores a curriculum learning framework for training GUI agents through two progressive stages. First, the model learns visual grounding from large-scale unannotated screenshots and webpages, leveraging the rich contextual signals inherent in GUI interactions without human annotations. Then, in Stage 2, we leverage a small amount of high-quality data to calibrate the model via reinforcement learning. Experiments show that GUICrafter achieves competitive, or even superior, performance to advanced systems like UI-TARS while using only 0.1% of its data. Furthermore, under the same amount of annotated data, GUICrafter surpasses all previous methods such as GUI-R1. Code, data, and models are available at https://github.com/fansunqi/GUICrafter.

Q1: 这篇论文试图解决什么问题？

当前 GUI 代理面临数据短缺问题：人工标注 GUI 交互数据成本高、难以规模化，导致代理跨设备泛化能力差，对细粒度 GUI 元素的视觉定位能力不足。本文试图通过弱监督方法，利用大规模未标注截图（如网页截图或电子设备界面截图）来降低对人工标注的依赖，提升视觉定位和泛化能力。

Q2: 有哪些相关研究？

现有 GUI 代理方法主要依赖人工标注的交互轨迹数据，如 UI-TARS、GUI-R1 等，这些方法数据成本高且难以覆盖多种设备和场景。近期有工作探索使用合成数据或模拟环境，但真实感不足。部分研究利用页面结构（如 DOM）辅助定位，但需要额外解析。GUICrafter 首次提出在无标注截图场景下通过交互信号自动生成训练数据，属于弱监督预训练范式，类似于 LLM 的文本预训练阶段。

Q3: 论文如何解决这个问题？

GUICrafter 采用两阶段课程学习框架：
1. 阶段一（弱监督 GUI 预训练）：从大规模未标注截图和网页中学习。利用 GUI 交互中固有的上下文信号（如点击目标、文本输入区域、滚动条等）自动定义元任务和动作，无需人工标注。模型在该阶段学习视觉定位和基本 GUI 理解。
2. 阶段二（强化学习校准）：使用少量高质量的人工标注数据，通过强化学习（如 PPO）对模型进行精细校准，提升任务执行准确性和安全性。
整体上，论文还构建了对应的两阶段数据集：大规模未标注截图数据集和小规模高质量数据集。

Q4: 论文做了哪些实验？

论文基于 Qwen2.5-VL-3B 基座模型，在 8 块 NVIDIA H20 GPU 上训练。数据分为 Web & Desktop 平台和移动设备平台。在多个 GUI 基准任务上（如点击预测、文本输入、滑动操作等）评估。主要基线包括 UI-TARS（使用大规模人工标注数据）、GUI-R1（使用监督学习）等。对比实验显示：GUICrafter 仅使用 UI-TARS 的 0.1% 数据量即达到相似或更优性能；在相同标注数据量下全面超越 GUI-R1。论文可能包含消融实验验证两阶段有效性。

Q5: 发现了什么实验现象？

未从检索证据中得到具体实验现象，但根据摘要可推断：
- 弱监督预训练阶段显著提升了视觉定位能力，使模型能够识别细粒度 GUI 元素。
- 强化学习校准阶段进一步提升了任务成功率和鲁棒性。
- 跨设备泛化能力优于依赖纯监督学习的方法。
- 数据效率极高：仅需少量标注数据即可匹配大规模标注方法的性能。

Q6: 有什么可以进一步探索的点？

1. 探索更复杂的交互信号和元任务设计，以覆盖更多 GUI 操作类型（如拖拽、手势等）。
2. 将框架扩展到更多设备和平台（如智能手表、车机界面）。
3. 结合多模态预训练（如语言-图像对齐）进一步提升视觉语言理解能力。
4. 研究如何自动生成高质量反馈信号以替代少量人工标注，实现完全无监督训练。
5. 探索更高效的强化学习策略，减少训练开销。

Q7: 总结一下论文的主要内容

该论文针对 GUI 代理数据稀缺问题，提出 GUICrafter 弱监督框架。核心贡献是：1) 提出一种利用大规模未标注截图进行弱监督预训练的方法，通过自动从交互信号中生成元任务和动作，免除了人工标注；2) 设计两阶段课程学习框架（弱监督预训练 + 强化学习校准），逐步提升模型能力；3) 构建了对应的数据集。实验表明，GUICrafter 在多个基准上仅使用 0.1% 的标注数据即可达到与 UI-TARS 相当的性能，且优于 GUI-R1。该工作展示了弱监督范式在 GUI 代理领域的潜力，为降低数据成本提供了新路径。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：与智能体方向高度相关，提出弱监督训练范式降低数据成本

## 基本信息

- 作者：Sunqi Fan, Lingshan Chen, Runqi Yin, Qingle Liu, Yongming Rao, Meng-Hao Guo, Shi-Min Hu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.CV
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.29705`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，但局限性、未来方向等部分基于摘要和推断补充。
