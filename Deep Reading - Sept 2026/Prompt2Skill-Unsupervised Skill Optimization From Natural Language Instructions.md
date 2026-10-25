---
user_id: "cheng tan"
paper_id: 14515
arxiv_id: "2609.38593v1"
title: "Prompt2Skill: Unsupervised Skill Optimization From Natural Language Instructions"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.38593v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.38593v1.pdf"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-09T12:59:28"
---
# Prompt2Skill: Unsupervised Skill Optimization From Natural Language Instructions

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：skill optimization · prompt optimization · agent framework · synthetic data generation

## 一句话总结

本文提出 Prompt2Skill，一个仅从自然语言任务描述出发、无需目标任务训练集即可自动构建并优化可复用技能（skill）的框架，通过 Data Agent 构建任务匹配的代理数据、Skill Agent 在闭环反思编辑中精炼技能，在四个领域、五个模型上平均比直接提示提升 10.8 分（相对提升 36.1%），比现成技能提升 83.0%。

## 摘要

> Skills are external artifacts that Large Language Models (LLMs) consume at inference time to improve their performance on specialized domains by incorporating relevant procedural and domain knowledge. Expert-authored skills are expensive to produce, and the resulting artifacts are not optimized for the specific model that consumes them, whose failure modes can vary with version, scale and training. In addition, emerging tasks may fall outside the scope of existing skill libraries, creating a need to develop new skills before curated training data become available. Recent works have explored automated skill optimization through reflection, but they require a curated, in-distribution training set, which users might not always have. To address these limitations, we present Prompt2Skill, a framework that builds skills from natural-language task description alone. From the prompt, the system derives a task specification, discovers or synthesizes datasets, and refines the skill in a closed loop of reflective editing. Across four domains spanning question answering, reading comprehension, spreadsheet manipulation, and mathematical reasoning, Prompt2Skill consistently outperforms the direct prompting baseline, achieving an average improvement of 10.8 across open-source and frontier models.

Q1: 这篇论文试图解决什么问题？

本文要解决的问题可拆为三层：1) 技能（skill）作为 LLM 推理时消费的外部工件，能注入程序性知识和领域知识以提升专业领域表现，但专家撰写技能成本高，且写好的技能没有针对具体消费模型优化——不同版本、规模、训练的模型失败模式不同，通用技能难以适配。2) 新出现的任务往往超出既有技能库范围，而在 curated 训练数据出现之前就需要开发新技能，传统依赖训练集的做法有滞后性。3) 现有自动技能优化工作（如基于 reflection 的方法）要求提供 curated、in-distribution 的训练集，用户不一定有；因此需要一种仅凭自然语言任务描述就能完成技能构建与优化的方法。核心矛盾是：在缺少目标任务标注数据的前提下，如何让系统自己发现/合成可信的代理数据，并据此在闭环中持续改进技能，同时避免过拟合特定数据批次。

Q2: 有哪些相关研究？

相关工作分两条线：1) Skill Optimization / Prompt Optimization：APE（Zhou et al., 2023）生成并筛选指令候选；ProTeGi（Pryzant et al., 2023）用自然语言批评（critiques）引导 prompt 编辑；GEPA（Agrawal et al., 2026）结合执行反馈反思与 Pareto 候选搜索；Trace2Skill（Ni et al., 2026）把执行轨迹中的教训固化为可迁移技能目录；SkillOpt（Yang et al., 2026）通过有界编辑（bounded edits）和验证门控更新（validation-gated updates）精炼技能文档。2) NLP AutoML：Prompt2Model（Viswanathan et al., 2023）把任务描述转成可部署模型（数据集/模型检索、合成数据、SFT）；AutoML-Agent（Trirat et al., 2025）用检索增强规划与多阶段验证协调多个 agent 完成从数据检索到模型部署的 pipeline；MLZero（Fang et al., 2025）结合多模态数据理解和记忆引导代码生成；AIDE（Jiang et al., 2025）把机器学习工程建模为可执行解空间上的树搜索。Prompt2Skill 与这两条线的关系是：它把 prompt optimization 的对象从单条指令扩展到可复用技能文档，同时把 AutoML 的思路（数据检索/合成）引入技能构建，但最终交付物不是模型而是冻结模型的技能。附录还讨论了 Prompt2Skill 可视为 NLP AutoML 中任务特化（task specialization）的另一种途径。

Q3: 论文如何解决这个问题？

Prompt2Skill 的核心管线（合理推断，基于摘要与结论）分为四步：1) 从自然语言任务描述推导任务规格（task specification），明确任务定义、输入输出格式、评估方式等；2) Data Agent 通过三种途径构建任务匹配的代理数据（proxy data）：检索（retrieval）、适配（adaptation）、生成（generation）——检索已有相关数据集、把已有数据适配到目标任务格式、或直接生成合成数据；3) Skill Agent 基于执行反馈进行反思编辑（reflective editing），即把技能应用到代理数据上，观察失败案例，用自然语言反思指导技能文档的修改；4) 用统计筛选（statistical screening）在 fresh batches（新鲜批次，避免同批次过拟合）上验证每次编辑是否带来显著提升，只有通过验证的编辑才被接受，形成闭环迭代。最终得到针对特定冻结模型优化的可复用技能文档，推理时以外部工件形式注入模型。关键机制包括：数据与技能分离、数据代理与技能代理分工、闭环反思与统计门控。

Q4: 论文做了哪些实验？

论文实验覆盖四个领域：1) 问答（SearchQA、SQuAD）；2) 阅读理解（具体数据集名未在证据中完整出现，SQuAD 通常属抽取式问答/阅读理解边界，证据里 General reasoning: SearchQA and SQuAD 出现于某附录表格，合理推断阅读理解数据集属于该领域组合）；3) 电子表格操作（SpreadsheetBench，证据中出现 Qwen3-8B 在 SpreadsheetBench 上的续表，含 Python 代码片段 defaultdict 处理 transactions 行，说明评测涉及让模型写代码操作电子表格）；4) 数学推理（证据中出现 Zebra 逻辑谜题类约束传播内容，如“If X has A, then X has B → assign B to X”，合理推断数学/逻辑推理任务包含此类约束满足问题）。模型方面：五个目标模型，包含开源模型（证据明确出现 Qwen3-8B）和前沿/闭源模型（摘要称 frontier models，具体名称未在证据中给出）。共 19 个模型-基准组合（4 个基准 × 5 个模型理论上 20 个组合，19 个说明至少一个组合缺失或领域内多数据集加总）。对照：直接提示 baseline（direct prompting）与现成技能（off-the-shelf skills）。附录包含 NLP AutoML 应用讨论（把 Prompt2Skill 视为任务特化途径）与技能内容示例（Zebra 约束传播规则表、SearchQA/SQuAD 的 Pass 2 选择策略和常见陷阱）。

Q5: 发现了什么实验现象？

可观察到的实验现象：1) Prompt2Skill 在所有四领域、五模型上一致优于直接提示 baseline，平均绝对提升 10.8 分，说明任务描述单独就能驱动有效的技能构建，无需目标任务训练集。2) 按 19 个模型-基准组合平均，相对直接提示提升 36.1%，相对现成技能提升 83.0%；“现成技能”与“直接提示”之间差距巨大，说明 off-the-shelf 的通用技能对特定模型/任务的适配不足，也说明针对消费模型优化技能的价值。3) 技能内容示例（Zebra 谜题）显示系统生成的技能包含结构化约束传播规则（位置约束、双条件约束、条件约束的传播逻辑），说明反思编辑产出的不只是规则列表，而是可操作的推理程序。4) SearchQA/SQuAD 技能示例包含“Pass 2 — Selection”和“Common traps”等元认知内容（如区分“true statements”与“most precisely correct”），提示技能优化会自动积累关于题目陷阱的归纳知识。5) SpreadsheetBench 的技能与代码执行相关（如对 transactions 行做 defaultdict 聚合），说明技能可以跨“文本推理”与“代码操作”两种形态。注意：证据中未出现消融实验、scaling trend、失败案例或指标间张力的直接描述，这些在原文中可能存在但本次检索未命中，需回原文确认。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向：1) 技能的可迁移性与跨模型泛化：当前技能针对特定消费模型优化，能否构建跨版本、跨规模的通用技能，或做模型间的技能迁移/蒸馏。2) 代理数据质量与偏差控制：检索、适配、生成三种数据来源的比例如何影响最终技能质量，合成数据引入的系统偏差如何检测与缓解。3) 收敛性与编辑门控机制：统计筛选的显著性阈值、批次大小、编辑预算如何设置，闭环是否会过拟合代理数据分布，是否需要多样性正则。4) 技能库的组织与检索：当技能数量增多时，如何做技能库索引、版本管理、冲突消解和按任务检索。5) 扩展到更多任务形态：当前四领域偏文本推理与代码表格操作，可扩展到多模态、长程 agent 任务、决策与控制等。6) 与 AutoML 的更深结合：附录已指出可视为 NLP AutoML 的任务特化途径，可探索技能优化与模型微调（SFT）、模型检索的互补或替代关系。7) 安全与可靠性：自动生成的技能可能包含错误推理规则或有害内容，需要验证机制与风险边界。8) 理论分析：AI Use Disclosure 提到 formulate theoretical proof，但证据未展示证明内容，可追问技能优化的收敛性、样本复杂度或提升下界。

Q7: 总结一下论文的主要内容

Prompt2Skill 的论证主线是：技能（skill）作为 LLM 推理时的外部工件能提升专业领域表现，但存在三个现实障碍——专家编写成本高、技能未针对具体模型优化、新任务超出既有技能库。现有自动技能优化依赖 curated 训练集，不满足“新任务早期无训练数据”的场景。因此作者提出只依赖自然语言任务描述的技能构建框架：由任务描述推导任务规格，Data Agent 通过检索/适配/生成构建代理数据，Skill Agent 基于执行反馈做反思编辑，并用 fresh batches 上的统计筛选门控更新。技术主线上，框架是典型的双代理闭环：数据代理负责“造题”，技能代理负责“改技能”，统计筛选负责“把关”，三者形成类似 AutoML 的自动化循环，但交付物是冻结模型的技能文档而非模型权重。实验主线上，论文在问答（SearchQA、SQuAD）、阅读理解、电子表格操作（SpreadsheetBench）、数学推理四个领域、五个模型（含 Qwen3-8B 等开源与前沿模型）上验证，共 19 个模型-基准组合：平均绝对提升 10.8 分，相对直接提示提升 36.1%，相对现成技能提升 83.0%，说明任务描述单独即可引导有效的技能构建。附录展示了技能内容示例（Zebra 谜题的约束传播规则表、SearchQA/SQuAD 的选择策略与常见陷阱），以及把 Prompt2Skill 视为 NLP AutoML 任务特化途径的讨论。论文还包含 AI Use Disclosure，声明使用了生成式 AI 辅助头脑风暴、部分代码实现和理论证明草拟。需要注意：摘要里的 10.8 与结论里的 36.1%/83.0% 是两套口径（前者是绝对分提升，后者是相对提升，且后者按 19 个组合平均），不能混读。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与画像中 agent（0.10）方向直接重合：Data Agent 与 Skill Agent 的双代理闭环是典型的多代理自动化框架，且技能最终服务于 agent 类应用（技能作为推理时外部工件）

## 基本信息

- 作者：Bo Ni, Li Li, Ryan A. Rossi, Franck Dernoncourt, Tyler Derr
- 机构：未提供
- 来源：arxiv
- 主题/分类：high_relevant
- 日期：2026-09-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.38593v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（124 个 chunk 的检索结果），证据主要覆盖 Introduction、Conclusion 和附录表格片段；主实验分项数字与部分数据集名证据不足，已在相应字段标注需回原文核对。
