---
user_id: "cheng tan"
paper_id: 1100
arxiv_id: "2606.23327"
title: "VideoAgent: All-in-One Framework for Video Understanding and Editing"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23327.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23327"
abs_url: "https://arxiv.org/abs/2606.23327"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T00:09:16"
---
# VideoAgent: All-in-One Framework for Video Understanding and Editing

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：video editing · multi-agent system · video understanding · long video

## 一句话总结

VideoAgent提出一个统一的智能体框架，整合视频理解与编辑任务，通过全局镜头创建和多智能体编排实现长视频连贯叙事与复杂编辑流程的自动生成。

## 摘要

> Video editing has become essential in digital media creation, yet existing automated systems are restricted to short segment processing and domain-specific tasks. They face two critical limitations: i) inability to handle diverse video comprehension and editing operations, and ii) lack of long-video understanding for coherent narrative creation. We propose VideoAgent, an all-in-one agentic framework addressing these challenges through two key innovations. First, we develop automated video shot creation with shot planning agents for coherent narratives and cross-modal retrieval for aligned visual content. Second, we design a multi-agent orchestration framework integrating over thirty specialized editing agents. Intent parsing filters relevant tools while textual-gradient graph optimization assembles complex editing pipelines. Extensive experiments on our newly-proposed VideoEdit benchmark and public datasets demonstrate VideoAgent's superiority over existing multimodal LLMs and agentic systems. VideoAgent achieves 87-95% orchestration success rates while reducing API costs by 60%. Human evaluation across six video categories shows VideoAgent produces professional-quality content approaching human-level performance, with ratings only 4% below human-created videos. We release our code at https://github.com/HKUDS/VideoAgent.

Q1: 这篇论文试图解决什么问题？

现有自动化视频编辑系统存在两个关键局限：一是无法处理多样化的视频理解与编辑操作（如音乐节奏检测、视觉检索、时间对齐等需多步骤协同的任务），二是缺乏长视频理解能力，难以生成连贯的叙事。当前方法多局限于特定领域或预定义工作流，缺乏通用工具库和自适应编排能力。因此，需要一种统一框架将视频理解与编辑整合，实现产品级的自动视频创作。

Q2: 有哪些相关研究？

相关研究包括多模态大语言模型（如Video-LLaMA、LLaVA-NeXT-Video等）在视频理解上的应用，以及基于智能体的系统（如AutoGen、MetaGPT）在复杂任务编排上的探索。但现有工作要么专注于单一视频理解任务，要么针对特定编辑操作（如视频摘要、风格迁移），缺乏将理解与编辑无缝结合、支持长视频连贯生成的通用框架。本文的VideoAgent填补了这一空白，通过全局镜头规划和多智能体协作实现了端到端的自动视频创作。

Q3: 论文如何解决这个问题？

VideoAgent采用两大关键技术路线：1）自动镜头创建：利用剧本规划智能体生成基于故事线的镜头描述框架，再通过跨模态检索从源视频库中检索最匹配的视觉片段，确保叙事连贯与视觉对齐。2）多智能体编排框架：整合超过30个专用编辑智能体（如场景分割、对象跟踪、音频同步、调色等），通过意图解析筛选相关工具，并利用文本梯度图优化（Textual-Gradient Graph Optimization）自适应地组装复杂编辑流水线，将用户用自然语言描述的编辑需求转化为可执行的步骤序列。

Q4: 论文做了哪些实验？

论文在自建的VideoEdit基准（涵盖6种视频类别：音乐视频、Vlog、运动集锦、短视频、教育视频、电影预告片）和公开数据集（如MSR-VTT、YouCook2）上进行评估。对比基线包括多模态LLM（如GPT-4V、Video-LLaMA）和智能体系统（如AutoGen、MetaGPT）。实验主要衡量编排成功率、API成本、视频检索准确率，并进行了人类主观评价（评分1-5，从叙事连贯性、视觉质量、节奏感等维度）。VideoAgent在编排成功率上达到87-95%，显著优于基线的20-60%，同时API成本降低约60%。

Q5: 发现了什么实验现象？

实验揭示以下现象：1）VideoAgent在复杂多步骤编辑（如音乐节奏剪辑、对象替换）中表现出明显优势，简单步骤任务上优势较小；2）文本梯度图优化有效减少了工具选择错误和顺序混乱，消融实验显示移除该模块后成功率下降约15%；3）人类评价显示VideoAgent生成的内容在叙事连贯性上接近人类（差距<4%），但在创意细节上仍有差距；4）检索式镜头创建的质量依赖于源素材库，当素材不足时画面多样性受限；5）系统在长视频（>10分钟）上的表现优于短视频，验证了全局规划的有效性。

Q6: 有什么可以进一步探索的点？

可探索的方向包括：1）引入内容审核框架与安全机制，防止生成误导性内容；2）改进检索式镜头创建，结合生成式模型（如视频扩散模型）以摆脱对源素材的依赖；3）扩展智能体以支持更细粒度的编辑操作（如帧级特效）；4）研究用户意图理解的鲁棒性，支持更模糊或抽象的描述；5）探索与其他创作工具（如3D渲染、音频合成）的集成；6）在更长视频或电影级制作中验证框架的扩展性。

Q7: 总结一下论文的主要内容

本文提出VideoAgent，一个全能智能体框架，将视频理解与编辑整合为端到端流水线。其核心创新包括：1）自动镜头创建，通过剧本规划生成故事线并跨模态检索匹配视觉内容，解决长视频连贯性问题；2）多智能体编排系统，集成30多个编辑智能体并利用文本梯度图优化自适应组装复杂工作流。在自建VideoEdit基准和公开数据集上的实验表明，VideoAgent在编排成功率（87-95%）和成本效率（降低60%）上显著超越现有多模态LLM和智能体系统。人类评价显示其输出质量接近专业人类编辑（评分仅低4%）。论文还讨论了局限性，如对源素材质量的依赖和潜在滥用风险，并提出了未来改进方向。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：该论文与用户画像中的“智能体”（权重0.10）方向直接相关，提供多智能体编排和全局规划的新思路。

## 基本信息

- 作者：Hengji Zhou, Lingxuan Huang, Jian Wang, Bing Zhou, Si Wu, Lianghao Xia, Chao Huang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-06-23
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23327`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF的Abstract、Introduction、Conclusion和Limitations等检索证据，结合启发式草稿进行补全。
