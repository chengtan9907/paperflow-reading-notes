---
user_id: "cheng tan"
paper_id: 4056
arxiv_id: "2607.13653"
title: "Exploratory, Communicative, and Deployable: Vision-Driven Embodied Agents for Open-World Mobile Manipulation"
publish_date: "2026-07-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.13653.pdf"
pdf_url: "https://arxiv.org/pdf/2607.13653"
abs_url: "https://arxiv.org/abs/2607.13653"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-17T00:59:34"
---
# Exploratory, Communicative, and Deployable: Vision-Driven Embodied Agents for Open-World Mobile Manipulation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：embodied agent · mobile manipulation · vision-driven · world graph

## 一句话总结

本文提出一个视觉驱动的具身智能体框架，结合探索、通信和部署能力，通过世界图表示与两阶段训练，在开放世界移动操作任务上实现从模拟到真实世界的零样本迁移。

## 摘要

> 论文介绍一种用于开放世界移动操作的视觉驱动具身智能体系统。该系统利用世界图作为场景状态表示，支持导航、操作、探索和通信等多种工具；采用监督微调与强化学习（GSPO）两阶段训练策略，在模拟器中训练后可直接部署至真实环境。在7,304次模拟轨迹中达成39.3%成功率，并在60余次真实世界任务中展示零样本迁移能力。分析揭示逻辑推理提升与动作记忆丢失、指令模糊等失败模式。

Q1: 这篇论文试图解决什么问题？

开放世界移动操作任务要求智能体在未知动态环境中执行长期、多步骤操作，同时能够处理模糊指令、自主探索并与人通信。现有方法多数依赖精确环境模型或固定策略，难以零样本迁移到真实世界；缺乏完整的框架同时覆盖探索、通信与部署能力。本文旨在构建一个实际可部署的系统，弥合模拟与真实的差距，在开放世界中不依赖微调直接执行移动操作任务。

Q2: 有哪些相关研究？

相关工作涵盖移动操作（mobile manipulation）、视觉语言导航（VLN）、具身任务规划（task planning）、强化学习与sim-to-real迁移。具体包括：多智能体协作方法（如REMAC）、基于世界模型的方法、利用视觉语言模型进行任务分解的工作等。本文的独特之处在于将探索、通信和部署整合到统一框架中，并强调从模拟到真实的零样本迁移。

Q3: 论文如何解决这个问题？

解决方案包含三部分：1）世界图（World Graph）作为场景状态表示，将每个容器映射到其内的物体实例，提供结构化场景理解；2）工具集API，包括导航（move_to）、操作（pick, place, open, close）、探索（visual_find, show_object_by_category）和通信（ask_human）；3）训练采用两阶段：先通过监督微调（SFT）学习基础策略，再使用GSPO（Generalized Supervised Policy Optimization）强化学习优化长期任务成功率。关键设计是保持模拟与真实的一致性，例如统一的动作接口和世界图更新机制，确保策略可以直接迁移。

Q4: 论文做了哪些实验？

实验在模拟器（可能是ManipulaTHOR或自定义环境）和真实世界进行。模拟阶段：使用7,304条RL rollout轨迹评估，整体训练中成功率达到39.3%。真实世界部署：超过60个episode，覆盖多种任务（如放置物体、打开微波炉等），无需任何微调。此外包含了失败案例分析，以及可能有的消融实验（从证据C.2和D.3推断有详细奖励配置和工具实现细节）。

Q5: 发现了什么实验现象？

1）RL训练后逻辑推理能力显著提升，但出现了动作记忆丢失现象：在长期任务中代理遗忘先前已执行的动作或中间状态。2）面对欠指定指令（如“把东西放好”而没有指定目标位置），代理倾向于立即执行动作而非主动询问，导致错误。3）真实世界演示显示代理能够主动打开微波炉门、导航到面包架等，但仍有因视觉识别误差或工具调用错误导致失败。4）消融实验（推测）可能验证了世界图表示和GSPO训练的必要性。

Q6: 有什么可以进一步探索的点？

1）增强主动通信能力，使代理在指令模糊时主动提问。2）解决长时动作记忆问题，可能引入外部记忆模块或状态记录。3）扩展至更复杂的场景（如多房间、多人协作）。4）探索多智能体协作与任务分配。5）提升视觉感知模块在真实世界的鲁棒性。6）结合大规模预训练模型进行常识推理。

Q7: 总结一下论文的主要内容

本文提出一个名为REAL（推断）的视觉驱动具身智能体框架，针对开放世界移动操作任务，将探索、通信与部署能力统一在一个系统内。核心组件包括世界图（WG）用于结构化场景表示，一组工具API用于环境交互，以及两阶段训练（SFT → GSPO RL）策略。在模拟器中训练后，该框架在真实世界超过60次任务中展现出零样本迁移能力，成功率达到可对比水平（模拟内39.3%）。论文详细分析了训练过程中的成功瓶颈与失败模式，包括逻辑推理提升、动作记忆丢失、模糊指令处理等，为后续研究提供了系统基线。主要贡献：1）提出包含探索、通信与部署的完整框架；2）设计sim-to-real一致的表示与动作接口；3）通过大量模拟与真实实验验证了有效性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在真实世界部署细节与失败案例分析。

## 基本信息

- 作者：Boyu Mi, Mengchen Ma, Yifei Yao, Xing Gao, Junting Chen, Yangzi Li, Zihou Zhu, Guohao Li, Zhenfei Yin, Tai Wang, Yao Mu, Jiangmiao Pang, Hanqing Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.RO
- 日期：2026-07-16
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.13653`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF检索证据（涵盖introduction、method、results、limitations、relevance等片段），部分细节（如REAL框架名）基于推断，整体分析忠实于所提供材料。
