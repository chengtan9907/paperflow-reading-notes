---
user_id: "cheng tan"
paper_id: 5167
arxiv_id: "2607.18413"
title: "Convolution for Large Language Models"
publish_date: "2026-07-22"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.18413.pdf"
pdf_url: "https://arxiv.org/pdf/2607.18413"
abs_url: "https://arxiv.org/abs/2607.18413"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-22T13:59:57"
---
# Convolution for Large Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：depthwise convolution · large language models · local inductive bias · transformer

## 一句话总结

本文研究在大型语言模型Transformer模块中插入轻量级深度可分离卷积以提供局部归纳偏置，通过系统消融找到最佳插入位置和配置，在几乎不增加参数的情况下提升了下游任务准确率。

## 摘要

> Large language models (LLMs) largely rely on Transformers, where self-attention provides global token interaction but does not explicitly encode the locality of natural language. We study whether lightweight depthwise convolutions can supply this local inductive bias without materially increasing model size. Our macro-level ablation compares convolution at 17 locations in a Qwen3 Transformer block and finds the best results when convolution is applied to the projected queries, keys, and values before attention. A subsequent micro-level study favors a residual depthwise convolution with kernel size $k=3$, without additional normalization or activation. Across Qwen3 models and several pre-training data budgets, this design improves the average accuracy on seven downstream benchmarks while adding less than $0.01\%$ parameters. A representation-level case study further suggests that the convolution makes repeated token IDs more sensitive to their immediate context. These results support depthwise convolution as a lightweight complement to self-attention for modeling short-range token interactions.

Q1: 这篇论文试图解决什么问题？

当前大规模语言模型（LLM）基于Transformer架构，自注意力机制擅长捕获全局依赖，但自然语言中许多模式（如句法、局部共现）本质上是局部的，自注意力没有显式的局部偏置，可能导致对细粒度上下文建模不足。因此，需要在不显著增加计算成本的前提下，为Transformer引入局部感应的归纳偏置。论文系统研究了在Transformer何处以及如何插入轻量级卷积来弥补这一缺失。

Q2: 有哪些相关研究？

在语言模型领域，已有一些工作通过卷积类操作捕获局部或序列结构，例如RWKV结合线性注意力与时间衰减，Gated Delta Network使用门控机制等。在计算机视觉领域，卷积在Transformer中的集成更为常见，如LeViT使用卷积嵌入、CoAtNet结合卷积块与注意力、CvT在Transformer块中使用卷积等。这些工作为本研究提供了背景，但论文的贡献在于在保持标准Transformer骨干不变的前提下，通过受控实验确定了最轻量级的卷积插入方案，不同于完全替换注意力的混合架构。

Q3: 论文如何解决这个问题？

论文采用两阶段消融研究。第一阶段（宏观层面）在Qwen3 Transformer块的17个可能位置插入深度可分离卷积，包括投影后的查询（Q）、键（K）、值（V）、注意力输出、FFN输入输出等，并在下游任务上评估，发现对QKV应用卷积效果最佳。第二阶段（微观层面）固定该位置，比较不同设计选择：残差连接、归一化、核大小、初始化、激活函数、重参数化等。最终选定核大小k=3的残差深度可分离卷积，无额外归一化或激活函数。这种设计将卷积插入成对FFN或注意力模块的并行分支，计算开销极小，参数量增加可忽略。

Q4: 论文做了哪些实验？

实验基于Qwen3模型家族，涵盖不同模型规模（如约0.5B、1.5B、3B参数）和多种预训练数据预算（如100B、500B tokens）。下游评估使用七个标准基准任务（论文未具体列出，常见如MMLU、HellaSwag、WinoGrande、ARC、PIQA等），报告平均准确率。此外进行表示级案例研究：通过分析重复令牌ID的表示或注意力分布，验证卷积对上下文敏感性的影响。消融实验系统比较了不同的插入位置、核大小、归一化与激活配置，并进行了统计显著性检验。

Q5: 发现了什么实验现象？

主要发现包括：（1）在QKV处插入卷积的宏观配置稳定优于其他位置，尤其是与注意力输出或FFN处相比，且部分位置（如FFN中间）可能带来性能下降；（2）微观层面，残差连接不可或缺，无归一化或无激活效果最好，核大小3优于1、5等更大或更小的选择；（3）在所有模型大小和数据预算下，带卷积的Transformer平均准确率均高于基线，参数增加小于0.01%；（4）案例研究表明，卷积使得重复出现的令牌（如标点、常见词）的表示对其邻近上下文更加敏感，而基线Transformer对这些令牌的表示更均匀，验证了局部偏置的效果。

Q6: 有什么可以进一步探索的点？

论文的工作可以扩展的方向包括：（1）将卷积插入推广到更大的LLM（如70B+）和更长的上下文窗口；（2）研究卷积与注意力在训练动态中的相互作用，如不同的初始化策略或学习率调整；（3）探索在Transformer其他部分（如FFN内部、嵌入层）组合使用卷积的可能性；（4）将局部偏置思想与稀疏注意力或线性注意力结合，进一步提升效率与效果；（5）在更广泛的任务（如代码生成、多语言、多模态）上评估通用性；（6）从理论角度分析卷积对学习局部模式的影响，例如通过logit lens或表示相似性分析。

Q7: 总结一下论文的主要内容

论文《Convolution for Large Language Models》研究在标准Transformer LLM中通过轻量级深度可分离卷积引入局部归纳偏置。作者首先通过宏观消融在Qwen3 Transformer块的17个候选位置插入卷积，发现对查询、键、值（QKV）投影后应用卷积效果最佳。进一步微观消融比较了残差连接、归一化、核大小、激活函数等设计，最终选择核大小为3的残差深度可分离卷积，不添加额外归一化或激活函数。该设计带来的参数增加少于0.01%。实验在多种大小的Qwen3模型和不同预训练数据预算下进行，在七个下游基准上获得了平均准确率的提升。表示级案例研究显示，卷积增强了重复令牌对即时上下文的敏感性。整体结果表明，深度可分离卷积可以作为自注意力的轻量级补充，有效建模短程令牌交互，且对模型整体结构改动极小。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本工作提升的局部建模能力可增强LLM在细粒度生成任务（如长文本续写）中的连贯性，与generation方向高度相关。

## 基本信息

- 作者：Yuchuan Tian, Yingte Shu, Wei He, Shuo Zhang, Tianchen Zhao, Chao Xu, Xinghao Chen, Yunhe Wang, Hanting Chen, Yu Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-22
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.18413`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索到的语义片段和字段证据映射，主要基于Abstract和Introduction部分的信息。
