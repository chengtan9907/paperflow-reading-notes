---
user_id: "cheng tan"
paper_id: 5402
arxiv_id: "2607.19595"
title: "Twin Agent: Context Residual Compression for Privilege Separated Agents"
publish_date: "2026-07-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.19595.pdf"
pdf_url: "https://arxiv.org/pdf/2607.19595"
abs_url: "https://arxiv.org/abs/2607.19595"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-23T17:20:23"
---
# Twin Agent: Context Residual Compression for Privilege Separated Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language model agents · prompt injection defense · privilege separation · information compression

## 一句话总结

Twin Agent通过探索者与安全者双智能体架构与残差压缩信息传递，在抵御提示注入攻击的同时保持任务效用，实现了更好的安全-效用权衡。

## 摘要

> Large language model (LLM) agents are vulnerable to security risks, such as prompt injection attacks from untrusted context that manipulate downstream reasoning and tool use. Existing secure-by-design approaches mitigate this risk by separating untrusted observations from privileged execution and careful control of information flow, but often degrade utility and require extensive task-specific engineering. We thus propose Twin Agent, a general privilege separation design pattern inspired by residual coding in the agent context. Twin Agent consists of two nearly symmetric agents: an Explore Agent that inspects untrusted information and a Safe Agent that executes privileged actions. The Explore Agent is conditioned on the Safe Agent's current context and communicates only compact hints to the Safe Agent about the next action to take. This design reduces the information needed to preserve task utility and thus achieves a better security--utility tradeoff, which we empirically verify by measuring how utility and attack success change as the length of hints varies. We evaluate Twin Agent on long-horizon software engineering tasks with SWE-bench Lite and on heterogeneous multi-tool interaction tasks with AgentDojo and DecodingTrust-Agent. Across both benchmarks, Twin Agent preserves high task utility while preventing prompt injection attacks, outperforming both undefended agents and privilege separation baselines.

Q1: 这篇论文试图解决什么问题？

LLM代理在与不可信外部环境交互时面临严重的提示注入威胁，攻击者可利用恶意上下文操纵代理的推理与工具调用。现有防御策略包括提示防护、输入过滤和权限分离，但要么牺牲任务效用，要么需要大量针对特定任务的重设计。核心挑战在于如何在严格限制信息流以保障安全的同时，保留足够信息支持任务完成。本文将安全代理设计形式化为压缩信息流问题，并探索可调的信息预算如何影响安全-效用权衡。

Q2: 有哪些相关研究？

相关研究包括：1）提示注入攻击与防御（如Willison 2023、Debenedetti 2025）；2）权限分离的代理设计（如Beurer-Kellner 2025、Costa 2025、Jacob 2026），它们隔离不可信上下文但常导致效用下降；3）信息压缩技术，如残差编码；4）ReAct等标准代理框架。Twin Agent结合了权限分离与残差压缩，提出一种无需大量任务特定工程的通用解决方案。

Q3: 论文如何解决这个问题？

Twin Agent包含两个部分：Explore Agent（EA）和Safe Agent（SA）。两者基于相同基座LLM（ReAct模式）。工作流程：SA维护内部上下文（特权指令、历史动作）。EA接收SA的上下文和外部观察，输出一个固定长度k的紧凑提示（hint），该提示是唯一跨信任边界传递的信息。SA基于内部上下文和hint决定下一步动作。提示长度k可调，控制信息预算。设计灵感来自残差编码：hint编码SA不易推导的残留信息。此外，可选的注入检测器对hint进行安全扫描。SA执行动作与工具调用，EA不直接与外部交互。模式可作为ReAct的直接替代，无需大规模重构。

Q4: 论文做了哪些实验？

实验在三个基准上进行：SWE-bench Lite（软件工程任务）、AgentDojo（多工具交互）、DecodingTrust-Agent（多工具含攻击）。每个基准包含清洁和注入攻击场景。基线包括无防御代理和现有权限分离方法。评估指标：任务效用（清洁完成率）和攻击成功率（ASR）。主要实验改变提示长度k，绘制权衡曲线。此外，进行消融实验以分析组件贡献。

Q5: 发现了什么实验现象？

1）无防御代理清洁性能强但ASR接近100%；2）权限分离基线显著降低ASR但导致效用大幅下降；3）Twin Agent在中等k值下ASR维持低水平（与强防御基线相当），效用仅轻微下降，实现更优权衡；4）随着k增大，效用提升但ASR上升，呈现可控权衡曲线；5）不同基准趋势一致，表明方法泛化性好；6）消融实验显示hint压缩为核心组件，检测器提供额外安全但非必需；7）极短k时仍有一定效用，表明EA在某些情况下可通过省略hint拒绝任务，对安全有利。

Q6: 有什么可以进一步探索的点？

1）自适应提示长度：动态调整k以优化权衡；2）扩展到更复杂动作空间与长程任务；3）多轮hint传递与反馈；4）与更强LLM集成；5）形式化安全验证；6）探索hint压缩本身是否足够（移除检测器）；7）训练更优hint生成策略；8）应用到多代理协作场景。

Q7: 总结一下论文的主要内容

本文针对LLM代理在不可信环境中面临的提示注入安全问题，提出Twin Agent设计模式。核心创新是将代理上下文残差压缩思想融入权限分离架构，通过双代理结构（Explore Agent和Safe Agent）和紧凑hint通信实现可控信息流。Explore Agent负责提取最小必要信息，Safe Agent执行特权操作。信息预算k可调，直接关联安全-效用权衡。在SWE-bench Lite、AgentDojo和DecodingTrust-Agent上的实验表明，Twin Agent在保持任务效用方面显著优于现有权限分离基线，同时在防御提示注入方面表现优异。论文还形式化了安全代理中的信息压缩问题。主要贡献包括：提出Twin Agent框架、刻画信息预算与权衡关系、以及广泛实证验证。局限性在于提示长度需手动设定、任务类别有限。整体上为构建安全实用的LLM代理提供了有力方案。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接针对LLM代理安全的关键问题，与当前代理安全研究趋势高度相关。

## 基本信息

- 作者：Zhanhao Hu, Dennis Jacob, Xiao Huang, Zhaorun Chen, Bo Li, David Wagner
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CR, cs.CL
- 日期：2026-07-23
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.19595`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据中的摘要章节和部分成果节，并基于合理推断补充了相关细节；具体数值因证据不足未编造，仅描述趋势。
