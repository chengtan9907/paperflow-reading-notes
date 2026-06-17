---
user_id: "cheng tan"
paper_id: 123
arxiv_id: "2606.13679v1"
title: "InterleaveThinker: Reinforcing Agentic Interleaved Generation"
publish_date: "2026-06-11"
pdf_path: "/Users/mario/Documents/Obsidian Vault/Daily Note/Daily Note 2026/arXiv - Jun 2026/2606.13679v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.13679v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
report_version: "2026-06-04-v5"
daily_note_path: "/Users/mario/Documents/Obsidian Vault/Daily Note/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-15T01:21:03"
---
[[Daily Note - Jun 2026]]

- PDF：[https://arxiv.org/pdf/2606.13679v1](https://arxiv.org/pdf/2606.13679v1)

# InterleaveThinker: Reinforcing Agentic Interleaved Generation

- PDF: https://arxiv.org/pdf/2606.13679v1
- 原文: https://arxiv.org/abs/2606.13679v1

> ★★☆☆☆ 按需阅读 · 约 20 分钟 · 模型 openai-compatible/gemini-3-flash-preview · 证据 PDF 全文 + 元数据

🏷 关键词：interleaved generation · multi-agent system · reinforcement learning · grpo

## 一句话总结

InterleaveThinker 通过 Planner 和 Critic 双智能体协作及基于 GRPO 的强化学习，赋予现有图像生成器强大的交错文本图像序列生成与自我修正能力。

## 摘要

> Recent image generators have demonstrated impressive photorealism and instruction-following
> capabilities in single-image generation and editing. However, constrained by their architectures,
> they cannot achieve interleaved generation (text-image sequence), which has crucial applications
> in visual narratives, guidance, and embodied manipulation. Even the latest open-source Unified
> Multimodal Models (UMMs) exhibit limited performance in this regard. In this paper, we introduce
> InterleaveThinker, the first multi-agent pipeline designed to endow any existing image generator with
> interleaved generation capabilities. Specifically, we employ a planner agent to organize the image-text
> input sequence, instructing the image generator on the required execution at each step. Subsequently,
> we introduce a critic agent to evaluate the generator’s outputs, identify samples that deviate from
> the planned instructions, and refine the instructions for regeneration. To implement this pipeline,
> we construct the Interleave-Planner-SFT-80k and Interleave-Critic-SFT-112k to perform a format
> InterleaveThinker: Reinforcing Agentic Interleaved Generation
> cold-start. Then we develop Interleave-Critic-RL-13k to reinforce the step-wise instruction correction
> capability within a generation trajectory using GRPO. Since a single interleaved generation trajectory
> may involve over 25 generator calls, optimizing the entire trajectory is computationally impractical.
> Therefore, we propose accuracy reward and step-wise reward, allowing single-step RL to effectively
> guide the entire generation trajectory. The results show that InterleaveThinker improves performance
> across various image generators. On interleaved generation benchmarks, it achieves performance
> comparable to Nano Banana and GPT-5. Surprisingly, it also significantly enhances the base model
> on reasoning-based benchmarks; for example, on 4-step FLUX.2-klein, we observe substantial gains
> on WISE (from 0.47 to 0.73) and RISE (from 13.3 to 28.9)

Q1: 这篇论文试图解决什么问题？

该论文主要解决了交错生成（Interleaved Generation）任务中的三个核心痛点：
1. 架构限制：现有的高性能图像生成器（如 FLUX）本质上是为单图生成设计的，无法原生处理需要多步图文交替的复杂序列任务。
2. 统一多模态模型（UMMs）的缺陷：虽然 UMMs 支持交错输入输出，但在长程任务中容易出现“视觉过度依赖”（Visual Over-reliance），即模型可能因为中间生成的图像在视觉上与目标相似而错误地停止动作；同时，它们缺乏自我修正机制，导致早期步骤的微小误差在后续步骤中不断累积（Step-wise Error Accumulation）。
3. 训练与优化难题：一个完整的交错生成轨迹可能包含超过 25 次生成器调用，直接对整个轨迹进行强化学习在计算上是不可行的，需要一种更高效的对齐策略。

Q2: 有哪些相关研究？

相关研究主要集中在两个方向：
1. 图像生成与编辑模型：如 FLUX.2-klein 和 Qwen-Image-Edit 等，这些模型在单图质量和指令遵循上表现优异，但缺乏处理序列化任务的逻辑规划能力。
2. 统一多模态模型（UMMs）：如 Chameleon、Emu 和最近的 Qwen-VL 系列。虽然这些模型在架构上支持交错生成，但在处理需要严密逻辑推理的长序列任务时，往往表现出较弱的鲁棒性和自我纠错能力。此前虽有研究尝试利用 LLM 作为智能体驱动生成工具，但缺乏针对交错生成轨迹的系统性强化学习优化框架。

Q3: 论文如何解决这个问题？

InterleaveThinker 采用了一种解耦规划与生成的双智能体架构：
1. 智能体分工：Planner 智能体负责将任务分解为具体的执行步骤并提供指令；Critic 智能体负责评估生成器的输出，识别与指令不符的偏差，并生成修正建议驱动重新生成。
2. 数据集构建：构建了 Interleave-Planner-SFT-80k 和 Interleave-Critic-SFT-112k 用于格式冷启动，确保智能体掌握基本的规划与评价逻辑。
3. 强化学习策略：开发了 Interleave-Critic-RL-13k，使用 GRPO 算法增强 Critic 的步进式指令修正能力。为了降低计算开销，提出了“双重奖励机制”：Accuracy Reward 用于评估单步生成的准确性，Step-wise Reward 用于引导整个轨迹的对齐，从而实现高效的单步 RL 优化。

Q4: 论文做了哪些实验？

实验在多个基准测试上验证了系统的通用性和有效性：
1. 实验设置：智能体底座采用 Qwen3-VL-8B，生成器包括 FLUX.2-klein-9B 和 Qwen-Image-Edit-2511。在 H800 GPU 集群上完成了训练。
2. 交错生成评估：在 UEval 和 CoMM 基准上，InterleaveThinker 显著优于现有的开源 UMMs，表现接近 GPT-5 和 Nano Banana。
3. 推理生成评估：在 WISE（图像生成推理）和 RISE（图像编辑推理）上，InterleaveThinker 大幅提升了基础模型性能。例如，FLUX.2-klein 在 WISE 上的得分从 0.47 提升至 0.73，在 RISE 上的得分从 13.3 提升至 28.9。
4. 消融实验：验证了最大迭代次数 Tmax 的影响，结果显示增加修正次数能持续提升生成质量，证明了 Critic 模块的有效性。

Q5: 有什么可以进一步探索的点？

论文指出了几个值得进一步探索的方向：
1. 推理效率优化：目前的多次迭代修正机制虽然保证了质量，但增加了推理延迟，未来可以研究如何通过模型蒸馏或更高效的架构减少迭代次数。
2. 跨模态一致性：在极长序列中，如何更好地保持角色、场景在视觉和文本描述上的一致性仍有提升空间。
3. 自动化数据闭环：探索如何利用模型生成的负样本进行自我博弈（Self-play），进一步减少对高质量人工标注数据的依赖。
4. 扩展至视频生成：将这种规划-修正的智能体框架应用到视频序列生成中，处理更复杂的时空一致性问题。

Q6: 总结一下论文的主要内容

本文提出了 InterleaveThinker，一个旨在增强图像生成器交错生成能力的智能体框架。针对现有模型在处理长程图文序列时存在的逻辑断裂和误差累积问题，作者设计了由 Planner 和 Critic 组成的双智能体系统。Planner 负责任务分解与指令下发，Critic 负责质量监控与反馈修正。通过大规模 SFT 数据初始化和基于 GRPO 的强化学习，并结合创新的双重奖励机制，该系统成功实现了对生成轨迹的高效优化。实验结果表明，InterleaveThinker 不仅能让普通的单图生成器胜任复杂的交错生成任务，还在多项推理基准测试中刷新了性能记录，展现了智能体架构在多模态生成领域的巨大潜力。

## PDF 证据定位

- 研究背景：
  Abstract | score=1.231 | Recent image generators have demonstrated impressive photorealism and instruction-following capabilities in single-image generation and editing. However, constrained by their…
  Abstract | score=1.175 | pipeline, we construct the Interleave-Planner-SFT-80k and Interleave-Critic-SFT-112k to perform a format InterleaveThinker: Reinforcing Agentic Interleaved Generation cold-start.…
  Introduction | score=1.171 | search agents to guide knowledge-intensive image generation, [35, 36, 37, 38, 39] explore multi-turn refinement for image generation/editing and [23, 40] further employs RL to…
- 核心方法：
  Introduction | score=1.017 | prising an accuracy reward and a step-wise reward. This formulation achieves trajectory-level alignment through efficient single-step RL, drastically reducing computational…
  Introduction | score=1.010 | patible with arbitrary image generators. InterleaveThinker overcomes these limitations by decoupling planning and generation, preventing myopic reactions to local visual…
  Results | score=0.998 | forcing Agentic Interleaved Generation RL, we further introduce a dual-reward strategy that enables efficient single-step RL on the Critic to guide the entire generation…
- 主要结果：
  Results | score=1.076 | t relies on the maximum iteration count Tmax. Increasing Tmax consistently improves performance over the single-pass baseline (Tmax = 1), demonstrating the Critic’s…
  Results | score=0.936 | -klein-9B [2] 7.1 13.3 24.0 7.1 13.3 +InterleaveThinker (Ours) 36.5 33.3 34.0 10.6 28.9 Qwen-Image-Edit-2511 [3] 21.2 18.9 31.0 4.7 19.4 +InterleaveThinker (Ours) 27.1 38.9 39.0…
  Results | score=0.876 | apabilities into the critic, allowing the model + Full-SFT 58.6 70.4 64.5 to simultaneously plan the next step and evaluate the pre- + RL w/o step reward 58.2 72.2 65.2 vious…
- 局限性：
  Introduction | score=0.967 | terleaved generation data across diverse scenarios, resulting in three high-quality datasets: Interleave-Planner-SFT-80k, Interleave-Critic-SFT-112k, and…
  Introduction | score=0.956 | n lower back. and textually. (a) Output Restriction (b) Visual Over-reliance How to draw a shop storefront? Show each step UMM Text… Text… Text… Text… both visually and…
  Introduction | score=0.954 | Recent advancements in image generation and editing have demonstrated remarkable photorealism and instructionfollowing capabilities. However, these models [1, 2, 3, 4, 5, 6] are…
- 相关性：
  Introduction | score=1.179 | kflow examples in Fig 4. 3.2 Dataset Construction Pipeline High-quality training data is essential for developing agents capable of long-horizon planning and step-wise…
  Introduction | score=1.171 | of image generation models [1, 5, 19, 3, 20]. Building upon these foundational architectures, researchers have developed robust image editing models [21, 3, 4, 22, 23, 24, 2…
  Introduction | score=1.168 | generation. However, because they generate sequences step-by-step based on preceding images, UMMs suffer from two critical problems in long-horizon tasks: 1) Visual…

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：该研究直接关联智能体（Agent）与生成（Generation）方向，展示了如何用 Agent 框架封装传统生成模型。

## 基本信息

- 作者：Dian Zheng, Harry Lee, Manyuan Zhang, Kaituo Feng, Zoey Guo, Ray Zhang, Hongsheng Li
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-06-11
- 推荐级别：**按需阅读**
- 预计阅读时间：约 20 分钟
- 解析来源：PDF 全文 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2606.13679v1`

## 代码与资源

- 代码：暂未发现公开链接
- 数据：暂未发现公开链接
- 项目主页：暂未发现公开链接
- 原文 PDF：https://arxiv.org/pdf/2606.13679v1
- arXiv：https://arxiv.org/abs/2606.13679v1
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成深度参考了 PDF 检索证据，特别是关于双重奖励机制、数据集规模及实验数值的细节。
- 方法证据锚点：Introduction | score=1.017 | prising an accuracy reward and a step-wise reward. This formulation achieves trajectory-level alignment through efficient single-step RL, drastically reducing computational…
- 结果证据锚点：Results | score=1.076 | t relies on the maximum iteration count Tmax. Increasing Tmax consistently improves performance over the single-pass baseline (Tmax = 1), demonstrating the Critic’s…
