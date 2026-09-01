---
user_id: "cheng tan"
paper_id: 9641
arxiv_id: "2608.27881"
title: "StreamEMS: Streaming Video Understanding with Self-Evolving Memory Scheme for Vision-Language Models"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.27881.pdf"
pdf_url: "https://arxiv.org/pdf/2608.27881"
abs_url: "https://arxiv.org/abs/2608.27881"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-01T01:12:23"
---
# StreamEMS: Streaming Video Understanding with Self-Evolving Memory Scheme for Vision-Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：streaming video understanding · self-evolving memory · vision-language model · external memory

## 一句话总结

StreamEMS 通过引入语义进化与先验信息进化两个模块，对流式视频理解中的外部记忆进行自进化重构，在 OVO-Bench 和 StreamingBench 上取得优于现有方法的性能，并在高 token 使用率下降场景下保持优势。

## 摘要

> Recently, many streaming video understanding methods have been proposed by constructing an external memory to store historical data for computational reduction. Most methods focus on optimizing the injection procedure of current data (write) and retrieving informative historical data (read) from memory, while overlooking the opportunity to further enhancing the representational capability of memory itself. In this work, we present StreamEMS, a general mechanism for improving streaming video understanding by re-structuring the historical data stored in memory through self-evolving memory scheme, enabling more informative and robust memory representations. Specifically, we first introduce a Semantic Evolution Module to evolve the memory into more information-dense representations by exploiting informative memory entities discovered via progressively shrinking semantic scales from coarse to fine. In addition, we further introduce a Prior-informed Evolution Module to evolve memory into more robust representations by leveraging prior memory distributions to refine the current memory state. We validate the effectiveness of our proposed designs on widely-used streaming video understanding datasets, i.e., OVO-Bench and StreamingBench, and the results showcase that our method performs better than other methods. Moreover, the advantage of our method becomes consistently evident even under high token usage drop rate settings, indicating the effectiveness and robustness of our method in unleashing the potential of the memory itself.
> Keywords: Online Video Understanding, Multi-Modal Understanding, Streaming Video Understanding, Vision-Language Model

Q1: 这篇论文试图解决什么问题？

本文针对流式视频理解中外接记忆表示能力不足的问题。具体而言：
1. 任务背景：流式视频理解要求模型处理连续、可能无限长度的视频流，对计算效率与记忆容量都有极高要求。为了缓解计算压力，主流做法是显式构建外部记忆（external memory），将历史帧或片段的信息压缩存储，推理时按需读取。
2. 现有方法的共同局限：绝大多数已有工作都把优化重点放在两个环节——一是‘写’（write），即如何把当前时刻的数据更有效地注入记忆；二是‘读’（read），即如何从记忆中检索出对当前推理有用的历史信息。然而，记忆本身被当作一个基本静态的存储容器，其存储内容的表示是否紧凑、是否信息密集、是否会随着新知识的到来而得到重新组织，这些问题几乎被忽略。
3. 核心矛盾：随着视频流不断增长，记忆中的历史数据会逐渐累积冗余、过时或区分度低的表示，这会直接限制后续推理性能。即使读写策略设计得再精巧，也无法弥补记忆表示本身质量下降带来的信息瓶颈。
4. 本文针对性：StreamEMS 选择从记忆内部表示重构入手，提出自进化记忆方案，让记忆在存储过程中基于语义层次和先验分布不断演化，从而生成信息更密集、更鲁棒的表示。
5. 难点分析（合理推断）：设计一种通用且高效的进化机制并不容易——它既要保留原始记忆中的关键语义，又要能去粗取精、压缩冗余；同时不能带来过高的额外计算开销，否则会抵消外部记忆本身的计算节省优势。论文特意在高 token 使用率下降设置下验证，说明作者关注计算效率与表示质量的权衡。

Q2: 有哪些相关研究？

根据检索到的相关章节与摘要，相关研究可以归纳为三个方面：
1. 流式视频理解中的外部记忆方法：流式视频理解要求模型处理连续且可能无限的视频流。大量已有工作（论文引用 [5, 7, 8, 9, 10, 11] 等）显式构建外部记忆来存储历史数据，以解决无限长视频带来的计算与存储挑战。这些工作的主流改进集中在记忆的写入过程和读取过程：比如优化当前数据注入记忆的方式，或提高从记忆中高效读取相关历史信息的能力（后一类在启发式草稿中提及 [12, 13, 14] 等改进 memory-read 操作的方法）。但总体上，它们都把记忆当作固定容器，没有考虑记忆表示本身的自演化。
2. 自进化机制的已有探索：在一些其他领域中，自进化（self-evolving）思想已被初步应用，例如通过指数移动平均（Exponential Moving Average, EMA）等方式来稳定模型的预测或表示，使模型在使用自身预测进行更新时更加鲁棒。然而，论文指出，这种自进化范式此前尚未被引入流式视频理解任务中，本文是首次在这一场景下检验自进化记忆的潜力。
3. 人类记忆研究的启发：论文引用人类认知科学结论（参考文献 [15]）指出，人类记忆系统并不只是被动存储信息，而是会持续对已获取的知识进行内部重组，以形成更丰富的表示；大脑还会通过多条通路在层次化语义尺度上反复解读同一信息，从而发现更显著、更有区分度的特征。这一认知机制直接启发了本文的语义进化模块设计。
总体来看，本文填补了现有工作在‘记忆表示自身演化’上的空白，将自进化思想从其他领域迁移到流式视频理解，并以人类记忆重组机制为设计灵感。

Q3: 论文如何解决这个问题？

StreamEMS 的整体方法是在广泛使用的流式视频理解 pipeline 中引入自进化记忆方案。具体组成如下：
1. 基础架构（依据 Overview 片段）：系统首先使用视觉编码器（vision encoder）和文本编码器（text encoder）分别提取视频帧和文本信息，随后将联合特征送入大语言模型（LLM）进行推理。外部记忆用于存储历史数据，以降低长视频流的计算负担。StreamEMS 在这种通用框架中对记忆模块进行改造，因此可视为一种通用机制（general mechanism）。
2. 语义进化模块（Semantic Evolution Module）：该模块的目标是让记忆中的实体携带更密集的信息。其核心思想是进行‘逐步缩小的语义尺度’的探索，即从粗粒度语义到细粒度语义反复解读记忆中的信息，从而发现更有信息量的记忆实体。在具体实现上，论文采用一组可学习查询（learnable queries）从对应的片段特征（clip features）中聚合信息，并将这些查询作为基本记忆实体送入记忆。这样，记忆中的每个实体都相当于对某个片段的高层语义总结，同时通过多尺度语义发现可以突出更具区分度的信息。
3. 先验信息进化模块（Prior-informed Evolution Module）：该模块负责利用记忆的先验分布来精炼当前记忆状态，使记忆表示更加鲁棒。摘要没有给出具体数学形式，但根据相关片段中‘稳定性’和‘指数移动平均（EMA）’的上下文（该片段来自 Related Work 部分），可以合理推断该模块可能借鉴类似 EMA 的策略，在更新记忆时引入历史分布作为先验，对当前候选状态进行平滑或校正，从而降低单步更新带来的波动。
4. 自进化特性：两个模块共同构成一种 self-evolving memory scheme——记忆会随输入数据不断自我调整，而不是被静态写入。语义进化侧重提升信息密度，先验信息进化侧重提升稳定性，二者互补。
需要注意的是，论文没有在检视到的证据中提供模块的公式、损失函数、训练策略等细节，这些需要查阅完整论文。

Q4: 论文做了哪些实验？

根据摘要与检索片段，论文实验部分的关键信息如下：
1. 数据集：使用两个广泛应用的流式视频理解基准——OVO-Bench 和 StreamingBench。这两个数据集分别覆盖不同类型的在线视频理解问题（具体任务定义未在检索证据中呈现）。
2. 对比方法：论文表示方法‘比其它方法表现更好’，但检索证据中没有列出具体 baseline 名称、数量或性能数值表。
3. 关键评测设置：论文专门考察了‘高 token 使用率下降率’设置下的性能。该设置通常意味着大幅减少每个时间步或每个查询所允许使用的 token 数量，从而模拟更严苛的计算预算。结果显示，在这种设置下，StreamEMS 的性能优势依然‘持续明显’，说明方法在低资源条件下也能保持较好的记忆表示能力。
此外，论文没有在检视到的片段中展示消融实验、效率对比或可视化分析。若需要具体数据（准确率、token 节省比例、运行时间等），必须查阅原论文的实验表格和讨论部分。

Q5: 发现了什么实验现象？

根据摘要中的定性描述，实验观察到的核心现象包括：
1. 总体性能提升：StreamEMS 在 OVO-Bench 和 StreamingBench 上均优于对比方法，表明对记忆表示进行自进化重构确实能改善流式视频理解效果。
2. 高 token 使用率下降下的鲁棒性：在高 token 使用率下降率设置下，方法优势‘持续明显’（consistently evident）。这是一个重要的观察——说明当每个步骤可用 token 大幅减少时，自进化后的记忆仍能保留足够的信息，模型对 token 数量不再敏感。这很可能与语义进化模块产出的紧凑、信息密集的表示有关。
3. 两个模块的潜在分工（合理推断）：语义进化模块负责增强信息密度，先验信息进化模块负责增强状态稳定性。从实验结果和设计逻辑看，二者可能具有互补效应，但缺乏消融实验的直接证据。
注意：以上现象均来自摘要的定性表述，没有具体数值支撑；若需要精确的性能差异、高 token 下降率的具体数值范围，需要核对原文中的实验图表和设置描述。

Q6: 有什么可以进一步探索的点？

基于本文方法与实验设定，可以探索的后续方向包括（以下多为合理推断，原文未明确列出）：
1. 语义进化的更灵活设计：当前采用从粗到细逐步缩小语义尺度的固定流程，未来可以研究自适应确定语义尺度的层级数量、粒度以及跨层聚合策略，甚至引入可学习的尺度选择机制，使进化过程更贴合具体任务。
2. 先验建模的强化：除了简单的指数移动平均式先验，可以用可学习的预测器或生成式模型来建模记忆分布的先验，从而更精细地校正当前记忆状态。
3. 跨任务推广：该自进化记忆机制具有通用性，可迁移到其他记忆增强型任务，如长视频问答、具身智能体的长期经验管理、多轮对话中的上下文组织、时间序列预测等。
4. 效率与轻量化：研究如何降低两个进化模块带来的额外计算开销，比如通过蒸馏或稀疏化设计，使其更适合实时或边缘部署场景。
5. 可解释性分析：分析进化前后记忆实体的语义变化，观察是否对应更重要的对象、动作或事件，从而解释自进化带来的性能提升机制。
6. 更长流与更强鲁棒性：在更长、更复杂的视频流上评测，检验记忆表示是否会退化或出现灾难性遗忘，并设计相应的稳定化策略。

Q7: 总结一下论文的主要内容

StreamEMS 论文针对流式视频理解任务中外部记忆表示能力未被充分利用的问题展开研究。流式视频理解要求模型持续处理可能无限长的视频流，为控制计算成本，主流方法构建外部记忆来缓存历史信息，并围绕‘写入’和‘读取’两个操作进行优化。但已有方法都忽略了一个更本质的环节：记忆本身存储的内容如何被表示和重组。如果记忆内部的信息冗余、语义区分度低，那么无论读写策略多优秀，下游推理都会受限。
本文提出 StreamEMS，一个通用的自进化记忆方案，通过两个模块对记忆内容进行动态重构。第一个是语义进化模块，它借鉴人类大脑在多个语义尺度上反复解读信息的机制，采用逐步缩小语义尺度（从粗到细）的方式，在记忆中发现更有信息量的实体。具体实现上，使用一组可学习查询（learnable queries）从对应片段特征中聚合信息，并将这些查询作为基本记忆实体存入记忆。第二个是先验信息进化模块，它利用先验记忆分布对当前记忆状态进行精炼，使表示更鲁棒。整体上，记忆不再是静态的存储，而是随着输入数据的到来不断自我进化。
实验部分在 OVO-Bench 和 StreamingBench 两个基准上验证了方法，结果表明 StreamEMS 优于其他方法。尤其是在高 token 使用率下降设置下，方法的性能优势依然明显，说明自进化后的记忆具有较高的信息密度，能在计算资源受限时依然维持较强的理解能力。
论文的主要贡献可以总结为：一，首次将自进化范式引入流式视频理解的外部记忆机制；二，设计了语义进化和先验信息进化两个模块，分别提升记忆的信息密集度和鲁棒性；三，通过实验证明了该机制的有效性和鲁棒性。由于检索到的证据有限，论文中关于模块的具体实现细节、实验数值和消融分析等内容需要进一步阅读原文核实。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该方法的自进化记忆思想对智能体（agent）的长期记忆管理有直接启发，可用于组织智能体积累的历史经验，使其随交互不断更新和精炼（合理推断）。

## 基本信息

- 作者：Yuxin Liu, Peiqin Zhuang, Yali Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.27881`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本报告基于提供的摘要、PDF语义检索证据及启发式草稿生成，未获取完整论文正文；方法细节与实验数值存在多处推断，已相应标注，具体内容需核对原文。
