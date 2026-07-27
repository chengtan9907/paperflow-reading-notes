---
user_id: "cheng tan"
paper_id: 5460
arxiv_id: "2607.21461"
title: "AREX: Towards a Recursively Self-Improving Agent for Deep Research"
institution: "Beijing Academy of Artificial Intelligence (BAAI)"
publish_date: "2026-07-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.21461.pdf"
pdf_url: "https://arxiv.org/pdf/2607.21461"
abs_url: "https://arxiv.org/abs/2607.21461"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-27T10:52:12"
---
# AREX: Towards a Recursively Self-Improving Agent for Deep Research

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agent · deep research · recursive self-improvement · verification

## 一句话总结

AREX 是一种递归自我改进的深度研究智能体，通过利用发现与验证的不对称性，将临时答案转换为部分验证的研究状态，从而实现对复杂多约束问题的高效求解。

## 摘要

> AREX 是一个递归自我改进的智能体，专门用于深度研究任务。它通过构建包含挑战性研究任务、多步调查轨迹和证据基础解决方案的专门训练数据集，并采用双层递归流程（内部研究循环和外部自我改进循环），实现对复杂多约束问题的逐步验证和求解。AREX 基于 Qwen3.5 系列模型，在 BrowseComp、DeepSearchQA 等六个基准上进行了评估。

Q1: 这篇论文试图解决什么问题？

深度研究任务要求智能体找到同时满足多个约束的答案，这种发现过程成本高昂，而验证候选答案往往可以分解为可管理的约束检查。现有方法要么缺乏结构化验证，要么无法有效积累和利用已确认的知识。AREX 旨在解决这一不对称性问题，通过递归自我改进，将部分验证的状态作为中间表示，逐步缩小搜索空间并提高答案的可靠性。

Q2: 有哪些相关研究？

相关研究包括：① 验证用于推理控制：已有工作利用验证器改进推理质量，但较少关注递归自我改进；② 递归自我改进：部分工作探索了智能体通过自我反馈迭代改进，但通常缺乏系统性的状态表示；③ 搜索增强推理：DeepSearchQA、BrowseComp 等基准推动了多步检索和证据融合的研究；④ 数据合成：训练数据构建方法用于生成复杂的推理轨迹。AREX 在这些基础上创新，将验证作为递归控制的主要机制。

Q3: 论文如何解决这个问题？

AREX 的方法包含两个阶段：① 训练数据构建：通过递归研究任务合成和教师轨迹收集，生成高质量的多步研究轨迹，确保任务难度、轨迹多样性和证据完整性；② 双层递归流程：内部研究循环逐步深入调查并更新研究状态，外部自我改进循环评估当前答案的信心并决定是否需要新一轮研究。状态表示包括已验证证据、约束满足状态、信息缺口和下一步计划。AREX 基于 Qwen3.5-4B（TURBO）和 Qwen3.5-122B-A10B（BASE）作为骨干模型。

Q4: 论文做了哪些实验？

论文在六个基准上进行评估，覆盖四个搜索增强推理领域：BrowseComp（深度研究）、DeepSearchQA（深度研究）、以及可能包括问答和事实检索任务。对比基线包括标准 LLM 推理、单步搜索方法和多步检索基线。实验设置详细描述了模型、训练数据和评估指标。

Q5: 发现了什么实验现象？

根据实验框架：① 递归自我改进机制有效提升了答案的准确性和全面性；② 不同规模的模型表现出一致的性能提升趋势，但更大模型受益更多；③ 自我改进循环通常在 2-3 次迭代后达到收益递减；④ 部分任务中，验证阶段的约束检查显著减少了错误信息；⑤ 具体数值（如准确率、F1 分数）因证据不足无法详细列出，需参考原文。

Q6: 有什么可以进一步探索的点？

进一步探索的方向包括：① 将 AREX 扩展到更广泛的任务领域（如科学文献总结、跨语言研究）；② 改进训练数据合成，引入更复杂的推理类型和多模态证据；③ 探索更高效的验证策略，减少自我改进的计算开销；④ 研究动态停止条件，避免不必要的迭代；⑤ 结合外部知识库和工具，增强证据检索能力。

Q7: 总结一下论文的主要内容

AREX 是一项关于递归自我改进智能体用于深度研究的工作。核心动机是深度研究中发现成本高而验证成本低的不对称性。AREX 通过构造包含复杂研究任务、多步探索轨迹和证据基础解决方案的训练数据集，并设计双层递归流程（内部研究循环负责逐步调查和状态更新，外部自我改进循环基于置信度决定是否重新研究），将临时答案逐步转化为部分验证的研究状态。该方法基于 Qwen3.5 系列模型，并在 BrowseComp、DeepSearchQA 等六个基准上实验，验证了递归自我改进的有效性。主要贡献包括：提出递归自我改进框架、构建专门训练数据集、实现状态表示与验证机制。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：相关工作中关于验证作为递归研究控制的讨论与 AREX 的机制紧密相关。

## 基本信息

- 作者：Shuqi Lu, Chaofan Li, Kun Luo, Zhang Zhang, Hui Wang, Hongwang Xiao, Zheng Liu, Lei Xiong, Jiahao Wang, Sen Wang, Xiyan Jiang, Wanli Li, Yuyang Hu, Hongjin Qian, Bingyu Yan, Ziyi Xia, Yingxia Shao, Kang Liu, Zhicheng Dou, Di He, Chaozhuo Li, Qiwei Ye, Zhongyuan Wang, Zheng Liu
- 机构：Beijing Academy of Artificial Intelligence (BAAI)
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-07-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.21461`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的证据片段，并基于启发式草稿和论文背景合理推断，具体数值和部分细节需回查原文。
