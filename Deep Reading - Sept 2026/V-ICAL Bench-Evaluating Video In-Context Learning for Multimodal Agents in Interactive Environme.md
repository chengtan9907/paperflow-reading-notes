---
user_id: "cheng tan"
paper_id: 11739
arxiv_id: "2609.15683"
title: "V-ICAL Bench: Evaluating Video In-Context Learning for Multimodal Agents in Interactive Environments"
institution: "VisionXLab"
publish_date: "2026-09-15"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.15683.pdf"
pdf_url: "https://arxiv.org/pdf/2609.15683"
abs_url: "https://arxiv.org/abs/2609.15683"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-16T11:18:04"
---
# V-ICAL Bench: Evaluating Video In-Context Learning for Multimodal Agents in Interactive Environments

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：video in-context learning · multimodal agents · interactive environments · benchmark

## 一句话总结

V-ICAL 是一个评估多模态智能体在交互式环境中通过视频演示进行上下文学习（Video ICL）能力的基准测试，揭示了当前 SOTA 模型与人类水平之间的巨大差距。

## 摘要

> While In-Context Learning (ICL) enables models to adapt from exemplars without parameter updates, multimodal ICL remains largely underexplored, particularly regarding video demonstrations in interactive environments. For multimodal agents, learning from videos presents unique challenges: they must translate in-context demonstrations into executable policies, ground these policies in novel visual states, and iteratively refine actions based on environmental feedback. We introduce V-ICAL, a novel benchmark designed to evaluate video-based ICL for multimodal agents. Comprising 342 interactive tasks across 37 environments, V-ICAL utilizes human-curated demonstration videos as task-specific behavioral exemplars, evaluating agents through sustained interaction from a target initialization. The benchmark seamlessly connects in-context knowledge induction with core agentic capabilities, including state grounding, temporal memory, planning, and adaptation in dynamic environments. Extensive evaluations across 19 state-of-the-art multimodal agents reveal significant limitations: the best-performing model, Seed-2.1-Pro, achieves a score of only 54.4/100, while other leading models (e.g., Gemini-3.1-Pro, GPT-5.6) fail to surpass 50, far below the human baseline of 83.6. Controlled comparisons further demonstrate that current agents struggle to reliably translate video exemplars into effective policies, failing to yield consistent performance gains. Ultimately, V-ICAL exposes a critical gap in the ICL capabilities of multimodal agents, underscoring an urgent need for future research.
> Date: September 15, 2026
> Code: https://github.com/VisionXLab/V-ICAL
> Dataset: https://huggingface.co/datasets/VisionXLab/V-ICAL
> Homepage: https://visionxlab.github.io/V-ICAL-Homepage

Q1: 这篇论文试图解决什么问题？

### 核心挑战：视频上下文学习的复杂性
这篇论文试图解决多模态智能体在交互式环境中如何通过“看视频”来学习新任务的问题。传统的上下文学习（ICL）主要集中在文本或静态图像上，而视频演示引入了独特的挑战：
1. **视频到策略的翻译（Video-to-Policy Translation）**：智能体必须从非结构化的像素流中提取出潜在的动作序列、操作逻辑和任务目标。
2. **跨状态的策略对齐（State Grounding）**：演示视频中的视觉状态（如物体位置、背景）通常与智能体当前面临的初始状态不同，智能体需要具备强大的泛化能力，将演示中的行为映射到新的视觉上下文中。
3. **闭环交互中的迭代优化**：在动态环境中，智能体不仅要执行动作，还要根据环境的实时反馈（Feedback）不断调整后续规划，这要求模型具备极强的时间记忆和动态适应能力。

### 现有研究的局限性
目前的评估体系大多侧重于被动的视频理解（如视频问答）或简单的静态图像 ICL，缺乏一个能够衡量智能体在“感知-决策-行动-反馈”闭环中利用视频示例能力的系统性基准。现有的具身智能体研究往往依赖于大规模的微调或强化学习，而忽略了像人类一样通过少量视频演示即时习得新技能的能力。

Q2: 有哪些相关研究？

### 上下文学习（ICL）的演进
ICL 最初由 GPT-3 普及，展示了大型语言模型（LLM）通过输入示例进行少样本学习的能力。随后，研究者将其扩展到多模态领域（Multimodal ICL），如 Flamingo 等模型通过图像-文本对实现了跨模态的少样本迁移。

### 具身智能体与交互环境
在具身智能（Embodied AI）领域，研究重点已从简单的导航转向复杂的物体操控。然而，大多数工作仍采用传统的监督学习或强化学习范式。虽然有一些研究尝试在交互环境中使用文本指令或图像示例，但利用视频作为演示源的研究相对匮乏。

### 视频理解与动作预测
现有的视频基准（如 Ego4D, Kinetics）主要关注动作识别或时空定位。虽然有些工作尝试从视频中学习奖励函数或世界模型，但将视频演示直接作为 ICL 的输入，并要求智能体在闭环环境中执行，是本论文 V-ICAL 区别于以往工作的核心点。

Q3: 论文如何解决这个问题？

### V-ICAL 基准设计框架
论文提出了 **Vision In-Context Agentic Learning (V-ICAL)**，这是一个专门为多模态智能体设计的视频 ICL 评估框架。其核心逻辑是将视频 ICL 形式化为一个多轮序列决策问题。

### 任务与环境构建
1. **规模与多样性**：包含 342 个精心策划的评估任务，分布在 37 个不同的环境和 7 个环境族中。这些环境涵盖了从简单的物体移动到复杂的逻辑推理任务。
2. **演示视频（Exemplars）**：利用人工策划的视频作为任务特定的行为示例。这些视频展示了完成任务的正确步骤和策略。
3. **交互协议**：智能体被部署在一个闭环系统中。在每一轮中，智能体接收：
 - 一个或多个视频演示。
 - 当前环境的视觉观察。
 - 历史动作和反馈记录。
 智能体随后输出下一步动作，环境返回新的观察和奖励/反馈信号。

### 核心评估维度
- **状态对齐（State Grounding）**：考察智能体能否在不同于演示视频的初始状态下正确执行任务。
- **时间记忆（Temporal Memory）**：评估智能体在长序列任务中保持目标一致性的能力。
- **规划与适应（Planning & Adaptation）**：测试智能体在面对环境动态变化或执行失败时，能否根据反馈调整策略。

Q4: 论文做了哪些实验？

### 实验设置与模型选择
研究团队对 19 个最先进的多模态智能体进行了大规模评估，包括：
- **闭源模型**：GPT-5.6 (推测为最新版本代号), Gemini-3.1-Pro 等。
- **开源模型**：Seed-2.1-Pro, LLaVA 系列等。
- **基准线**：设置了人类基准（Human Baseline），得分为 83.6，作为性能上限。

### 评估指标
主要采用任务成功率（Success Rate）和归一化得分。实验特别区分了“规则转移”（Rule Transfer，即学习简单的操作规程）和“策略转移”（Strategy Transfer，即学习复杂的应对逻辑）。

### 对照实验设计
为了隔离视频演示的影响，研究者进行了消融实验：
- **无演示（Zero-shot）**：仅提供任务描述，不提供视频。
- **文本演示 vs 视频演示**：对比不同模态示例对智能体性能的提升效果。
- **跨环境测试**：在演示视频与测试环境存在显著视觉差异的情况下评估模型的鲁棒性。

Q5: 发现了什么实验现象？

### 关键实验发现
1. **巨大的性能鸿沟**：即使是表现最好的模型 Seed-2.1-Pro 也仅获得了 54.4/100 的分数，而 GPT-5.6 和 Gemini-3.1-Pro 均未能突破 50 分。这表明当前最强模型在视频 ICL 能力上仍处于初级阶段。
2. **策略转移的匮乏**：实验揭示了一个共同的局限性——智能体在“策略转移”任务上的得分远低于“规则转移”。这意味着模型可以模仿简单的动作序列，但难以理解并迁移视频中蕴含的高层逻辑或应对策略。
3. **视频转化的低效性**：受控对比显示，当前智能体往往无法可靠地将视频演示转化为有效的执行策略。在某些情况下，增加视频演示甚至没有带来预期的性能增益，显示出模型在处理高维度视频信息时的“信息过载”或“误解”。
4. **反馈利用不足**：在闭环交互中，当智能体执行动作失败时，它们往往会重复错误的动作，而不是根据环境反馈进行有效的重新规划。这反映了模型在动态适应能力上的缺失。
5. **视觉对齐难题**：当测试环境的视觉特征（如颜色、视角）与演示视频稍有不同时，模型的成功率会大幅下降，说明其状态对齐能力极度脆弱。

Q6: 有什么可以进一步探索的点？

### 潜在的研究方向
1. **增强视频表征的动作语义**：未来的模型需要更深层次地理解视频中的因果关系和动作意图，而不仅仅是像素级的特征匹配。
2. **长上下文与时间推理优化**：针对视频 ICL 的特点，开发能够处理长视频序列并保持长期记忆的架构，以支持更复杂的任务。
3. **改进闭环反馈机制**：研究如何让智能体更有效地从环境反馈中学习，实现实时的策略修正和自我进化。
4. **跨模态对齐预训练**：在预训练阶段引入更多“视频-动作-状态变化”的三元组数据，以增强模型对交互逻辑的先验认知。
5. **鲁棒的状态对齐算法**：开发能够忽略环境噪声、专注于核心任务逻辑的视觉对齐技术，提高模型在异构环境下的泛化能力。

Q7: 总结一下论文的主要内容

本文针对多模态智能体在交互环境下的视频上下文学习（Video ICL）能力缺失问题，提出了 V-ICAL 基准测试。该基准包含 342 个任务，覆盖 37 个环境，要求智能体从人类演示视频中归纳规则并执行。通过对 19 个 SOTA 模型的深度评测，研究发现当前最先进模型（如 GPT-5.6, Gemini-3.1-Pro）在处理此类任务时存在严重局限，特别是在策略提取和环境适应方面，得分远低于人类水平。实验进一步揭示了模型在“策略转移”上的短板，以及在闭环反馈利用和视觉状态对齐上的脆弱性。V-ICAL 不仅提供了一个严苛的评估平台，也为未来开发具备真正“看即学会”能力的多模态智能体指明了技术瓶颈和改进方向。该研究强调，从简单的模式匹配转向深层的策略理解与闭环执行是通往通用人工智能（AGI）的关键一步。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该工作直接关联智能体（Agent）和多模态学习方向，是当前具身智能的热点

## 基本信息

- 作者：Ziqian Fan, Shibo Xu, Junjie Li, Xiangyu Zhao, Shengyuan Ding, Yifan Yang, Zhenjie Yang, Haodong Duan, Yue Zhou, Zhihang Zhong, Xue Yang
- 机构：VisionXLab
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-09-15
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.15683`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，重点结合了摘要、引言和结论部分的详细数据，确保了实验数值和结论的准确性。
