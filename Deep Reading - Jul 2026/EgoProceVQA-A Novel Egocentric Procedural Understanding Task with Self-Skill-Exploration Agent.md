---
user_id: "cheng tan"
paper_id: 4278
arxiv_id: "2607.13792"
title: "EgoProceVQA: A Novel Egocentric Procedural Understanding Task with Self-Skill-Exploration Agent"
publish_date: "2026-07-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.13792.pdf"
pdf_url: "https://arxiv.org/pdf/2607.13792"
abs_url: "https://arxiv.org/abs/2607.13792"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-17T01:07:22"
---
# EgoProceVQA: A Novel Egocentric Procedural Understanding Task with Self-Skill-Exploration Agent

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：egocentric vision · procedural understanding · video question answering · multimodal large language model

## 一句话总结

提出EgoProceVQA任务、EgoProceGen数据生成平台和EgoProceAgent自技能探索代理框架，系统评估和提升第一人称视频程序性理解能力，并在多个任务上取得开源模型最佳性能。

## 摘要

> Most daily activities are inherently procedural. However, existing evaluations for egocentric video understanding seldom address procedural understanding and largely overlook complex key-step-level reasoning under the widely used video question answering (VQA) paradigm for MLLMs. Such capabilities are crucial for building procedural AI assistants deployable on wearable devices. To bridge this gap, we introduce the Egocentric Procedural Understanding VQA task (EgoProceVQA), which systematically evaluates egocentric procedural reasoning abilities of current MLLMs and agents through six types of key-step-centric questions. Furthermore, we develop EgoProceGen, a data generation platform that efficiently constructs QA data tailored to different question types. Based on this platform, we build a benchmark with 3,600 questions, four common procedural scenarios, and 31 everyday procedural tasks. Evaluations on EgoProceVQA show that existing MLLMs and agents still have substantial room for improvement in procedural understanding. Therefore, we further propose EgoProceAgent, a self-skill-exploration agentic framework. We design a generic tool library for procedural understanding and a standardized sub-skill library shared across tools and models, enabling self-exploration without ground-truth supervision. By exploring how to compose and select sub-skills, the agent discovers effective skill strategies for diverse problems, and attains state-of-the-art performance among open-source models on multiple tasks. Together, our benchmark, generation platform, and agentic framework establish a unified foundation for EgoProceVQA. Project page: https://z1oong.github.io/EgoProceVQA/.
> Keywords
> Egocentric vision, procedural understanding, agent skill, self-evolution
> ACM Reference Format:
> Junlong Li $^{*}$ , Junxi Li $^{*}$ , Yuxiang Yang, Wenbin Zou, Lap-Pui Chau, and Yi Wang $^{\dagger}$ . 2026. EgoProceVQA: A Novel Egocentric Procedural Understanding Task with Self-Skill-Exploration Agent. In Proceedings of the 34th ACM International Conference on Multimedia (MM'26). ACM, New York, NY, USA, 20 pages. https://doi.org/10.1145/nnnnnnn.nnnnnnn
> Most daily activities are inherently procedural. However, existing evaluations for egocentric video understanding seldom address procedural understanding and largely overlook complex key-step-level reasoning under the widely used video question answering (VQA) paradigm for MLLMs. Such capabilities are crucial for building procedural AI assistants deployable on wearable devices. To bridge this gap, we introduce the Egocentric Procedural Understanding VQA task (EgoProceVQA), which systematically evaluates egocentric procedural reasoning abilities of current MLLMs and agents through six types of key-step-centric questions. Furthermore, we develop EgoProceGen, a data generation platform that efficiently constructs QA data tailored to different question types. Based on this platform, we build a benchmark with 3,600 questions, four common procedural scenarios, and 31 everyday procedural tasks. Evaluations on EgoProceVQA show that existing MLLMs and agents still have substantial room for improvement in procedural understanding. Therefore, we further propose EgoProceAgent, a self-skill-exploration agentic framework. We design a generic tool library for procedural understanding and a standardized sub-skill library shared across tools and models, enabling self-exploration without ground-truth supervision. By exploring how to compose and select sub-skills, the agent discovers effective skill strategies for diverse problems, and attains state-of-the-art performance among open-source models on multiple tasks. Together, our benchmark, generation platform, and agentic framework establish a unified foundation for EgoProceVQA. Project page: https://z1oong.github.io/EgoProceVQA/.

Q1: 这篇论文试图解决什么问题？

现有第一人称视频理解评估多聚焦于动作识别、对象交互等，极少关注程序性推理，即理解任务中的关键步骤及其顺序。在视频问答（VQA）范式下，多模态大语言模型（MLLM）通常无法进行步骤级别的细致推理，限制了程序性AI助手的开发。本文旨在填补这一空白，提出系统评估方法并构建能自我提升的代理框架。

Q2: 有哪些相关研究？

相关研究包括第一人称视频基准（如EgoTaskQA、EPIC-Kitchens等），但它们未专门设计程序性推理问题；MLLM在视频VQA中虽取得进展，但在步骤级推理上表现不足；现有代理框架如AutoGPT、ReAct等将工具调用固定化，缺乏针对程序性理解的适应性技能探索。本文综合这些方面提出评估和提升方案。

Q3: 论文如何解决这个问题？

本文首先定义EgoProceVQA任务，包括六类关键步骤问题（如步骤识别、顺序排序、时间定位等）。开发EgoProceGen数据生成平台，通过任务定义、步骤分解、问题模板和答案生成等模块自动化构建QA数据，支持可扩展和场景迁移。基于此构建含3600个问题的基准。针对现有模型不足，提出EgoProceAgent框架：设计通用工具库（如视频裁剪、字幕生成等）和标准化子技能库，通过四阶段自探索（包括子技能组合、技能链优化、策略验证、迭代更新）在不依赖真实反馈的情况下发现不同问题类型的最佳技能策略，最终实现性能提升。

Q4: 论文做了哪些实验？

论文在EgoProceVQA基准上评估了多种MLLM，包括Qwen2.5-VL、InternVL2等，对比零样本和微调设定。主要实验包括：1) 基准测试模型在六类问题上的准确率；2) 消融研究输入帧数（8帧 vs 全帧）和思维链（CoT）步骤深度（1-3级）的影响；3) 评估EgoProceAgent框架在Qwen2.5-VL-7B上的效果，并与同等规模开源模型对比；4) 分析不同子类型的技能策略有效性。

Q5: 发现了什么实验现象？

实验发现：1) 现有MLLM在程序性理解上普遍较低，尤其需要步骤时序推理的任务表现差；2) 增加CoT步骤和输入上下文长度并不一致提升性能，有时甚至下降，表明模型缺乏稳健步骤推理；3) EgoProceAgent在多数任务上显著超越基线，其中过程步骤识别（PSR）任务相对提升84.3%（8帧输入）；4) 时序推理（TC）任务未见提升，推测为该任务难度高且框架设计偏向识别类；5) 不同子类型需要不同技能策略，自探索能自动适配。

Q6: 有什么可以进一步探索的点？

1) 扩展EgoProceVQA到更多日常和工业程序场景；2) 探索更复杂的技能组合和长的依赖链；3) 将框架部署到真实可穿戴设备验证实用性；4) 结合更强的基础模型提升上限；5) 研究技能策略的跨任务迁移和持续学习；6) 解决时序推理等困难任务。

Q7: 总结一下论文的主要内容

本文提出EgoProceVQA任务，系统评估第一人称视频的程序性理解能力。开发EgoProceGen平台自动化构建QA数据，建立3600问题的基准。评估发现现有MLLM和代理存在明显不足。为此设计EgoProceAgent框架，通过预定义工具库和标准化子技能库，采用四阶段自探索机制发现有效策略，无需真实反馈。实验表明EgoProceAgent在Qwen2.5-VL-7B上达到开源模型最佳，PSR任务提升84.3%。本文贡献了新任务、数据平台和自探索代理框架，为程序性AI助手奠定基础。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：涉及智能体自主探索技能，与用户方向中的agent高度相关。

## 基本信息

- 作者：Junlong Li, Junxi Li, Yuxiang Yang, Wenbin Zou, Lap-Pui Chau, Yi Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-16
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.13792`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，包括摘要、方法、实验等片段。
