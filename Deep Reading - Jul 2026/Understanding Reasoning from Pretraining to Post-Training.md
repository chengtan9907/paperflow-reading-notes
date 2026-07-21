---
user_id: "cheng tan"
paper_id: 4681
arxiv_id: "2607.16097"
title: "Understanding Reasoning from Pretraining to Post-Training"
publish_date: "2026-07-20"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.16097.pdf"
pdf_url: "https://arxiv.org/pdf/2607.16097"
abs_url: "https://arxiv.org/abs/2607.16097"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-21T01:46:32"
---
# Understanding Reasoning from Pretraining to Post-Training

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · pretraining · post-training · reasoning

## 一句话总结

本文以国际象棋为受控实验平台，系统研究了从预训练到强化学习后训练全流程中推理能力的发展，揭示了预训练损失对后RL性能的预测关系以及RL对策略的不同影响机制，并验证了向数学领域的迁移性。

## 摘要

> Reinforcement learning (RL) has become central to improving large language models (LLMs) on complex reasoning tasks, yet RL post-training is largely studied in isolation from the pretraining that precedes it. As a result, two basic questions remain open: (1) how do pretraining choices (model size, data) shape the returns to RL compute, and (2) what does RL actually do to the model? These questions are difficult to study in the standard LLM setting: pretraining corpora are vast and uncontrolled, making it hard to attribute behaviors to pretraining versus RL, and systematic compute sweeps across both stages are prohibitively expensive. To address these challenges, we use chess as a controlled testbed for studying reasoning across the full pretraining-to-post-training pipeline. We follow the standard LLM training pipeline by pretraining language models from 5M to 1B parameters on human chess games, supervised fine-tuning on synthetic reasoning traces, and running RL on chess puzzles with verifiable rewards. Using this framework, we find that the post-RL performance at given RL compute level is well-predicted from the pretraining loss, and slope of the RL reward curves improves approximately linearly with the pretraining tokens. Beyond scaling, we find that RL does not simply sharpen the SFT policy: on easy puzzles it amplifies correct moves the SFT policy already preferred, while on hard puzzles it surfaces correct moves that were nearly absent under SFT. We further test whether our findings transfer beyond chess by training a 1B language model on math-domain text, where the same predictive pattern emerges: longer-pretrained checkpoints reach higher post-RL performance and improve faster under RL. In sum, we provide a quantitative account of the pretraining-to-RL interface and a controlled testbed for studying the science of reasoning across the full pretraining-to-post-training pipeline.

Q1: 这篇论文试图解决什么问题？

论文旨在解决两个核心问题：（1）预训练选择（模型大小、数据量）如何影响后续强化学习计算投入的回报？（2）强化学习后训练实际上如何改变模型策略？由于标准LLM设置中预训练语料庞大且不可控，难以归因，因此论文采用国际象棋作为受控实验环境。

Q2: 有哪些相关研究？

相关工作包括：强化学习在推理中的应用（如Guo et al., 2025; Lambert et al., 2024），从预训练到后训练的缩放定律（如Kaplan et al., 2020; Hoffmann et al., 2022），以及RL策略变化机制的分析（如Yue et al., 2025认为RL主要锐化推理，而其他观点认为RL浮现新能力）。论文在此基础上提出了统一的定量框架。

Q3: 论文如何解决这个问题？

论文构建了一个受控的国际象棋学习环境，模拟标准LLM训练流水线：预训练阶段在人类棋局上训练5M至1B参数的语言模型；监督微调阶段在合成推理轨迹上训练；强化学习阶段在谜题上使用可验证奖励进行训练。通过系统扫描预训练数据量、模型大小和RL计算量，分析它们对性能的影响。

Q4: 论文做了哪些实验？

论文进行了一系列实验：1）扫描不同预训练数据量和模型大小下的RL性能曲线；2）建立预训练损失与后RL性能的预测关系；3）分析RL奖励曲线的斜率随预训练数据量的变化；4）研究RL在不同难度谜题上对策略概率分布的具体影响；5）将发现迁移至数学领域，在1B语言模型上验证相同模式。

Q5: 发现了什么实验现象？

实验现象包括：1）后RL性能（ELO评分）与预训练损失呈强相关，且该关系可被幂律拟合；2）RL奖励函数收敛时性能随预训练数据量增加而线性提升；3）在简单谜题上，RL主要放大SFT策略已偏好的正确走法，而在困难谜题上，RL能够浮现SFT几乎不存在的正确走法；4）RL同时会强化部分错误走法，导致困难谜题上性能提升伴随着偏差放大；5）在数学领域的迁移验证显示，更长时间预训练的检查点在RL后达到更高性能且提升更快。

Q6: 有什么可以进一步探索的点？

进一步探索方向包括：1）在更多复杂推理任务（如数学证明、代码推理）中验证结果；2）研究不同RL算法（如PPO, GRPO）对预训练-后训练交互的影响；3）探索预训练数据混合（如增加推理相关数据）对RL回报的定量影响；4）将分析扩展到多轮交互和长期规划场景；5）理论分析预训练损失与RL性能之间的因果关系。

Q7: 总结一下论文的主要内容

本文以国际象棋为受控平台，系统地研究了从预训练到强化学习后训练全流程中推理能力的发展。论文首先构建了一个标准的语言模型训练流水线：在人类棋局上预训练多个不同大小（5M至1B参数）和不同训练步数的模型，然后通过监督微调（SFT）在合成推理轨迹上初始化，最后在棋局谜题上使用强化学习（RL）进行后训练。通过这一设定，论文能够精确控制预训练和后训练的计算量，从而定量回答两个基本问题。主要发现包括：（1）后RL性能（以ELO评分为衡量）可以由预训练损失（即验证集交叉熵）准确预测，两者呈现幂律关系；（2）RL奖励曲线的斜率（即RL效率）随预训练数据量的对数线性增加；（3）RL在不同难度谜题上对策略的影响不同：在简单谜题上，RL主要放大SFT已偏好的正确走法，在困难谜题上，RL能够挖掘SFT几乎未出现的正确走法，但同时也会强化错误走法，导致困难谜题上性能提升受限；（4）论文还验证了这些发现向数学领域的迁移性，在1B参数模型上使用数学领域文本预训练，得到了一致的预测模式。论文最终提供了一个关于预训练与RL交互的科学定量理解，并为未来研究提供了受控实验框架。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文提供了预训练与RL交互的定量理解，对AI for Science中优化训练流程具有参考价值

## 基本信息

- 作者：Jingyan Shen, Ang Li, Salman Rahman, Yifan Sun, Micah Goldblum, Matus Telgarsky, Pavel Izmailov
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL
- 日期：2026-07-20
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.16097`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的证据片段，并优先采用了这些片段。
