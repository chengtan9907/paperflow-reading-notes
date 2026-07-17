---
user_id: "cheng tan"
paper_id: 4307
arxiv_id: "2607.15161v1"
title: "On-Policy Delta Distillation"
publish_date: "2026-07-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.15161v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.15161v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-18T00:55:06"
---
# On-Policy Delta Distillation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：on-policy distillation · delta signal · reasoning transfer · large language models

## 一句话总结

本文提出On-Policy Delta Distillation (OPD^2)，通过定义教师模型与推理调优前基础模型之间的logits差异为delta信号作为蒸馏奖励，明显提升了on-policy蒸馏在数学、科学和代码推理任务上的效果。

## 摘要

> On-policy distillation is an alternative post-training method in reinforcement learning that alleviates the constraints imposed by reward models by providing token-level supervision from a teacher model. Although on-policy distillation has been studied and applied across various settings, its fundamental design remains underexplored. In this paper, we introduce a new distillation reward, termed the delta signal, instead of directly imitating the teacher's output distribution. The delta signal is defined as the difference between the teacher model and its base model prior to instruction tuning for reasoning capability. It therefore captures the changes induced by reasoning tuning and provides a more direct signal for transferring reasoning capabilities. Using extensive empirical evidence, we show that the delta signal substantially improves on-policy distillation and refer to the new distillation method as On-Policy Delta Distillation (OPD$^2$). Experiments across mathematics, science, and code-reasoning benchmarks demonstrate that OPD$^2$ consistently outperforms conventional on-policy distillation, enabling reasoning LLMs to achieve strong performance with only a short post-training period. Code will be available at https://github.com/naver-ai/opd2

Q1: 这篇论文试图解决什么问题？

论文试图解决现有on-policy蒸馏方法在传递推理能力时效率低下的问题。标准的on-policy蒸馏直接使用教师模型的输出分布作为软标签，但这一分布混合了大量预训练知识和通用的语言规律，使得推理调优带来的具体改变（例如对推理链的偏好）被湮没。特别是当教师模型经过专门的推理调优后，其输出相对于调优前的差异中包含最核心的推理改进信号，而直接模仿教师输出会忽视这一差异。此外，直接应用标准OPD在不同模型家族之间可能不稳定，导致性能退化。因此，本文的目标是设计一种蒸馏信号，能够明确地聚焦于推理调优引入的变化，从而更高效地传递推理能力，同时提高跨模型的稳定性。

Q2: 有哪些相关研究？

相关研究主要围绕LLM后训练中的知识蒸馏和on-policy蒸馏方法。在传统知识蒸馏中，常用教师软标签指导学生，但大多用于分类任务。对于自回归语言模型，on-policy蒸馏利用教师生成输出时提供的token级监督，替代了基于奖励模型的强化学习方法。现有的on-policy蒸馏工作包括标准OPD（直接匹配教师分布）和ExOPD（扩展OPD，通过改进采样或损失函数来增强稳定性）。这些方法虽然有效，但都未能显式分离推理调优带来的特殊信号。此外，还有一些工作探索利用基础模型和微调模型之间的差异（如LIAR、对比解码等），但主要用在推理阶段而非训练阶段。本文的delta信号类似概念但用于蒸馏训练，相关工作的具体比较在论文Related Work部分有详细说明。

Q3: 论文如何解决这个问题？

论文提出OPD^2，其核心是delta信号的设计。对于输入序列中的每个token，令教师模型（推理调优后）输出logits为t，同一教师的基础模型（推理调优前）输出logits为b，则delta信号定义为t与b的某种差异（如softmax后分布相减或计算KL散度）。在蒸馏训练中，学生模型不再直接拟合教师的原始分布，而是拟合delta信号所代表的修正分布。具体地，损失函数鼓励学生输出分布与教师的delta信号分布一致。训练过程中需要额外执行基础模型的前向传播以获取b，但基础模型可以是教师的一个副本或知识蒸馏中使用的较早期检查点。除了推理模式（thinking）外，OPD^2也适用于非推理模式（non-thinking）。论文在实现上采用了在线蒸馏（on-policy）的设置，即教师生成训练样本并同时提供监督信号。

Q4: 论文做了哪些实验？

实验在三个推理领域进行：数学（包含Aimo-2等竞赛数据集）、科学和代码。基准包括标准OPD、ExOPD以及原始教师模型本身的性能。使用的模型包括Qwen3系列和Gemma4-E4B，以覆盖不同规模和架构。实验分为三部分：（1）主性能对比：在多个基准上比较OPD^2与基线，并报告各自的分数；（2）消融研究：通过替换delta信号为标准OPD信号、去除基础模型组件等，分析各部分的贡献；（3）计算成本分析：记录训练每步的墙钟时间，对比OPD^2与标准OPD的开销。核心实验使用了大量标准评估指标，如pass@k、accuracy等。此外还分析了跨模型家族迁移的稳定性，即用不同家族（如Qwen3与Gemma4）的教师蒸馏学生。

Q5: 发现了什么实验现象？

实验揭示了多个重要发现：（1）OPD^2在所有三个推理领域和所有模型配置上一致超越标准OPD和ExOPD，且多数情况下达到统计显著。（2）消融实验中，将delta信号替换为标准OPD信号导致最大的性能下降（在非思考模式下drop超过一定幅度，思考模式下同样明显），证实delta信号是性能提升的关键。（3）直接应用标准OPD在跨模型家族蒸馏时可能出现不稳定，性能甚至低于原始学生；ExOPD改善了稳定性但仍有差距，OPD^2则完全避免了退化并带来增益。（4）计算成本方面，OPD^2需要额外一次基础模型前向，对于Qwen3模型训练时间增加约24-28%，对于Gemma4-E4B增加约8%，但考虑到性能提升，这种开销被认为可接受。（5）在非思考模式（即不产生思维链）下OPD^2同样有效，说明delta信号捕捉的不仅是长推理链信息，还包括更细微的推理偏好。

Q6: 有什么可以进一步探索的点？

尽管OPD^2展示了强大性能，但仍有可探索的方向：（1）减少计算开销：当前需要完整的教师基础模型前向，未来可以利用知识蒸馏中的轻量级baseline模型或共享层近似，或者只在部分token上计算delta。（2）扩展到多模态推理：将delta信号定义扩展到视觉、多模态场景，以增强多模态LLM的推理能力。（3）理论分析：进一步研究delta信号为何能够更有效地传递推理能力，以及它与互信息、表示学习的关系。（4）与其他后训练方法结合：例如将delta信号与直接偏好优化（DPO）或强化学习结合，可能进一步提升效果。（5）探索在更大规模模型和更多任务上的应用，包括一般问题回答和创造性任务。（论文原文可能包含更多方向，这里基于合理推断补充。）

Q7: 总结一下论文的主要内容

这篇论文提出了一种新的知识蒸馏方法OPD^2，专门用于提升大语言模型的推理能力。其核心贡献是引入delta信号，即教师模型与其推理调优前的基础模型之间的输出差异，作为蒸馏的监督信号。相比于传统on-policy蒸馏直接模仿教师输出，delta信号更清晰地指示了推理调优带来的特征变化，使得学生模型能够更高效地学习教师的推理策略。论文从问题出发，分析了现有蒸馏方法的不足，然后形式化定义了delta信号，并基于此设计了训练流程。实验部分设计非常系统：在数学、科学、代码三个典型推理领域进行验证，涵盖了多种模型（Qwen3、Gemma4）和多种对比方法（标准OPD、ExOPD）。主要结果显示OPD^2在所有设定下均显著优于基线，且稳定性更强。消融实验进一步确认了delta信号是关键因素。此外，论文还务实地讨论了计算成本的增加（训练时间增加8-28%），并认为这一代价换来的性能提升是值得的。总体而言，OPD^2为LLM推理能力的后训练提供了一种简单、有效且通用的新途径，对于希望高效蒸馏大型推理模型的研究者和工程师具有直接参考价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文聚焦推理能力蒸馏，对于AI for Science场景（特别是科学推理和代码推理）有直接应用潜力。

## 基本信息

- 作者：Byeongho Heo, Jaehui Hwang, Sangdoo Yun, Dongyoon Han
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.CL
- 日期：2026-07-16
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.15161v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的证据片段，并基于这些片段进行了归纳和合理推断。
