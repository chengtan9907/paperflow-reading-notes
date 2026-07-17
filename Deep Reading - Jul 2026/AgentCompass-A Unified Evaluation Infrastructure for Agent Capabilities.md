---
user_id: "cheng tan"
paper_id: 4250
arxiv_id: "2607.13705"
title: "AgentCompass: A Unified Evaluation Infrastructure for Agent Capabilities"
institution: "Shanghai AI Laboratory"
publish_date: "2026-07-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.13705.pdf"
pdf_url: "https://arxiv.org/pdf/2607.13705"
abs_url: "https://arxiv.org/abs/2607.13705"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-17T01:06:43"
---
# AgentCompass: A Unified Evaluation Infrastructure for Agent Capabilities

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agent evaluation · benchmark infrastructure · modular design · asynchronous runtime

## 一句话总结

AgentCompass 是一个统一的智能体能力评估基础设施，通过模块化组件和异步运行时解决当前评估管道的碎片化与耦合问题，并基于集成基准对多种模型进行了实证分析。

## 摘要

> 随着大型语言模型发展为自主智能体，统一评估基础设施变得关键。本文提出 AgentCompass，它通过将评估分解为 benchmark、harness、environment 等可组合组件，并定义稳定协议来替换高度耦合的脚本。用户通过声明式 RunRequest 指定评估对象与操作选择，运行时采用异步架构高效处理智能体-环境交互的高 I/O 开销。AgentCompass 原生集成了超过 20 个基准，覆盖工具使用、Web 导航、科学推理、智能体编码和生产力五大维度。基于该平台，论文评估了多个模型在编码与生产力任务上的表现，观察到大部分模型遵循 test-time scaling law，但也发现 DeepSeek-V4-Pro 等异常值。

Q1: 这篇论文试图解决什么问题？

当前智能体评估面临高度碎片化和耦合性问题：不同基准拥有各自的评估脚本、运行环境和评分逻辑，导致复现困难、工程冗余，并难以横向比较。现有评估框架多针对语言模型，缺乏对智能体特有的多步交互、工具调用、环境隔离等需求的原生支持。论文旨在构建一个统一的评估基础设施来标准化评估流程，提升可复现性、可扩展性和易用性。

Q2: 有哪些相关研究？

相关评估基础设施包括 OpenCompass（面向语言模型综合评测）、LM Evaluation Harness（标准化评测流程）、BigBench（多任务评测集合）等，但它们主要针对单轮文本生成任务，缺乏对智能体多步交互和环境反馈的原生支持。此外，特定智能体基准如 GAIA、WebArena、SciWorld 等各自独立开发，评估协议不统一。AgentCompass 借鉴了模块化理念和异步设计，但首次面向智能体能力提供统一的声明式评估协议和运行时隔离环境，填补了基础设施空白。

Q3: 论文如何解决这个问题？

AgentCompass 通过三个核心设计解决碎片化问题：1）模块化组件：将评估过程分解为 benchmark（定义任务集）、harness（协调智能体与环境的交互）、environment（提供执行沙箱，支持命令执行、文件操作等），并通过稳定协议连接各组件。2）声明式 RunRequest：用户通过 JSON 格式声明性请求指定评估的实质对象（如哪些基准、哪些模型）和操作选择（如并行度、超时设置），实现评估配置与执行逻辑的解耦。3）异步执行运行时：运行时采用事件驱动架构，高效处理智能体-环境交互中大量的异步 I/O 操作（如 API 调用、环境反馈），支持并行评估和动态资源管理。此外，AgentCompass 内置了 20+ 基准的 harness 实现，并支持用户自定义组件的扩展。

Q4: 论文做了哪些实验？

论文在 AgentCompass 平台上进行了两类实证分析：1）编码能力评估：使用 OpenClaw 框架（集成自动评测）在多个编码基准（如 CodeGeneration、BugFix 等）上评估了包括 GPT-4o、Claude-Opus-4.8、DeepSeek-V4-Pro 等模型，观察输出长度与得分的关系。2）生产力任务评估：在 PinchBench 等基准上评估模型执行生产力任务（如文档处理、表格操作）的性能，记录 token 消耗与任务成功率的权衡。此外，论文还展示了 AgentCompass 对异步执行效率（如并发请求吞吐）的验证，以及跨不同基准的复现性测试。整体实验覆盖了 23 个任务，采用 full suite 设置。

Q5: 发现了什么实验现象？

在编码任务上，大部分模型遵循 test-time scaling law：生成更长输出往往获得更高分数，表明推理时间扩展对编码有帮助。但是 DeepSeek-V4-Pro 出现明显异常，其输出长度增加时得分未提升甚至下降，提示该模型可能存在编码能力上的瓶颈或微调策略不同。在生产力任务上，Claude-Opus-4.8 能以较低 token 预算取得较高得分，表现出高效的任务理解能力；而 Qwen3.5-397B-A17B 在此类任务上相对较弱。这些观察验证了 AgentCompass 作为分析平台的有效性，展示了不同模型在同维度下的对比模式。

Q6: 有什么可以进一步探索的点？

AgentCompass 当前版本主要支持文本交互的智能体，未来可扩展至多模态环境（如视觉工具调用）。基准覆盖方面可引入更多动态交互和开放型基准（如手机操作、机器人控制）。此外，可探索利用 AgentCompass 运行时数据自动生成诊断报告和失败分析。另一个方向是将评估结果反馈到模型训练中，构建评估驱动的优化循环。最后，环境隔离的进一步优化（如容器化、安全沙箱）也是提升实用性的关键。

Q7: 总结一下论文的主要内容

论文《AgentCompass: A Unified Evaluation Infrastructure for Agent Capabilities》针对大型语言模型作为智能体时评估基础设施碎片化、难以复现的问题，提出了统一的评估平台 AgentCompass。首先，论文分析了当前评估管道的高耦合性、基准分散以及缺乏统一运行环境等痛点。然后，详细介绍了 AgentCompass 的设计：包括声明式 RunRequest 解耦评估对象与操作、模块化组件（Benchmark、Harness、Environment）通过标准化协议组合、异步运行时高效处理多轮交互。AgentCompass 原生集成了 20+ 基准，涵盖工具使用、Web 与研究、科学推理、智能体编码和生产力五大维度。基于该平台，论文在编码和生产力任务上对多个主流模型（如 GPT-4o、Claude-Opus-4.8、DeepSeek-V4-Pro、Qwen 等）进行了评估，观察到大部分模型遵循 test-time scaling law，但同时发现异常值。实验还展示了 AgentCompass 在效率、复现性和可扩展性方面的优势。最后，论文讨论了基础设施的局限性及未来发展方向。总体而言，AgentCompass 为智能体评估提供了一个稳健、标准化的平台，有助于推动智能体研究的良性发展。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：AgentCompass 原生集成了超过 20 个智能体基准，覆盖工具使用、Web 与研究、科学推理、智能体编码和生产力，与用户关注的智能体方向直接相关。

## 基本信息

- 作者：Zichen Ding, Jiaye Ge, Shufan Jiang, Kai Chen, Mo Li, Qingqiu Li, Zehao Li, Zonglin Li, Tiaohao Liang, Shudong Liu, Zerun Ma, Zixing Shang, Wenhui Tian, Zun Wang, Liwei Wu, Zhenyu Wu, Jun Xu, Bowen Yang, Dingbo Yuan, Qi Zhang, Songyang Zhang, Peiheng Zhou, Dongsheng Zhu
- 机构：Shanghai AI Laboratory
- 来源：arxiv
- 主题/分类：cs.AI, cs.SE
- 日期：2026-07-16
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.13705`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要基于 PDF 检索命中的证据片段（来自 section 3、4 和附录），并结合 heuristic_draft 进行润色与补充。部分字段如 related_work、future_directions 基于论文整体内容进行合理推断。
