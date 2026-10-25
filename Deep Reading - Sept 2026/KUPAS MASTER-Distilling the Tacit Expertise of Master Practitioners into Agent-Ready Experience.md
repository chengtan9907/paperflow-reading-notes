---
user_id: "cheng tan"
paper_id: 14282
arxiv_id: "2609.37673v1"
title: "KUPAS MASTER: Distilling the Tacit Expertise of Master Practitioners into Agent-Ready Experience Corpora"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Sept 2026/2609.37673v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.37673v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/reading_reports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-08T23:41:02"
---
# KUPAS MASTER: Distilling the Tacit Expertise of Master Practitioners into Agent-Ready Experience Corpora

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：experience engineering · tacit knowledge distillation · llm agent · cognitive corpus construction

## 一句话总结

提出 KUPAS MASTER，一个基于九层认知语料构建的经验工程平台，将从业者的隐性经验转化为可追溯、可复用的智能体经验资产，并通过大规模评估验证其显著优于原始语料 RAG。

## 摘要

> 论文指出，专业从业者所掌握的不仅是事实和结论，还包括哪些线索重要、为什么某个判断合理、应采取什么行动、以及熟悉方法何时不再适用等隐性知识。常规工作记录往往遗漏这些隐性知识，导致大语言模型（LLM）智能体难以有效利用专业经验。为此，论文提出 KUPAS MASTER，一个围绕九层认知语料构建（nine-layer cognitive corpus construction）的经验工程平台，将异构工作记录和从业者访谈转化为可追溯、可复用的智能体经验语料。平台用六类案例元素（context、cues、judgment、action、boundaries、outcomes）保留任务过程，沿九个提取维度组织隐性经验，并将成果存入六个资产库：规则、约束、最佳实践、负例、边界案例和技能。语义对齐、个人经验蒸馏、组织整合和交叉审查四步流程保留源证据、使用条件和未解决分歧。平台把资产打包为具有显式输入、步骤、依赖和停止条件的可调用技能，将经验收集与任务执行和评估反馈连接起来。使用 20 名随机选取从业者的授权样本，平台处理了 1,576 个源文件，生成 23,024 条个人经验记录和 13,113 条组织资产。评估覆盖 177 个问题和 531 个回答，跨多个专业领域。在统一任务输入和评分标准下，基础模型、原始语料 RAG 和 KUPAS MASTER 智能体的得分分别为 70.63、79.75 和 89.58，KUPAS MASTER 相对原始语料 RAG 提升 9.83 分，且在全部七个评分维度上都有增益。

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：专业从业者的隐性经验（tacit expertise）难以被 LLM 智能体有效利用。具体而言，经验丰富的专业人士不仅知道事实和结论，还知道哪些线索重要、为什么某个判断合理、应采取什么行动、以及熟悉的方法何时不再适用，但常规工作记录往往只留下任务流程和结论，丢失了这些隐性判断依据。因此，如何将分散在从业者头脑和日常记录中的经验转化为可追溯、可复用、可执行的知识资产，成为企业知识管理和智能体落地的关键瓶颈。论文还从宏观背景说明这一问题的重要性：中国的 AI Plus 倡议要求可复用的专家知识和高质量数据集，美国的 Genesis Mission 要求利用联邦科学数据集构建科学基础模型和智能体，两者都强调让领域知识可被 AI 系统使用。所以论文试图构建一个系统化平台，完成从原始工作记录到智能体可调用技能的端到端经验工程。

Q2: 有哪些相关研究？

论文可用片段中未显式列出相关工作，但根据问题背景可合理推断其关联到以下几个研究方向：一是企业知识管理，特别是将专家隐性知识转化为显式知识资产的工作；二是 LLM 智能体的上下文学习与工具使用，尤其是检索增强生成（RAG）在专业问答和任务执行中的应用；三是语料构建与数据处理，包括如何对非结构化文档进行解析、索引和检索以支持知识密集型任务；四是技能库与智能体编排，即将经验打包为可调用技能并接入任务执行流程。此外，论文提到的多领域案例（如工伤认定、调解评估等）表明其与垂直领域智能体平台相关。这些推断基于摘要和引言片段，具体文献综述需查阅论文原文的 Related Work 部分。

Q3: 论文如何解决这个问题？

论文提出 KUPAS MASTER，一个经验工程平台，围绕九层认知语料构建展开，具体方法包括：一、定义六类案例元素（context、cues、judgment、action、boundaries、outcomes），以保留任务过程的关键环节，其中 context 提供背景，cues 列出关键信号，judgment 记录判断理由，action 描述采取的行动，boundaries 界定适用边界和停止条件，outcomes 记录结果和反馈。二、沿九个提取维度组织隐性经验，将访谈和记录中的散点信息结构化。三、将成果存入六个资产库：规则（rules）、约束（constraints）、最佳实践（best practices）、负例（negative examples）、边界案例（corner cases）和技能（skills），每个库对应不同类型的经验知识。四、通过语义对齐（semantic alignment）确保不同来源的术语和概念一致，个人经验蒸馏（individual experience distillation）提取个体特有的判断模式，组织整合（organizational consolidation）将个人经验合并为组织级资产，交叉审查（cross-review）保留源证据、使用条件和未解决分歧。五、平台将这些资产打包为具有显式输入、步骤、依赖和停止条件的可调用技能，使经验能够直接连接到任务执行和评估反馈。整体流程覆盖从原始材料解析、索引、检索到经验资产生成和技能调用的完整链路。

Q4: 论文做了哪些实验？

论文的实验分为语料处理和系统评估两个部分。语料处理方面，使用 20 名随机选取从业者的授权样本，处理了 1,576 个源文件，生成 23,024 条个人经验记录和 13,113 条组织资产。评估方面，构建了覆盖多个专业领域的 177 个问题，并生成 531 个回答（合理推断：每个问题可能由多个模型或多次运行产生，但论文未在可用片段中明确说明）。在统一任务输入和评分标准下，比较了三种条件：基础模型（Base model）、原始语料 RAG（Raw-corpus RAG）和 KUPAS MASTER 智能体（KUPAS MASTER agent）。评分采用七个维度（具体维度论文未在摘要中列出，需查阅原文）。此外，检索片段中提及“core rose from 62.18 to 77.27”，推测可能是解析、索引和检索组件单独评估的中间结果，还有“217 retained reports and 1,345 rows”以及“1,109 C-only wins (82.5%) 和 214 C ties (15.9%)”等数据，推测来自组织整合环节或报告对比分析。这些细节需要根据原文实验章节确认。

Q5: 发现了什么实验现象？

实验发现的核心现象是：KUPAS MASTER 智能体显著优于原始语料 RAG。在统一评分标准下，基础模型得分为 70.63，原始语料 RAG 为 79.75，KUPAS MASTER 智能体为 89.58，相对 RAG 提升 9.83 分，并且在全部七个评分维度上均获得增益，说明经验结构化带来的改进是全面而非局部的。检索片段还显示“parsing, indexing, and retrieving source materials strengthen evidence-grounded answers”，表明中间环节对答题质量有正向贡献。另一个片段提到“A topic change alone is not a state change”以及“Stated confidence, evidence sufficiency, graph gaps, and role boundaries remain separate”，推测论文在评估中区分了表面表达与真实状态判断，强调语言上的自信不能直接等同于证据充分性或角色边界。此外，报告级别的对比显示“C-only wins (82.5%)”和“C ties (15.9%)”，说明在大量报告中 KUPAS MASTER 输出优于 RAG 或与其持平，负例较少。还有“Asset count vs C−B (Spearman ρ=0.07)”和“Raw-corpus scale vs…”等片段，推测论文检查了资产数量或语料规模与性能差异之间的相关性，结果可能是弱相关（ρ=0.07），但需原文确认。

Q6: 有什么可以进一步探索的点？

基于论文框架和可推断的边界，可以进一步探索的方向包括：一、扩大领域覆盖和样本规模，验证平台在不同专业场景（如法律、医疗、工程、科研）中的通用性；二、研究如何自动评估经验资产的质量，以及如何将评估反馈闭环到语料构建流程中；三、探索技能的跨任务迁移和复用，即一个领域提取的技能能否在相关任务中泛化；四、深化对隐性知识中“边界条件”和“停止条件”的形式化表达，使智能体更可靠地判断何时不应沿用旧方法；五、在科学场景（如 Genesis Mission 提到的科研自动化）中应用该平台，将科研人员的工作记录转化为可执行的假设检验流程；六、处理跨组织知识共享中的隐私、授权和信任问题，尤其是保留未解决分歧的机制如何影响决策质量；七、与强化学习或在线学习结合，让智能体在实际执行中更新经验资产。这些方向部分是论文隐含的延伸，需要结合原文讨论部分进一步确认。

Q7: 总结一下论文的主要内容

KUPAS MASTER 论文围绕一个核心问题展开：专业从业者的隐性经验如何被系统地转化为智能体可用的经验语料。论文首先指出，专业经验包含大量超越事实和结论的层面——哪些线索有判别力、为什么某个判断合理、应该采取什么行动、以及何时既有方法失效——这些隐性知识通常不会出现在常规工作记录中，导致 LLM 智能体难以在专业任务中表现稳定。论文从国家战略层面论证了该问题的紧迫性，包括中国的 AI Plus 和美国 Genesis Mission，都要求领域知识可被 AI 系统使用。为应对这一挑战，论文提出 KUPAS MASTER 平台，其核心是九层认知语料构建。平台定义六类案例元素（情境、线索、判断、行动、边界、结果）来保留任务过程；沿九个提取维度从访谈和记录中抽取隐性经验；将成果存入六个资产库（规则、约束、最佳实践、负例、边界案例、技能）。构建过程包含语义对齐、个人经验蒸馏、组织整合、交叉审查四个阶段，分别处理概念一致性、个体特殊经验、集体知识汇聚和分歧保留，从而确保资产具有可追溯性、明确适用条件和保留未解决分歧。最后，平台将资产打包为带有显式输入、步骤、依赖和停止条件的可调用技能，连接经验收集、任务执行和评估反馈，形成完整闭环。实验部分使用 20 名从业者的授权样本，处理 1,576 个源文件，产出 23,024 条个人记录和 13,113 条组织资产。评估涉及 177 个问题和 531 个回答，在统一评分标准下比较基础模型、原始语料 RAG 和 KUPAS MASTER 智能体，得分分别为 70.63、79.75 和 89.58，后者相对 RAG 提升 9.83 分，且在全部七个评分维度上均有增益。结论强调，资产和技能为智能体提供了判断条件和使用边界，运行记录证实了其在真实任务中的有效性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文与用户画像中的“agent”方向直接相关，核心目标是提升 LLM 智能体在专业任务中的表现。

## 基本信息

- 作者：Changmian Wang, Yuchao Ma, Xuchao Lu, Chen Zhang, Ping Sun, Jiazheng Wang, Shan Wang, Xuanwen Chen, Yihe Sun, Ziyu Lu, Jianqiang Huang, Hongzhi Li, Ziqing Xia, Kaihua Tang, Xian-Sheng Hua, Qinghua Zheng
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-09-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.37673v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的摘要、引言和结论片段，并结合论文元数据进行了推断性补充；部分实验细节因可用证据有限而标注为合理推断或推测。
