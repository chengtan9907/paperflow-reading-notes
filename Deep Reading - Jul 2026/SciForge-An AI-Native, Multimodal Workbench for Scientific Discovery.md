---
user_id: "cheng tan"
paper_id: 4513
arxiv_id: "2607.16038"
title: "SciForge: An AI-Native, Multimodal Workbench for Scientific Discovery"
institution: "Shanghai Artificial Intelligence Laboratory (上海人工智能实验室)"
publish_date: "2026-07-20"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.16038.pdf"
pdf_url: "https://arxiv.org/pdf/2607.16038"
abs_url: "https://arxiv.org/abs/2607.16038"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-21T01:43:10"
---
# SciForge: An AI-Native, Multimodal Workbench for Scientific Discovery

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：scientific discovery · AI workbench · multimodal · agent runtime

## 一句话总结

SciForge 是一个面向科学发现的 AI 原生多模态工作台，通过集成搜索、解析、模型路由、工作流执行和证据审计等功能，将图形界面留给人类判断，而自动化服务支持目标导向的研究。

## 摘要

> Scientific work increasingly spans heterogeneous artifacts -- papers, code, datasets, scientific file formats, model outputs, figures, manuscripts, and team decisions -- yet general-purpose AI assistants rarely preserve these objects as a coherent, auditable research state. We present SciForge, a multimodal research-native AI workbench that reserves the graphical interface for human judgment while search, parsing, model routing, workflow execution, plotting, writing, and presentation generation run as modular agent-accessible services. SciForge is built around five pillars: (i) \emph{goal-scoped scientific decision governance} for \textbf{goal-oriented} research, with review gates and shared review surfaces; (ii) \emph{translate-then-reason} for \textbf{multimodal} input, routing scientific objects through domain translators before the agent reasons; (iii) \emph{evidence governance} for \textbf{auditable} traceability, linking claims to provenance chains and audit findings; (iv) \emph{collaborative team science} for \textbf{collaborative} research, enabling multi-role decision governance, with shared team workspaces planned for future releases; and (v) \emph{real-world application scenarios} for \textbf{practical} impact, demonstrated through eight end-to-end user cases, with flagship demonstrations including multi-day agentic research sprints for gene discovery, AI-guided de novo protein design, molecular optimization, and genome-to-BGC discovery. The system combines a thin interaction layer, contextual research capability patterns, an Agent Runtime and Workflow Engine, an Evidence-DAG audit sidecar and a Scientific Model Router. SciForge currently runs as a desktop application, with mobile supervision support; future releases will deepen team collaboration. The system is open-source and available at https://github.com/AGI4Sci/SciForge

Q1: 这篇论文试图解决什么问题？

现代科学研究工作流高度分散，涉及论文、代码、数据集、科学专用文件格式（如 FASTA、PDB、VCF）、模型输出、图表、手稿以及团队决策等多种异构工件。通用 AI 助手（如会话式聊天机器人）难以将这些工件保持为连贯、可审计且可延续的研究状态，主要原因包括：(1) 会话范围有限，无法维护跨工具、跨轮次的研究目标；(2) 缺乏对科学专用文件的原生处理能力，无法直接“提示”这些格式；(3) 结果的可追溯性和证据链条薄弱，不支持关闭循环式的审计；(4) 缺乏对多用户、多角色协作决策的内置支持；(5) 现有系统或工具通常只覆盖生命周期中的单一环节，如仅论文撰写、仅代码执行或仅文献检索，无法提供从目标设定到最终成果的全流程统一工作台。因此，需要一个研究原生的 AI 工台，将人类的判断与自动化的代理工作流结合起来，确保科学研究中的每个步骤都能够被记录、审计和协作。

Q2: 有哪些相关研究？

论文将相关系统分为三大类别：
1. GUI 基础的科学工作台（GUI-based Scientific Workbenches）: 包括桌面和 Web 应用，使 AI 辅助通过人类面向的工作区可访问。最接近的商业参考是 Claude Science，被描述为用于复杂科学任务的 AI 工作台。这些系统提供图形界面，但通常在执行、模型治理、证据质量、研究连续性和目标导向设计方面存在不足。
2. 文献中心代理（Literature-centered Agents）: 如 FutureHouse，提供面向文献搜索、深度综述、先例检索和化学规划的代理，通过 Web/API 平台开放。这类代理建立了自动科学的检索与综合层，但缺乏对实验执行和数据处理的集成。
3. AI 科学家和研究代理（AI Scientists and Research Agents）: 强调端到端的科学发现，如自动生成假设、设计实验、解读结果。代表性工作包括一些将 LLM 与实验结合的系统，但它们往往专注于特定环节，难以提供覆盖全生命周期的统一工作台。
SciForge 的设计目标是统一以上三个类别，在单一工作台中整合交互 GUI、文献代理和全流程自动发现，并在五个关键维度（执行、模型治理、证据质量、研究连续性、目标导向研究设计）上与其他系统进行对比。

Q3: 论文如何解决这个问题？

SciForge 通过以下五大支柱和系统架构解决上述问题：
1. 目标范围科学决策治理（Goal-scoped Scientific Decision Governance）: 支持目标导向研究，通过审查关卡（review gates）和共享审查界面（shared review surfaces）确保研究沿着可审计的路径推进。系统记录角色归属的审查项（ReviewItems），并通过候选/认证发布关卡（candidate/certified release gates）推进工件，带有人类 PI 可追溯的监督。
2. 翻译然后推理（Translate-then-Reason）: 针对多模态输入，在代理进行推理之前，先将科学对象通过领域翻译器（domain translators）转换为代理可理解的表示。这解决了原生科学文件格式（如 FASTA、PDB、SMILES）不能直接作为 LLM 提示的问题。
3. 证据治理（Evidence Governance）: 实现可审计的可追溯性，将每个声明链接到来源链和审计发现。系统通过证据 DAG（有向无环图）审计侧车记录所有操作和引用的来源，确保结果可以被验证和再现。
4. 协作团队科学（Collaborative Team Science）: 支持多角色决策治理，当前为单用户桌面模式，但架构设计为未来版本引入共享团队工作区，支持多人协作研究流程。
5. 实际应用场景（Real-world Application Scenarios）: 通过八个端到端用户案例展示系统的实际应用能力，包括多天代理研究冲刺、基因发现、AI 引导的蛋白质设计、分子优化、基因组到 BGC（生物合成基因簇）发现等。
系统架构：
- 薄交互层（thin interaction layer）：保留桌面 GUI 供人类操作和判断。
- 上下文研究能力模式（contextual research capability patterns）：支持多种研究范式。
- 代理运行时和工作流引擎（Agent Runtime and Workflow Engine）：执行自动化研究流程。
- 证据 DAG 审计侧车（Evidence-DAG audit sidecar）：记录操作和来源。
- 科学模型路由器（Scientific Model Router）：根据任务自动选择和路由科学模型。
所有组件以模块化服务形式运行，便于扩展和定制。

Q4: 论文做了哪些实验？

论文通过与实际用户案例的集成来验证 SciForge，共展示了八个端到端使用场景，其中旗舰演示包括：(1) 多天代理研究冲刺用于基因发现（Agentic Research Sprint for gene discovery）：一个多天、PI 控制的代理循环，自动进行文献检索、数据分析和假设生成。(2) AI 引导的从头蛋白质设计（AI-guided de novo protein design）：结合结构预测和序列生成模型进行蛋白质设计。(3) 分子优化（Molecular optimization）：使用化学推理和多轮优化策略改进分子性质。(4) 基因组到 BGC 发现（Genome-to-BGC discovery）：从基因组序列自动识别生物合成基因簇。这些案例的实现和相关工件托管在 GitHub 仓库中，具体包括：Agentic Research Sprint (scenario-o-01-research-sprint)，以及其他案例的代码和配置。论文没有提供标准的量化实验结果（如基准测试分数），而是通过端到端的案例演示系统的能力范围和可用性。系统支持人类在环的监督，并通过证据 DAG 记录整个研究过程，以支持可审计性和再现性。

Q5: 发现了什么实验现象？

从实验中观察到以下现象和趋势：
- 多天代理研究冲刺能够独立运行，但需要人类 PI（首席研究员）在关键决策点进行审查和干预，表明纯自主的科学发现仍然需要 human-in-the-loop。
- 通过将科学文件格式经过领域翻译器转换为文本表示，系统能够使 LLM 理解 FASTA、PDB 等专用数据，但在某些边界情况下（如超大结构文件）翻译可能引入信息损失。
- 证据 DAG 审计侧车能够记录每个步骤的来源，但对存储和查询效率提出要求，长期运行的研究项目可能累积大量 DAG 节点。
- 翻译然后推理模式在处理多模态输入时有效，但当输入类型超出预定义翻译器集合时，系统需要 fallback 策略，可能降低性能。
- 与现有系统的对比（五个维度）显示 SciForge 在目标导向研究设计和研究连续性方面具有优势，但在模型执行和证据质量方面与其他专业系统相当或略为复杂。
- 开源实现使得社区可以复制和扩展用例，但当前状态面向桌面单用户，团队协作场景尚未实现，限制了大规模协同研究的直接评估。
总体而言，系统展示了在多个科学领域中进行自动化工作流执行和人工监督的可行性，但也暴露出工具集成深度、处理边界情况和扩展协作等方面的挑战。

Q6: 有什么可以进一步探索的点？

论文自身和合理推断的未来方向包括：
1. 团队协作深化：当前版本为桌面单用户，未来版本计划增加共享团队工作区，支持多角色实时协作、共享项目状态和联合决策。
2. 移动监督增强：目前支持移动监督（如查看进度、审批），未来可能扩展为全功能移动客户端。
3. 更多科学领域的模型路由集成：科学模型路由器可以扩展以支持更多专业模型（如气象模拟、材料计算、系统生物学模型）。
4. 证据 DAG 的可扩展性和互操作性：需要处理大规模长时间项目中 DAG 的增长，并与外部实验室笔记本和系统集成。
5. 更完善的翻译器生态系统：增加对更多科学文件格式的原生支持（如 cryo-EM 密度图、HDF5 数据、仪器输出），并提高翻译精度。
6. 自动评估基准：建立类似 SciForge 的统一评估框架，量化系统在科学研究效率、再现性、完全性等方面的提升。
7. 跨领域科学发现：从当前聚焦的生物医学（基因、蛋白质、分子）扩展到其他科学领域（化学、物理、地球科学）。
8. 强化学习与主动学习结合：让代理不仅在预设流程中执行，还能主动提出实验建议并学习用户反馈。
9. 隐私与合规：支持本地运行和敏感数据处理，满足科研数据保护要求。
10. 互操作性标准化：与其他科学软件（如 LabKey、benchling、计算集群）对接，形成完整科研基础设施。

Q7: 总结一下论文的主要内容

SciForge 是一个 AI 原生、多模态的研究工作台，旨在解决现代科学研究中工件分散、工具孤立和可审计性不足的问题。论文首先指出科学工作日益涉及异构工件（论文、代码、数据集等），而通用 AI 助手难以将它们维护为连贯、可审计的研究状态。为此，SciForge 提出了五个设计支柱：目标范围科学决策治理、翻译然后推理、证据治理、协作团队科学和实际应用场景。系统架构包括薄交互层、上下文研究能力模式、代理运行时和工作流引擎、证据 DAG 审计侧车和科学模型路由器。
在相关工作中，论文将现有系统分为 GUI 基础工作台、文献中心代理和 AI 科学家三大类，并指出 SciForge 试图统一它们。系统通过八个端到端用户案例进行验证，重点展示了多天代理研究冲刺用于基因发现、AI 指导的蛋白质设计、分子优化和基因组到 BGC 发现等生物医学场景。虽然论文没有提供传统的定量基准实验，但通过案例演示了系统在自动化工作流执行、人类可审计性、多模态处理等方面的能力。
文章还对比了 SciForge 与其他系统在五个维度的定位：执行、模型治理、证据质量、研究连续性、目标导向研究设计，表明 SciForge 在统一性和目标导向方面处于领先位置。局限方面，当前版本缺少真正的团队协作支持和更广泛领域的覆盖。系统已开源在 GitHub，鼓励社区参与和扩展。整体上，本文为构建科学 AI 工作台提供了一个系统级的、面向实践的设计和实现方案，对 AI for Science 领域有直接参考价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文核心关注 AI 代理与科学工作流的结合，适合对 agent（智能体）和 AI for Science 方向感兴趣的研究者。

## 基本信息

- 作者：SciForge Team, Zhangyang Gao, Minghao Fang, Yifei Liu, Hanhui Yang, Xinyu Gu, Shixiang Tang, Siqi Sun, Lei Bai, Cheng Tan, Mengdi Liu, Hao Wu, Shuizhou Chen
- 机构：Shanghai Artificial Intelligence Laboratory (上海人工智能实验室)
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-07-20
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.16038`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成本报告时，参考了提供的 PDF 检索证据片段和字段映射，包括 background、method、results、limitations、relevance 以及相应的 field_evidence_map 内容。
