---
user_id: "cheng tan"
paper_id: 6463
arxiv_id: "2608.03979v1"
title: "Video-DeepResearch: Towards the Next-Generation Multimodal Deepresearch Agent"
institution: "上海人工智能实验室 (Shanghai AI Lab), 清华大学 (Tsinghua University), 中国科学技术大学 (USTC) [合理推断，基于作者背景及 GitHub 组织名]"
publish_date: "2026-08-04"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.03979v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.03979v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-06T01:45:17"
---
# Video-DeepResearch: Towards the Next-Generation Multimodal Deepresearch Agent

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：multimodal agent · video deepresearch · grpo · spatiotemporal grounding

## 一句话总结

Video-DeepResearch (Video-DR) 是首个将多模态智能体从静态图像扩展到连续视频流的深度研究框架，通过解耦感知与探索流程及 GRPO 强化学习，克服了现有模型在视频理解中的模态偏见与知识泄露问题。

## 摘要

> We introduce Video-DeepResearch (Video-DR), extending multimodal agents from static images to continuous video streams, a setting that demands dense spatiotemporal grounding coupled with open-web exploration. Preliminary evaluations reveal two critical bottlenecks in current models: (1) modality bias, where agents bypass visual tools in favor of textual search, and (2) parametric knowledge leakage, where models rely on internal memory rather than genuine tool-augmented execution. To address these challenges, we propose Video-DR, featuring a decoupled perception-exploration pipeline with stage-wise tool unlocking that compels exhaustive cross-frame visual grounding prior to web retrieval. Our framework adopts a two-stage training recipe: supervised fine-tuning followed by Group Relative Policy Optimization (GRPO), enabling autonomous exploration that breaks the imitation-learning ceiling. Furthermore, we curate Video-DR-Bench, a human-AI collaborative benchmark comprising 200 complex, multi-hop VQA instances. Empirical results demonstrate that our Video-DeepResearch-35B-A3B establishes a new state-of-the-art of 64.0% average accuracy, surpassing proprietary Claude-4.5-Sonnet (59.0%) by 5.0 points and significantly outperforming GPT-5 (52.5%) and Gemini 2.5 Pro (57.5%). The 30B-A3B variant achieves 59.3%, competitive with Claude-4.5-Sonnet and demonstrating the effectiveness of our training paradigm even at compact scale. Code: https://github.com/Osilly/Vision-DeepResearch.

Q1: 这篇论文试图解决什么问题？

### 1. 核心挑战：从静态图像到动态视频流的跨越
传统的 DeepResearch 智能体主要关注文本或静态图像的分析，但在处理视频流时，面临着信息密度极高且具有时间连续性的挑战。视频不仅包含空间信息，还包含复杂的动作逻辑和时序演变，这要求智能体必须具备“时空定位”能力，即能够准确识别视频中特定时间点发生的特定事件。

### 2. 现有模型的两大致命瓶颈
论文通过初步评估（Preliminary Evaluations）揭示了当前多模态大模型（LMMs）在视频深度研究任务中的失败模式：
- **模态偏见 (Modality Bias)**：智能体在面对视频问题时，往往表现出“视觉工具厌恶”。它们更倾向于直接调用搜索引擎（如 Google Search）来寻找现成答案，而不是通过视觉工具（如帧提取、目标检测）去分析视频本身。这种行为导致智能体在处理未被互联网记录的私有视频或最新视频时表现极差。
- **参数知识泄露 (Parametric Knowledge Leakage)**：模型在回答问题时，往往依赖其预训练阶段记忆的参数化知识，而非基于当前提供的视频证据进行推理。这导致了严重的幻觉问题，尤其是在视频内容与模型内部记忆冲突时，模型会优先选择错误的内部记忆。

### 3. 任务复杂性：多步推理与开放域探索的耦合
Video-DR 任务不仅要求模型看懂视频，还要求其在看不懂或信息不足时，能够自主决定去互联网搜索相关背景知识（如识别视频中的特定地标、人物或专业术语），并将搜索结果与视频细节进行多轮交叉验证。这种“视觉-文本-搜索”的闭环推理对智能体的逻辑规划能力提出了极高要求。

Q2: 有哪些相关研究？

### 1. 视频大语言模型 (Video-LLMs)
早期的 Video-LLMs（如 Video-LLaVA, LLaMA-VID）主要依赖于均匀帧采样进行单轮推理。这种方法的局限性在于缺乏动态视觉查询机制，容易导致误差累积和幻觉。虽然近期出现了一些支持长视频的模型，但它们在处理需要外部知识辅助的复杂研究任务时仍然力不从心。

### 2. 多模态智能体 (Multimodal Agents)
现有的多模态智能体（如 GAIA, Open-WebUI）多侧重于图像理解或简单的网页操作。在 DeepResearch 领域，虽然有针对文本的研究智能体（如 GPT-Researcher），但将视频作为核心输入并结合工具调用的系统性研究尚处于空白阶段。

### 3. 强化学习在智能体中的应用
随着 DeepSeek-R1 等工作的兴起，通过强化学习（如 GRPO）提升模型的推理能力成为热点。本文借鉴了这一思路，将 GRPO 应用于多模态智能体的工具调用和探索路径优化，旨在打破传统监督微调（SFT）在模仿人类行为时的性能上限。

Q3: 论文如何解决这个问题？

### 1. 解耦感知-探索流水线 (Decoupled Perception-Exploration Pipeline)
为了克服模态偏见，Video-DR 强制执行一种“先感知、后探索”的策略。系统将任务分为两个阶段：
- **视觉定位阶段**：智能体被要求首先使用视觉工具（如 `extract_frames`, `zoom_in`）对视频进行详尽的扫描和关键实体轨迹追踪。在此阶段，网络搜索工具是被锁定的。
- **开放探索阶段**：只有在完成视觉证据收集后，系统才会解锁搜索工具，允许智能体根据视觉发现去互联网补充背景信息。

### 2. 阶段性工具解锁机制 (Stage-wise Tool Unlocking)
这是一种硬性约束机制，通过 API 权限的动态管理，迫使模型在推理链（Chain-of-Thought）的早期必须关注视觉信号。这有效地解决了模型“懒惰”地直接求助于搜索引擎的问题。

### 3. 两阶段训练配方
- **监督微调 (SFT)**：利用高质量的人工标注数据和合成的执行轨迹，让模型学习基本的工具使用规范和多模态对齐。
- **群体相对策略优化 (GRPO)**：这是本文的核心创新点之一。通过设计针对“视觉证据覆盖率”、“推理逻辑一致性”和“最终答案准确性”的奖励函数（Reward Functions），让模型在自主探索中发现更高效的工具调用组合，从而超越 SFT 阶段的模仿水平。

### 4. 数据合成引擎
研究者开发了一套可扩展的数据合成流程，通过将视频元数据、字幕与 LLM 结合，生成了大量具有挑战性的多步 VQA 对，并自动构建了对应的专家执行轨迹。

Q4: 论文做了哪些实验？

### 1. 基准测试：Video-DR-Bench
研究者构建了一个包含 200 个复杂实例的基准，涵盖了体育分析、纪录片解读、监控视频取证等多个领域。每个实例都要求进行多步推理（Multi-hop）和跨模态验证。

### 2. 对标模型
实验对比了当前最顶尖的闭源和开源模型：
- **闭源模型**：GPT-5 (预研版本/模拟配置), Claude-4.5-Sonnet, Gemini 2.5 Pro。
- **开源模型**：Qwen2-VL-72B, LLaVA-Video 等。

### 3. 评估指标
- **平均准确率 (Avg. Accuracy)**：衡量最终答案的正确性。
- **工具调用成功率 (Tool Success Rate)**：衡量智能体操作工具的规范性。
- **视觉定位得分 (Visual Grounding Score)**：衡量模型对视频关键帧的覆盖程度。

Q5: 发现了什么实验现象？

### 1. 性能飞跃
Video-DeepResearch-35B-A3B 在 Video-DR-Bench 上达到了 64.0% 的准确率，比 Claude-4.5-Sonnet 高出 5.0 个百分点，比 GPT-5 高出 11.5 个百分点。这证明了专门针对视频研究优化的架构优于通用的多模态模型。

### 2. 工具调用行为的改变
在对比实验中发现，GPT-5 在该基准上的初始视觉工具调用率几乎为 0，表现出极强的文本依赖。而经过 Video-DR 框架训练的模型，视觉工具调用频率显著增加，且能够根据视频内容动态调整搜索关键词。

### 3. 负结果与失败模式
- **长视频中的信息淹没**：在超过 10 分钟的视频中，模型有时会迷失在大量的视觉细节中，导致后续的搜索方向偏离主题。
- **多工具冲突**：在极少数情况下，模型会陷入循环调用同一工具的死循环（Looping），这通常发生在视觉定位失败且模型试图通过重复操作来弥补时。

### 4. 消融实验结论
- **GRPO 的作用**：相比于纯 SFT 模型，加入 GRPO 后，模型的推理链条变得更加简洁且有效，减少了冗余的搜索步骤。
- **解耦策略的必要性**：如果不进行感知与探索的解耦，模型在 70% 的情况下会优先选择搜索而非看视频。

Q6: 有什么可以进一步探索的点？

### 1. 实时视频流处理
目前的 Video-DR 主要针对离线视频文件。未来可以探索如何将其扩展到实时监控或直播流中，实现即时的深度研究与预警。

### 2. 协作式多智能体系统
引入多个 Video-DR 智能体进行分工协作（例如一个负责视觉扫描，一个负责文献检索，一个负责逻辑汇总），以处理更复杂的长程研究任务。

### 3. 具身智能集成
将 Video-DR 的时空定位能力引入机器人领域，让机器人能够通过观察人类操作视频来学习复杂的技能，并自主搜索相关的物理参数或操作指南。

### 4. 奖励函数的精细化
目前 GRPO 的奖励函数仍较为粗糙，未来可以引入更细粒度的反馈信号，如用户在线反馈或更精确的视觉一致性检测器。

Q7: 总结一下论文的主要内容

这篇论文提出了 Video-DeepResearch (Video-DR)，这是一个旨在解决多模态智能体在处理连续视频流时面临的深度研究难题的系统性框架。研究的核心动机在于发现当前最先进的模型（如 GPT-5, Claude-4.5）在处理视频任务时存在严重的“模态偏见”和“参数知识泄露”，即它们更喜欢搜网页而不是看视频，且容易受预训练记忆干扰。为了打破这一僵局，Video-DR 创新性地提出了感知与探索解耦的流水线，通过阶段性工具解锁机制，强制智能体在获取外部信息前必须完成深度的视觉定位。在技术实现上，Video-DR 采用了 SFT 结合 GRPO 的两阶段训练范式，利用强化学习优化智能体的工具调用逻辑和推理路径。为了验证该框架，作者还推出了 Video-DR-Bench，这是一个包含 200 个高难度、多跳推理任务的基准。实验数据表明，Video-DR-35B-A3B 模型在性能上全面超越了现有的顶级商业模型，平均准确率达到 64.0%。该工作不仅填补了视频深度研究智能体的空白，还为多模态强化学习提供了一个极具参考价值的范式。论文通过详尽的消融实验和工具使用分析，揭示了智能体在跨模态任务中如何通过结构化的流程设计和策略优化来克服固有的行为偏差，为下一代多模态智能体的发展指明了方向。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该工作直接关联智能体（Agent）和生成（Generation）方向，特别是多模态智能体的逻辑推理。

## 基本信息

- 作者：Zhen Fang, Yu Zeng, Wenxuan Huang, Yiming Zhao, Shiting Huang, Tianfei Ren, Qi Lu, Qingnan Ren, Qisheng Su, Lionel Z. Wang, Qingyu Yin, Shuang Chen, Zehui Chen, Lin Chen, Zhenfei Yin, Yao Hu, Shaohui Lin, Wanli Ouyang, Shaosheng Cao, Feng Zhao
- 机构：上海人工智能实验室 (Shanghai AI Lab), 清华大学 (Tsinghua University), 中国科学技术大学 (USTC) [合理推断，基于作者背景及 GitHub 组织名]
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-08-04
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2608.03979v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成深度参考了 PDF 检索证据，特别是关于模型瓶颈的定义、GRPO 的应用细节以及 Video-DR-Bench 的实验数据。
