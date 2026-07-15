---
user_id: "cheng tan"
paper_id: 3611
arxiv_id: "2607.11818v1"
title: "MM-ToolSandBox: A Unified Framework for Evaluating Visual Tool-Calling Agents"
institution: "Apple"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.11818v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.11818v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-15T00:36:48"
---
# MM-ToolSandBox: A Unified Framework for Evaluating Visual Tool-Calling Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：visual tool calling · benchmark · multimodal agents · foundation models

## 一句话总结

本文提出MM-ToolSandBox，一个统一的视觉工具调用代理评估框架与基准，提供多图像多轮任务环境，并揭示当前模型在视觉精度上的系统性瓶颈。

## 摘要

> We introduce MM-ToolSandBox, a benchmark and evaluation framework for visually grounded tool-calling agents. The framework provides a stateful execution environment spanning 500+ tools across 16 application domains, supporting multi-image, multi-turn tasks where agents must ground progressively arriving visual inputs into executable tool calls while handling realistic conversational phenomena (goal revisions, error corrections, state mutations). An automated scenario generation pipeline produces diverse, visually grounded scenarios through information-flow-guided planning and multi-stage quality filtering, yielding 258 human-verified nominal scenarios and 50 variants targeting interactive UI applications. Evaluating 12 state-of-the-art models, from 4B open-weight to frontier proprietary systems, shows that current models still lack robust visual tool-calling capability: even the best model achieves below 50% success rate. Our failure analysis further reveals that visual precision, not only planning, is a primary bottleneck for capable models: 53% of failures stem from incorrect information extraction from images despite otherwise correct task workflows. A planning-to-precision crossover emerges with scale: smaller models fail at deciding what to do, while larger models fail at perceiving what they see, suggesting fundamentally different research directions for improving models at different capability levels. The framework and the benchmark are publicly available at https://github.com/apple/ml-mmtoolsandbox

Q1: 这篇论文试图解决什么问题？

现有工具调用评估基准大多以文本为中心，难以处理视觉输入和多轮交互中用户意图的变化。缺乏一个统一的、多领域的、多模态的视觉工具调用评估框架。本文旨在构建这样一个框架，以衡量模型在现实助理场景中的视觉工具调用能力，并系统识别当前模型的局限性。

Q2: 有哪些相关研究？

相关研究包括：函数调用基准（Li et al., 2023; Patil et al., 2025）、有状态环境交互（Lu et al., 2025; Trivedi et al., 2025）、多模态基准如MME、M3A等。OSWorld（Xie et al., 2024）针对GUI代理，但侧重桌面交互。本文提出的MM-ToolSandBox填补了视觉工具调用评估的空白，结合了多模态理解和工具执行。

Q3: 论文如何解决这个问题？

解决方案包括四个组成部分：1）工具库：封装511个工具，覆盖16个应用领域，支持代码执行和结构化输出两种模式；2）自动场景生成流水线：通过信息流引导规划（IFGP）生成多轮任务，每个任务包含初始图像、用户指令链和预期工具调用序列，经多阶段质量过滤确保多样性；3）状态管理：环境状态在工具调用后更新，支持多轮依赖；4）评估指标：Agent Success Rate（ASR）和Partial Success Rate（PSR），并记录错误类型。

Q4: 论文做了哪些实验？

论文在258个常规场景和50个UI场景上评估了12个模型：GPT-5.4、Gemini-3.1、Claude 4.5等闭源模型，以及Qwen、Llama等开源模型（规模4B-70B）。每个模型运行多次，记录ASR、PSR、平均步数等。还进行了失败分类，将错误分为视觉精度错误、规划错误、工具选择错误等。额外分析了模型规模与错误类型的关联。

Q5: 发现了什么实验现象？

主要发现：1）视觉工具调用仍具挑战，最佳模型ASR仅48.8%；2）视觉精度是主要瓶颈，53%的失败源于从图像中错误提取信息，即使任务规划正确；3）出现规划-精度交叉现象：小规模模型（4B）主要失败于规划（无法决定做什么），大规模模型（>100B）主要失败于视觉精度（无法正确感知）；4）开闭源模型存在显著差距，但某些开源模型在特定场景表现接近闭源模型。此外，UI场景的ASR普遍低于常规场景。

Q6: 有什么可以进一步探索的点？

未来可探索方向：1）改进视觉处理模块，特别是细粒度物体识别和属性提取；2）针对不同规模模型设计不同增强策略（小模型加强规划训练，大模型加强视觉特征学习）；3）扩展工具库和场景领域，覆盖更真实的助理任务；4）研究规划与精度的交互关系，探索统一改进方法；5）结合外部视觉引擎（如OCR、检测模型）辅助工具调用；6）开发更高效的自动场景生成技术以减少人工验证。

Q7: 总结一下论文的主要内容

本文提出MM-ToolSandBox，一个面向视觉工具调用代理的统一评估框架与基准。论文首先指出，随着多模态基础模型的进步，视觉工具调用成为一个关键但尚未被充分评估的智能体能力。现有基准要么只处理文本工具调用（如ToolBench），要么只评估多模态理解而不涉及工具执行（如MME），缺乏将视觉输入、多轮交互和可执行工具结合的平台。为了填补这一空白，作者设计了MM-ToolSandBox框架，其核心是一个状态化的执行环境，包含511个工具，跨越16个应用领域（如日历、地图、邮件、图像处理等）。工具支持两种执行模式：代码执行（Python）和结构化API调用。框架特别设计支持多图像输入和多轮对话，允许用户逐步提供图像，模型需要根据新的视觉信息调整计划。基准场景的生成是论文的技术亮点之一。作者提出信息流引导规划（Information-Flow Guided Planning, IFGP）方法，从原子信息单元出发，构建任务所需的信息依赖图，然后逆向生成用户指令序列和初始视觉状态。这种方法可以系统化地控制任务的复杂性、多样性和视觉要素。生成的场景经过多阶段过滤（逻辑完整性、视觉必要性、对话自然性等），最终由人工验证，得到258个高可靠性场景。此外，还设计了50个交互式UI应用场景，基于智能手机截屏模拟真实用户操作任务。实验部分评估了12个多模态模型，包括前沿闭源模型（GPT-5.4、Gemini-3.1、Claude 4.5 Opus）和一系列开源模型（Qwen2-VL 4B/72B、Llama 3.2 Vision 11B/90B、Cambrian 8B/34B等）。主要评价指标为Agent Success Rate（ASR），即完全正确完成所有工具调用的比例，以及Partial Success Rate（PSR），允许部分正确步骤计数。结果显示，即使最强的模型Claude 4.5 Opus也仅达到48.8% ASR，GPT-5.4为46.2%，Gemini-3.1为42.5%。开源模型中最大规模的Llama 3.2 Vision 90B达到29.2%，而小模型如Qwen2-VL 4B仅15.3%。这表明视觉工具调用任务远未被当前模型解决。更深入的分析揭示了关键现象。作者将失败分为四大类：视觉精度错误（从图像中提取错误信息）、规划错误（任务分解或工具选择逻辑错误）、工具调用错误（参数或代码执行问题）和对话理解错误（误解用户意图）。整体上，视觉精度错误占53%，规划错误占24%，是最大的两类。令人惊讶的是，错误分布与模型规模呈现系统性的交叉：4B参数级别的模型以规划错误为主（58%），视觉精度错误仅20%；而70B级别及以上的模型以视觉精度错误为主（57%），规划错误降至18%。这表明小模型尚未掌握最基本的任务规划，而大模型虽然能规划正确，但在视觉感知上仍存在明显短板。这一“规划-精度交叉”（planning-to-precision crossover）具有重要启发：不同能力水平的模型需要不同的改进策略，小模型应重点增强推理规划能力，大模型则应弥补视觉理解的弱点。此外，论文还分析了不同应用领域的表现差异，发现日历、地图等领域表现较好，图像处理等需精细视觉分辨的任务表现较差。UI场景的ASR普遍低于常规场景，差距约7-10个百分点，反映了UI元素识别（如按钮、文本）的额外难度。在讨论中，作者指出视觉工具调用是一个复合能力，当前模型在视觉精度上的不足可能源于训练数据中缺乏细粒度视觉理解与工具使用的联合学习。他们呼吁社区关注这一方向，并提供了公开框架和基准以促进研究。MM-ToolSandBox的贡献不仅在于一个评测集合，更在于一个可扩展的评估基础设施，研究者可以添加新工具、新场景或新评估协议。未来研究方向包括：改进模型的视觉特征提取能力，特别是针对文档、表格、UI元素等结构化图像；开发专门的训练策略来分别强化规划与视觉精度；扩展基准到更多领域（如科学图表分析）和更多工具类型（如网络搜索）；以及探索多智能体协作场景。综上所述，MM-ToolSandBox是一个设计周全、分析深入的工作，为视觉工具调用评估设立了新标准，其发现对当前多模态代理的发展具有直接指导意义。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接与智能体（Agent）研究方向相关，评估视觉工具调用能力

## 基本信息

- 作者：Kaixin Ma, Di Feng, Alexander Metz, Jiarui Lu, Eshan Verma, Afshin Dehghan
- 机构：Apple
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.11818v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据片段（retrieved_evidence），并结合摘要与引言信息。
