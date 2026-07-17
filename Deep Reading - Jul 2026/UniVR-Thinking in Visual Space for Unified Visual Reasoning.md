---
user_id: "cheng tan"
paper_id: 3852
arxiv_id: "2607.12800v1"
title: "UniVR: Thinking in Visual Space for Unified Visual Reasoning"
publish_date: "2026-07-14"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.12800v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.12800v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-16T01:57:25"
---
# UniVR: Thinking in Visual Space for Unified Visual Reasoning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：visual reasoning · reinforcement learning · planning · physical dynamics

## 一句话总结

论文首次提出从纯视觉演示中同时学习复杂推理、细粒度物理动力学和长期规划的统一框架UniVR。

## 摘要

> Learning broad world knowledge directly from raw visual data is a fundamental capability of intelligence. We introduce UniVR, the first investigation into simultaneously learning complex reasoning, fine-grained physical dynamics, and long-term planning from pure visual demonstrations. At its core, UniVR features VR-GRPO, a reinforcement learning paradigm with complementary global and step-level rewards. This approach enforces logical coherence and physical consistency throughout the reasoning process without requiring task-specific heuristics or image-text pairs. To train and evaluate UniVR, we construct VR-X, a large-scale benchmark curated from 16 diverse sources spanning long-horizon manipulation, spatial puzzles, and physical reasoning. It is the first comprehensive suite to assess these heterogeneous capabilities under a purely visual protocol. Remarkably, UniVR achieves up to a 25% improvement on VR-X, and its superior visual reasoning also boosts performance on various multimodal understanding benchmarks. These findings underscore the vast potential of reasoning within visual spaces, with all code, data, and models are open-sourced for further research.
> Date: July 15, 2026
> Correspondence: Xiaojiao Jin, Yunchao Wei
> Project Page: https://UniVR.github.io/

Q1: 这篇论文试图解决什么问题？

现有视觉推理方法大多依赖语言模型或文本-图像对，未能充分利用视觉空间中的直接推理。纯视觉演示包含丰富的状态转移和物理动态，但缺乏有效方法从中学习复杂推理、细粒度物理理解和长期规划。UniVR试图填补这一空白，通过强化学习直接从图像序列学习视觉推理能力。

Q2: 有哪些相关研究？

相关研究包括视觉语言模型（如Gemini、Qwen-VL）、视频预测模型、基于RL的规划方法等。传统方法将视觉输入转换为语言表示进行推理，丢失了视觉细节。UniVR是首个纯视觉推理框架，直接建模视觉空间中的状态转移。缺乏直接对比方法，所以实验与现有VLM进行对比。由于检索证据未提供详细的相关工作讨论，此处根据摘要合理推断。

Q3: 论文如何解决这个问题？

UniVR采用VR-GRPO（Visual Reasoning Group Relative Policy Optimization）训练范式。给定图像序列x_{1:t}和指令，模型建模下一个帧分布p(x_{t+1}|x_{1:t})。使用包含任务执行轨迹的图像序列，无需密集文本推理链。VR-GRPO包括一个VLM评估器提供全局评估，以及逐步奖励信号。通过全局和步骤级奖励组合，确保逻辑和物理一致性。不确定性估计用于识别推理路径分歧点。模型基于预训练的视频预测框架，在VR-X数据集上通过强化学习优化。

Q4: 论文做了哪些实验？

实验在VR-X基准上进行，包含1.8k高质量推理轨迹，来自长期操作、空间谜题、物理推理等16个来源。指标：VLM score（视觉语言模型评分）和JEPA similarity（联合嵌入预测相似度）。对比基线包括现有VI-VL模型（如Emu3.5、Gemini、Qwen-VL）和纯视觉方法。在VR-X上，UniVR超越基线多达24.3%。还在多模态理解基准（如MMBench、MMLU等）上测试，显示提升。消融研究验证VR-GRPO各组件贡献。实验还分析不同任务类型上的表现，以及视觉一致性问题。

Q5: 发现了什么实验现象？

观察到UniVR在VR-X上显著提升，但复杂动态和规划任务中视觉不一致性仍然存在。语言模型演进（Gemini 3 vs 2.5 Pro，Qwen 3.5 vs 3-VL）仅带来边际改善。原生预训练的Emu3.5获得最高分，归功于日常和手工序列训练。VR-GRPO的全局和逐步奖励组合关键。模型在空间推理任务中表现最好，在长时间规划任务中仍存在失败案例。不确定性估计能识别模型分歧点。

Q6: 有什么可以进一步探索的点？

可探索方向包括：扩展到更多任务类型，如交互式环境；结合语言指导以处理歧义；改进视觉一致性；扩展到真实机器人平台；利用更大规模数据和模型；探索视觉推理与其他模态融合；研究基座模型与RL结合的scaling law。由于论文主要关注方法介绍，未来方向可推断为：提高复杂动态建模能力、降低计算成本、应用到具身智能等。

Q7: 总结一下论文的主要内容

UniVR论文首次系统研究了直接从纯视觉演示学习复杂推理、物理动态和长期规划的问题。提出VR-GRPO强化学习范式，通过全局和步骤级奖励确保推理一致性。构建大型基准VR-X。在多种任务上取得显著提升，证明了视觉空间推理的潜力。开源代码、数据和模型。论文论证了纯视觉推理的可行性，技术主线是RL优化序列预测，实验主线是基准评估和消融。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对智能体领域：直接视觉推理能力可提升机器人规划

## 基本信息

- 作者：Zhongwei Ren, Yunchao Wei, Yao Zhao, Weibo Gong, Xiao Liu, Anran Wang, Xiangtai Li, Xiaojie Jin
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-14
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.12800v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本生成主要参考了PDF语义检索命中的证据片段，特别是Abstract、Method、Results和Limitations部分。对于相关工作和未来方向等缺乏明确证据的字段，基于论文内容合理推断。
