---
user_id: "cheng tan"
paper_id: 1158
arxiv_id: "2606.23595"
title: "SPIRAL: Learning to Search and Aggregate"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23595.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23595"
abs_url: "https://arxiv.org/abs/2606.23595"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T00:36:36"
---
# SPIRAL: Learning to Search and Aggregate

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · reasoning · inference compute scaling · language model

## 一句话总结

SPIRAL通过结合集合强化学习和标准强化学习，联合训练语言模型在推理时有效使用顺序、并行和聚合三种计算原语，从而在数学推理任务上以更低的推理计算成本获得更高性能（相比GRPO提升高达11倍效率，15%绝对性能）。

## 摘要

> Language model reasoning can be substantially improved at test time via scaffolds that scale inference compute across different primitives -- sequential reasoning within a trace, independently sampled parallel traces, and aggregation of multiple reasoning traces into a final response. During post-training, however, language models are optimized only for sequential reasoning within a single trace. We introduce Sequential-Parallel-Aggregative Reinforcement Learning (SPIRAL), a framework in which a language model is trained to use all three primitives, as part of a unified inference compute pipeline. Concretely, the language model first samples a set of independent traces in parallel, each produced through sequential chain-of-thought reasoning, and then generates a final aggregation trace conditioned on those traces; all components are optimized end-to-end against the reward of the final aggregated response. To train this system, SPIRAL uses set reinforcement learning to teach models to produce a set of traces that are collectively useful for an aggregator and standard reinforcement learning to teach models to aggregate the set into improved final responses. Our experiments on reasoning tasks show that SPIRAL effectively scales with inference compute, outperforming GRPO by up to 11$\times$ scaling efficiency and 15% higher performance when all three compute primitives are scaled.

Q1: 这篇论文试图解决什么问题？

该论文针对的核心问题是：语言模型在推理时可以通过扩展计算资源（如多次采样、链式思考、聚合答案）来提升性能，但现有的后训练（post-training）只针对单轨迹顺序推理进行优化，模型没有学会如何有效利用并行采样和聚合（combination of multiple reasoning traces）来改进最终答案。具体来说，现有方法如GRPO虽然可以在测试时通过采样多个轨迹并聚合（例如多数投票），但模型本身并没有被训练成生成“对聚合有用的轨迹”或者“根据多个候选轨迹正确聚合”。这就导致模型在测试时扩展计算资源时效率低下。SPIRAL试图填补这一空白，让模型通过强化学习学会在推理过程中有机地结合顺序推理、并行探索和融合决策。

Q2: 有哪些相关研究？

相关研究主要分为三类：（1）推理时计算扩展（test-time compute scaling），如思维链（Wei et al., 2022）、self-consistency（Wang et al., 2023）等，这些方法在测试阶段通过多次采样和聚合提升性能，但模型本身并非为此训练；（2）强化学习推理（RL for reasoning），如GRPO（Shao et al., 2024）、R1（DeepSeek R1）等，通过RL优化单轨迹推理，但不处理多轨迹聚合；（3）分解与并行推理，如某些工作训练模型将任务分解为可并行子任务（YAL+25, PLL+25），但这些通常使用监督微调而非强化学习。SPIRAL与这些工作互补：不要求模型显式分解任务，而是通过RL端到端学习如何并行探索和聚合，且不与现有RL方法（如GRPO）冲突，可以叠加使用。

Q3: 论文如何解决这个问题？

SPIRAL的核心方法分三步：第一，定义三个推理计算原语：（1）顺序计算（sequential compute），即模型生成一个链式思考轨迹；（2）并行计算（parallel compute），模型同时生成多个独立轨迹（set of traces）；（3）聚合计算（aggregative compute），模型基于前面生成的轨迹集合生成一个最终聚合轨迹。第二，训练时，模型暴露于这三个原语构成的计算流程：先并行生成一组轨迹（每个轨迹通过顺序推理得到），然后将这些轨迹作为上下文输入给同一个模型，模型生成聚合轨迹。第三，优化目标：利用集合强化学习（set RL）来训练模型生成“对聚合有用的轨迹集”，奖励是最终聚合响应是否正确的函数，同时鼓励轨迹之间的多样性；利用标准强化学习（standard RL）来训练模型如何根据轨迹集进行聚合（即生成聚合轨迹）。整个流程端到端优化，模型共享参数。训练中通过强化学习信号，模型学会既要产生多样化且有潜力的候选轨迹（不要求单个轨迹正确），也要学会从候选轨迹中综合出正确答案。

Q4: 论文做了哪些实验？

论文在数学推理任务上进行实验，使用Qwen3-4b-Instruct-2507作为基础模型，对比基线包括GRPO（Shao et al. 2024）以及不训练直接使用多数投票等方法。实验设置中，SPIRAL模型经过强化学习训练，能够在测试时通过调整并行轨迹数量和聚合计算次数来扩展计算资源。评估指标包括准确率和计算效率（每个问题使用的token数）。实验还分析了不同规模的计算预算下的性能缩放曲线。

Q5: 发现了什么实验现象？

主要发现：（1）SPIRAL在相同推理计算预算下，性能显著高于GRPO，绝对准确率提升约15%；（2）SPIRAL的计算缩放效率更高，在扩展并行轨迹数量时，SPIRAL的性能提升曲线更陡，而GRPO在轨迹数较多时出现饱和；（3）SPIRAL的聚合模块不仅提升了准确率，还减少了冗余计算，因为模型学会了过滤低质量轨迹，减少需要生成的聚合轨迹数量；（4）消融实验表明，集合RL和标准RL组件缺一不可；移除集合RL后模型生成的轨迹多样性下降，聚合性能也下降；移除标准RL后聚合模块无法有效利用轨迹集；（5）在相同计算量下，SPIRAL相比“推理时采样+多数投票”方法有显著优势，说明训练使模型更善于利用并行资源。

Q6: 有什么可以进一步探索的点？

论文提到几个未来方向：（1）将SPIRAL与其他推理方法（如自一致性、树搜索）组合，看是否可以进一步提升；（2）将SPIRAL扩展到更通用的任务（如编程、科学推理）；（3）将SPIRAL与监督微调或人类反馈结合；（4）进一步分析聚合过程中模型内部的变化，如模型如何识别正确轨迹。此外，可以探索将SPIRAL用于多模态推理、具身推理等需要多次尝试的场景。

Q7: 总结一下论文的主要内容

论文介绍SPIRAL（Sequential-Parallel-Aggregative Reinforcement Learning），一种通过强化学习统一训练语言模型在推理时使用顺序、并行和聚合三种计算原语的框架。现有后训练方法（如GRPO）只优化单轨迹顺序推理，无法高效利用推理时的并行计算和聚合。SPIRAL首先定义三个原语，并设计一个统一的计算流程：模型先并行生成一组独立轨迹（每个轨迹是链式思考），然后基于这些轨迹生成一个聚合轨迹。训练目标分为两部分：集合强化学习训练模型生成对聚合有用的轨迹集（鼓励多样性和集体正确性），标准强化学习训练模型如何从轨迹集中聚合出正确答案。两者端到端优化，共享参数。在数学推理任务上，SPIRAL在相同计算量下性能远超GRPO（提升15%，效率提升11倍）。实验表明SPIRAL有效训练模型学会了更好的并行探索和聚合策略，且计算缩放效果显著。主要贡献：提出并验证了端到端训练模型使用多种推理计算原语的框架；展示集合RL在推理训练中的有效性；提供了一种可叠加现有RL方法的扩展方案。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：对于构建能够高效利用推理时计算的智能体系统，SPIRAL提供了一种端到端训练方法，使模型自主学习搜索和聚合策略。

## 基本信息

- 作者：Jubayer Ibn Hamid, Ifdita Hasan Orney, Michael Y. Li, Omar Shaikh, Yoonho Lee, Dorsa Sadigh, Chelsea Finn, Noah Goodman
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-06-23
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23595`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本文生成使用了PDF检索证据，所有信息均来自检索到的片段和摘要，关键结果和数值来自实验结果字段。
