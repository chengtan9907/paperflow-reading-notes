---
user_id: "cheng tan"
paper_id: 2474
arxiv_id: "2607.02490"
title: "Visually Grounded Self-Reflection for Vision-Language Models via Reinforcement Learning"
publish_date: "2026-07-03"
pdf_url: "https://arxiv.org/pdf/2607.02490"
abs_url: "https://arxiv.org/abs/2607.02490"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-03T22:02:23"
---
# Visually Grounded Self-Reflection for Vision-Language Models via Reinforcement Learning

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：vision-language model · self-reflection · reinforcement learning · visual grounding

## 一句话总结

我们提出VRRL框架，通过随机轨迹掩码和缓冲回滚注入两种机制，促进视觉语言模型在思考链中进行视觉接地自我反思。

## 摘要

> Large vision-language models can reason over multimodal inputs by generating textual chains of thought (CoT). A key capability exhibited in CoT reasoning is self-reflection: revisiting earlier decisions and correcting previous errors. However, existing LVLMs often fail to properly attend to visual inputs during reflection, limiting their ability to translate feedback into grounded corrections, especially for out-of-distribution images. To address this issue, we propose a novel reinforcement learning training framework VRRL, with two components explicitly designed to elicit visually grounded self-reflection. First, we randomly mask trajectory prefixes during training to emphasize recovery from incorrect intermediate predictions rather than making early mistakes. Second, we introduce buffered roll-ins from an experience replay buffer to expose the model to diverse failure states that it must learn to correct. We evaluate our approach on visual grounding tasks involving tables and charts, as well as spatial navigation benchmarks. While off-the-shelf and conventionally fine-tuned models degrade substantially under distribution shift, our method substantially improves average out-of-distribution accuracy over standard RL and reflection-oriented fine-tuning baselines by using self-reflection effectively. $^{1}$

Q1: 这篇论文试图解决什么问题？

论文试图解决现有视觉语言模型在思考链推理中自我反思能力不足的问题，特别是在反思阶段未能有效参照视觉信息，导致在分布外图像上表现退化。

Q2: 有哪些相关研究？

相关研究包括：1) 视觉语言模型的思考链推理（CoT），如VL-Rethinker等；2) 自我反思机制在语言模型中的应用，如Reflection Tuning (Wu et al., 2025a)；3) 使用强化学习训练视觉语言模型的方法，如RLHF、PPO等。VRRL区别于现有工作在于其显式设计视觉接地的反思训练策略。

Q3: 论文如何解决这个问题？

VRRL框架包含两个关键组件：1) 随机轨迹掩码（Random Turn Masking）：在训练时随机选择轨迹的后缀部分计算策略更新，迫使模型从早期错误中恢复，而非单纯避免错误；2) 缓冲回滚注入（Buffered Roll-In）：从经验回放缓冲区中采样历史错误前缀，让模型在此基础上继续生成，暴露于多样化失败状态从而学习纠正。

Q4: 论文做了哪些实验？

论文在两类任务上评估：1) 表格与图表视觉定位：要求模型定位图像中的特定区域；2) 空间导航基准：如Room-to-Room导航。实验设置了分布内和分布外（OOD）测试集，比较了基线包括原始LVLM、标准RL微调、反射微调等方法。

Q5: 发现了什么实验现象？

实验发现：1) 标准模型和常规微调模型在分布偏移下性能显著下降；2) VRRL在OOD准确率上显著优于RL和反射微调基线；3) 消融实验显示随机轨迹掩码和缓冲回滚注入均对提升有贡献，且视觉接地的反思比仅文本反思更有效。

Q6: 有什么可以进一步探索的点？

进一步探索方向包括：1) 将VRRL应用于无需环境反馈的任务，如开放域视觉问答；2) 扩展到更多多模态推理场景，如视频理解；3) 结合更复杂的强化学习算法；4) 研究反思次数的动态调整。

Q7: 总结一下论文的主要内容

本文针对视觉语言模型在思考链推理中自我反思缺乏视觉接地的问题，提出VRRL强化学习训练框架。方法通过随机轨迹掩码和缓冲回滚注入两个组件，迫使模型在训练中学会基于视觉反馈纠正错误。实验覆盖表格图表定位和空间导航任务，在分布外条件下取得显著提升。论文还分析了模型失效案例和组件贡献，展示了视觉接地反思的有效性。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：论文聚焦于视觉语言模型的推理鲁棒性，与智能体系统中的自我修正机制相关

## 基本信息

- 作者：Liyan Tang, Fangcong Yin, Greg Durrett
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.CV
- 日期：2026-07-03
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.02490`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据。
