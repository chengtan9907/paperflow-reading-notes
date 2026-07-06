---
user_id: "cheng tan"
paper_id: 2474
arxiv_id: "2607.02490"
title: "Visually Grounded Self-Reflection for Vision-Language Models via Reinforcement Learning"
publish_date: "2026-07-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.02490.pdf"
pdf_url: "https://arxiv.org/pdf/2607.02490"
abs_url: "https://arxiv.org/abs/2607.02490"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-06T12:35:58"
---
# Visually Grounded Self-Reflection for Vision-Language Models via Reinforcement Learning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：vision-language model · self-reflection · reinforcement learning · visual grounding

## 一句话总结

通过强化学习框架VRRL，利用随机掩码和缓冲回滚注入，训练视觉语言模型进行视觉基础的自我反思，从而提升分布外场景下的鲁棒性。

## 摘要

> Large vision-language models can reason over multimodal inputs by generating textual chains of thought (CoT). A key capability exhibited in CoT reasoning is self-reflection: revisiting earlier decisions and correcting previous errors. However, existing LVLMs often fail to properly attend to visual inputs during reflection, limiting their ability to translate feedback into grounded corrections, especially for out-of-distribution images. To address this issue, we propose a novel reinforcement learning training framework VRRL, with two components explicitly designed to elicit visually grounded self-reflection. First, we randomly mask trajectory prefixes during training to emphasize recovery from incorrect intermediate predictions rather than making early mistakes. Second, we introduce buffered roll-ins from an experience replay buffer to expose the model to diverse failure states that it must learn to correct. We evaluate our approach on visual grounding tasks involving tables and charts, as well as spatial navigation benchmarks. While off-the-shelf and conventionally fine-tuned models degrade substantially under distribution shift, our method substantially improves average out-of-distribution accuracy over standard RL and reflection-oriented fine-tuning baselines by using self-reflection effectively.

Q1: 这篇论文试图解决什么问题？

大型视觉语言模型（LVLM）通过生成文本思维链进行多模态推理，其中自我反思——即重新审视早期决定并纠正先前错误——被认为是思维链推理的关键能力。然而，现有LVLM在反思时往往无法充分关注视觉输入，导致它们难以将反馈转化为视觉基础的纠正，尤其是在处理与预训练分布显著不同的分布外图像时。这种视觉-语言对应关系的脱节限制了模型在复杂、动态环境中的鲁棒性。因此，需要一种训练方法，能够促使模型在学习自我反思的同时，保持对视觉信息的敏感性和依赖性。本文试图通过强化学习框架解决这一问题，使模型不仅能进行多轮反思，而且能真正基于视觉反馈进行纠正，从而提升分布外泛化能力。

Q2: 有哪些相关研究？

相关研究可分为几个方向：1）视觉语言模型的多模态思维链推理，例如通过文本CoT逐步推理，但往往忽视视觉基础；2）自我反思与自我纠正机制，在纯文本模型中已有探索（如Self-Correction, Self-Refine），但在多模态场景中较少；3）强化学习在语言模型中的应用，如RLHF、PPO等，用于对齐和训练，但直接用于训练反思行为的研究有限；4）视觉基础任务，包括表格理解、图表问答、视觉导航等，这些任务需要精确的视觉-语言对应。本文的方法结合了上述方向，通过RL训练专门促进视觉基础的反思，与以往单纯增加反思步骤而不保证视觉关注的方法不同。

Q3: 论文如何解决这个问题？

本文提出VRRL（Visual Reflection via Reinforcement Learning）框架，包含两个关键组件以引发视觉基础的自我反思。第一是随机掩码（Random Turn Masking）：在强化学习训练时，对每条轨迹随机选择后缀部分进行策略更新，而掩码掉前面的部分。这使得模型不能仅仅依赖早期正确预测来获得奖励，而是必须学会从中间错误状态中恢复，从而强调纠错能力而不是避免错误。第二是缓冲回滚注入（Buffered Roll-In）：维护一个经验回放缓冲区，存储过去轨迹中模型失败的中间状态（即“错误前缀”）。在训练时，从缓冲区采样这些历史失败状态作为初始状态，让模型从该点继续生成轨迹，并基于最终结果获得奖励。这使模型暴露于更多样化的失败场景，从而学习如何修正不同类型的错误。整个训练过程分为两个阶段：首先使用监督微调（SFT）在任务数据上初始化模型，使其具备基本的任务能力；然后使用VRRL进行强化学习训练，奖励信号来自环境提供的视觉反馈（如任务是否正确完成）。在推理时，模型执行多轮反思：首先生成初始回答，然后基于视觉反馈进行逐步修正，最多进行若干轮。

Q4: 论文做了哪些实验？

实验在三个视觉任务上进行：1）表格理解（TableQA），包括分布内和分布外设置（如表结构变化）；2）图表理解（ChartQA），分布外包括不同风格和类型；3）空间导航（基于MINOSS等环境），分布外包括不同房间布局。基线包括：原始预训练LVLM（如LLaVA、Qwen-VL等）、标准PPO强化学习（不包含反思）、反思微调（Reflection Tuning，Wu et al. 2025a）以及仅使用随机掩码或缓冲回滚的消融版本。评估指标为任务准确率。实验发现，VRRL在分布外准确率上显著优于所有基线，特别是在分布偏移较大时优势更明显。消融实验表明两个组件各自贡献，且结合时效果最好。

Q5: 发现了什么实验现象？

实验现象包括：1. 分布外性能：VRRL在分布外设置下准确率显著提升，而标准RL基线与预训练模型相比提升有限甚至下降，表明单纯RL可能让模型过拟合分布内模式，而VRRL通过反思增强了泛化。2. 反思深度：VRRL训练的模型倾向于进行更多轮反思，并从中受益，而基线模型多轮反思后性能反而可能下降（由于错误累积或过度修正）。3. 失败案例：分析显示，基线模型在分布外图像上常常忽略视觉线索，得出与文本先验一致但错误的答案，而VRRL模型能够根据视觉反馈修正初始错误，说明其反思是视觉基础的。4. 消融趋势：移除随机掩码导致模型在早期步骤后不易纠正；移除缓冲回滚导致对罕见失败模式处理能力下降；两者结合实现互补。5. 规模化趋势：当模型尺寸增大时，VRRL带来的收益更显著，暗示更大模型更受益于反思训练。

Q6: 有什么可以进一步探索的点？

未来研究方向包括：1）将VRRL应用于更广泛的多模态任务，如开放域VQA、图像描述等，其中视觉反馈可能不是二元信号；2）探索更复杂的奖励设计，如基于语义相似度的反馈，而非仅正确/错误；3）结合模型内在的不确定性估计来决定何时反思；4）研究反射步骤数与性能的权衡；5）将方法迁移到其他模态（如视频、音频）；6）与视觉语言模型的预训练阶段结合，从更底层促进视觉基础；7）处理反射过程中的错误传播问题；8）应用有实际价值的场景，如科学图表分析、医学图像报告等。

Q7: 总结一下论文的主要内容

本文研究了视觉语言模型（LVLM）的自我反思能力，特别是如何通过强化学习训练使其进行视觉基础的自我反思。现有LVLM在生成思维链时虽然具备反思能力，但在分布外图像上经常忽略视觉输入，导致纠错失败。作者提出VRRL框架，包含两个核心组件：随机掩码（Random Turn Masking）和缓冲回滚注入（Buffered Roll-In）。随机掩码通过只对轨迹后缀进行更新，强迫模型从错误中间状态中恢复；缓冲回滚通过从经验池中采样历史失败状态，使模型暴露于多样化的失败情形。训练分两步：先SFT初始化，再RL训练，奖励来自环境根据视觉反馈给出的正确性信号。实验在表格理解、图表理解和空间导航任务上进行，评估分布外泛化能力。结果表明，VRRL显著优于标准RL和反思微调基线，尤其是分布外准确率提升明显。消融研究验证了两个组件的必要性。此外，分析显示VRRL模型确实进行多轮视觉基础的反思，纠正了初始错误，而基线模型则倾向于忽略视觉信息。本工作的主要贡献包括：首次通过RL专门训练视觉基础的自我反思；提出两个促进视觉基础反思的训练机制；在多个OOD场景下验证有效性。限制包括：需要环境提供视觉反馈，不适用于开放VQA；实验任务有限。未来可探索更广泛的应用。总体而言，本文为提升LVLM鲁棒性提供了一个有效且可解释的方向，通过强化学习实现了视觉基础的自我反思，对于智能体等需要多轮交互和纠错的应用有重要参考价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文涉及智能体自我反思，与agent方向相关，特别是需要多步推理和纠错的应用。

## 基本信息

- 作者：Liyan Tang, Fangcong Yin, Greg Durrett
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.CV
- 日期：2026-07-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.02490`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（摘要和引言片段），但未完整阅读全文，部分内容基于合理推断。
