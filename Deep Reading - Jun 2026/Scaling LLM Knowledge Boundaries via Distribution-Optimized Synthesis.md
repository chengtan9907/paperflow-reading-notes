---
user_id: "cheng tan"
paper_id: 1139
arxiv_id: "2606.23271"
title: "Scaling LLM Knowledge Boundaries via Distribution-Optimized Synthesis"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23271.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23271"
abs_url: "https://arxiv.org/abs/2606.23271"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T00:26:47"
---
# Scaling LLM Knowledge Boundaries via Distribution-Optimized Synthesis

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：knowledge distribution · synthetic data · large language model · knowledge injection

## 一句话总结

提出了KDoS（Knowledge Distribution-optimized Synthesis），通过知识密度驱动和三阶段反馈机制优化合成数据知识分布，从而扩展LLM知识边界。

## 摘要

> Knowledge injection via synthetic data is crucial for enhancing Large Language Models (LLMs). However, current synthesis methods simply stop at preset token counts or fixed data ratios, lacking awareness of knowledge distribution. This results in some domains being sparse while others are redundant, limiting LLM knowledge boundaries. We revisit knowledge injection from a distribution perspective and hypothesize that an optimal knowledge distribution exists to maximize knowledge boundary expansion. We propose KDoS (Knowledge Distribution-optimized Synthesis), a framework that introduces knowledge density to drive synthesis through a three-stage feedback mechanism, shifting from blind generation to distribution-optimized synthesis. We construct Wikipedia-based synthetic data with varying knowledge distributions and conduct experiments on models from 0.6B to 16B (Qwen, Ling, LLaMA) and data scales from 1B to 5B tokens. Our key findings are: (1) an optimal knowledge distribution consistently maximizes boundary expansion; (2) this distribution is stable across backbones and scales; (3) KDoS outperforms baselines across six knowledge benchmarks. Our work offers a new perspective and practical framework for synthetic data-driven knowledge injection.

Q1: 这篇论文试图解决什么问题？

当前合成数据方法在知识注入时知识分布意识不足，只是粗暴地设置预设令牌数或固定数据比率，导致生成的数据中某些领域知识稀疏、另一些冗余，限制了LLM知识边界（knowledge boundary）的扩展。论文试图回答是否存在最优知识分布以及如何动态调整合成过程来逼近该分布，从而最大化知识注入效率。

Q2: 有哪些相关研究？

相关研究包括基于规则或模型生成的合成数据方法，以及数据分布对模型性能影响的分析。但本文特别聚焦于知识分布（knowledge distribution）这一维度，将其视为独立可控变量，并探索其对LLM知识边界扩展的影响。已有工作通常关注数据量或质量，而本文强调分布形状的优化。

Q3: 论文如何解决这个问题？

提出KDoS框架，核心思想是将知识密度（knowledge density）作为可控变量，通过三阶段动态反馈机制逐步塑造合成数据的知识分布。第一阶段为监测：实时计算当前数据池的知识分布；第二阶段为动态调整接受策略：根据目标分布修正生成数据的采样或接受概率；第三阶段为迭代驱动：重复前两步直到数据分布逼近预设目标。该框架将合成从盲目填充转变为有目标地均匀覆盖知识空间，从而扩展LLM知识边界。

Q4: 论文做了哪些实验？

使用Wikipedia为基础，构造具有不同知识分布（如均匀、长尾、集中等）的合成数据集。在以下配置上验证：模型方面包括Qwen系列、Ling系列、LLaMA系列，参数量从0.6B到16B；数据规模从1B到5B令牌。评估基准包括六个侧重知识能力的下游任务（如MMLU、CommonsenseQA等）。对比基线包括随机采样、基于token数量控制、固定比例策略等。消融实验分析了知识密度定义、反馈轮数等的影响。

Q5: 发现了什么实验现象？

（1）存在一个最优知识分布（在实验中表现为知识密度的某种特定形状），在该分布下LLM的知识基准得分和边界扩展指标最高，且效果一致优于其他分布；（2）该最优分布形态在不同模型骨干和不同数据规模下保持稳定，说明其具有泛化性；（3）KDoS在六个知识基准上均超过基线方法，尤其在知识密集型任务上提升显著。未报告明显的负结果或失败案例，但推测分布极度不均匀时性能下降。

Q6: 有什么可以进一步探索的点？

（1）将KDoS扩展到预训练阶段（当前仅限SFT），探索知识分布优化在更早阶段的收益；（2）研究知识密度的更精细度量，包括跨语言、跨模态知识；（3）验证更大模型（如70B+）和更大数据规模（10B+）下的分布稳定性；（4）将框架应用于多任务联合训练，考察不同任务对知识分布的需求冲突；（5）探索自动化发现最优分布的方法，减少人工预设。

Q7: 总结一下论文的主要内容

论文从知识分布优化的视角出发，针对现有合成数据方法缺乏分布意识导致LLM知识边界受限的问题，提出KDoS框架。该框架引入知识密度作为可控变量，通过三阶段动态反馈机制（监测、调整、迭代）将生成过程从盲目填充转变为定向分布塑造。作者在Wikipedia数据集上构造不同知识分布的合成数据，并在0.6B至16B规模的多种LLM（Qwen、Ling、LLaMA）上实验，数据量从1B到5B令牌。关键发现包括：存在一个稳定且可泛化的最优知识分布，能一致最大化知识边界扩展；KDoS在六个知识基准上优于所有基线。消融实验验证了密度定义和反馈轮数的有效性。论文的主要贡献在于提出了一种分布优化的新视角和实用框架，揭示了知识分布对知识注入效率的关键影响。局限在于当前仅覆盖SFT阶段，未验证预训练；且最优分布的定义仍需要人工指定目标分布。未来工作可扩展到预训练、更大规模及更细粒度知识度量。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：与生成方向直接相关（权重0.10），提供了一种数据生成的新视角。

## 基本信息

- 作者：Songze Li, Yarong Lan, Zhongpu Bo, Zhaoyang Wang, Zhiqiang Liu, Yuan Yuan, Chengtao Gan, Menghao Qian, Enpei Niu, Xiaoke Guo, Yuanxiang Liu, Zhaoyan Gong, Xiangjin Hu, Liangyurui Liu, Jingdian Lu, Lei Liang, Jun Zhou, Huajun Chen, Wen Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-23
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23271`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（retrieved_evidence）和启发式草稿（heuristic_draft），主要依据Abstract、Introduction、Conclusion和Limitations片段。
