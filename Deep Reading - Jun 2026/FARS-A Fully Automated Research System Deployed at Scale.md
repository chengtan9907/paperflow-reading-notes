---
user_id: "cheng tan"
paper_id: 2139
arxiv_id: "2606.31651v1"
title: "FARS: A Fully Automated Research System Deployed at Scale"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.31651v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.31651v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-07-02T01:35:17"
---
# FARS: A Fully Automated Research System Deployed at Scale

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：automated research · AI agent · large language model · multi-agent system

## 一句话总结

FARS是一个全自动的AI-for-AI研究系统，在首次公开部署中自主生成了166篇覆盖67个细粒度AI/ML主题的完整论文，并通过282份结构化评审揭示了其产生可评审论文的能力与常见失败模式。

## 摘要

> Recent automated research systems show that language-model agents can generate hypotheses, run experiments, and write complete manuscripts, but most evidence still comes from selected examples, human-framed topics, or a few pre-defined research tasks. We present FARS (Fully Automated Research System), a fully automated AI-for-AI research system designed to operate across research topics at scale. FARS autonomously generates and advances projects through ideation, planning, experimentation, and writing, using stage-specific agents coordinated through a shared workspace that records proposals, code, logs, results, and manuscripts. In its first public deployment, FARS produced 166 complete research papers spanning 67 fine-grained AI/ML topics while preserving intermediate artifacts as an auditable corpus rather than a curated set of successes. We evaluate this corpus with 282 structured reviews from volunteer reviewers covering 140 papers, including overall ratings, sub-scores, integrity checks, and LLM-use disclosure. The reviews indicate that FARS can produce review-worthy and occasionally strong AI/ML research artifacts in a large-scale public deployment, while also exposing recurring failure modes in narrow experimental scope, methodological limitations, and integrity issues.

Q1: 这篇论文试图解决什么问题？

现有自动研究系统大多在精选示例、人工限定主题或少量预定义任务上展示能力，缺乏在开放领域大规模运行的系统性证据。关键问题包括：1) 系统能否真正自主选题而非依赖人类预设问题？2) 在大规模持续运行中，系统产出质量分布如何？3) 是否存在可复现的失败模式？4) 如何构建可审计的语料库而非仅展示成功案例？FARS旨在回答这些系统级问题，提供第一个大规模、全自动、可审计的研究系统实证。

Q2: 有哪些相关研究？

相关研究包括：1) 基于LLM的自动科学发现系统（如GPT-researcher、SciAgents等），通常聚焦于文献总结或假设生成，但实验执行和论文写作多为模拟或有限范围。2) AI-for-AI方向，利用AI辅助AI研究（如AutoML、NAS、自动超参优化），但未覆盖从选题到发表的全流程。3) 多代理研究系统（如CAMEL、MetaGPT等），用于协作任务，但通常需要人类设定目标且规模较小。4) 大规模系统行为研究（如LLM沙盒、AI科学家β），但缺乏对产出质量的系统评审。FARS的独特之处在于：全自动化选题、多阶段流水线、大规模公开部署（166篇论文），以及结构化评审评价。

Q3: 论文如何解决这个问题？

FARS是一个多代理研究系统，将开放研究方向转化为完整的假设验证论文。系统包含四个专门阶段：1) 构思阶段（Ideation）：自动探索和生成候选研究假设，基于现有文献和领域趋势提出新方向。2) 规划阶段（Planning）：将假设转化为可执行的实验方案，包括方法设计、数据集选择、评价指标定义。3) 实验阶段（Experiment）：自动执行代码实现、训练、评估和日志记录，中间结果存储在共享工作空间。4) 写作阶段（Writing）：基于实验结果和计划撰写完整论文，包括摘要、引言、方法、实验和结论。所有阶段通过共享工作空间协调，记录提案、代码、日志、结果和手稿，形成可审计语料库。系统设计强调无需人类干预，从起始方向到最终论文完全自主运行。

Q4: 论文做了哪些实验？

实验设计为首次公开部署：1) 系统自主从AI/ML领域选题，最终覆盖67个细粒度主题。2) 生成了166篇完整研究论文。3) 部署持续417小时，消耗216亿模型token，总成本约186,000美元（含GPU集群使用）。4) 通过志愿者评审对产出进行评估，共282份结构化评审覆盖140篇论文，包含整体评分、子评分（合理性、表达、贡献）、评审者置信度、完整性检查和LLM使用披露。评审过程设计严谨，允许多轮评审比较。

Q5: 发现了什么实验现象？

1) 产出质量分布广泛：非所有论文为最高质量，但部分论文获得了认可，甚至被认为可提交至同行评审会议。2) 评审一致性：不同评审者对同一论文的评分存在一定变化，但结构评审工具帮助捕捉了质量差异。3) 失败模式：常见失败包括实验范围狭窄（如仅在一个数据集或小规模上验证）、方法论局限（如基准选择不当、消融不充分）和完整性问题（如图表格式错误、引用缺失或错误）。4) 成本效益分析：生成166篇论文总成本18.6万美元，平均每篇约1120美元，但质量差异大，需进一步优化。5) 完整性检查：部分论文存在代码不可复现或日志不完整问题。

Q6: 有什么可以进一步探索的点？

1) 改进失败模式：针对实验范围狭窄、方法局限和完整性问题优化代理引导逻辑。2) 跨领域扩展：将FARS应用于AI/ML之外的科学领域（如生物学、化学），需要适配不同实验范式。3) 评审反馈闭环：利用评审结果自动改进后续生成，形成自优化循环。4) 质量提升：引入更先进的模型（如GPT-5）或多模型集成提高论文深度。5) 伦理与可审计性：增强透明度和可复现性，确保自动生成研究符合学术规范。6) 降低成本和延迟：优化token使用和计算资源调度。7) 多轮迭代：允许论文根据评审反馈自动修订再提交，模拟真实审稿流程。

Q7: 总结一下论文的主要内容

现有自动研究系统虽展示了大语言模型代理在假设生成、实验执行和论文写作方面的潜力，但缺乏大规模、自主选题、可审计的公开部署证据。FARS（Fully Automated Research System）是一个完全自动化的AI-for-AI研究系统，旨在开放领域大规模运行。系统由四个阶段专用代理（构思、规划、实验、写作）组成，通过共享工作空间协调，从无到有生成完整论文并保留全部中间产物。首次公开部署在417小时内自主生成了166篇论文，覆盖67个细粒度AI/ML主题，总成本约18.6万美元。通过282份来自志愿者评审的结构化评审（覆盖140篇论文），包括整体评分、子评分、完整性检查和LLM使用披露，系统展示了能产出值得评审乃至高质量研究产物的能力。同时，评审也揭示了常见失败模式：实验范围狭窄、方法论局限（如基准选择不当、消融不足）和完整性问题（如图表格式、引用错误等）。该工作首次提供了大规模全自动AI研究系统的实证，为评估和改进此类系统奠定了基础。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：直接涉及大规模AI智能体系统（agent），与用户画像中的agent方向（权重0.10）高度相关

## 基本信息

- 作者：Qiong Tang, Xiangkun Hu, Xiangyang Liu, Yiran Chen, Yunfan Shao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.31651v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF检索证据，包括摘要、引言、方法、结论等片段，并结合了启发式草稿和字段映射。
