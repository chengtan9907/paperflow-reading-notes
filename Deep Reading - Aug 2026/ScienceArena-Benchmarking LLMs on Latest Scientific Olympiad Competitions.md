---
user_id: "cheng tan"
paper_id: 10004
arxiv_id: "2608.30517v1"
title: "ScienceArena: Benchmarking LLMs on Latest Scientific Olympiad Competitions"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.30517v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.30517v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-02T01:43:02"
---
# ScienceArena: Benchmarking LLMs on Latest Scientific Olympiad Competitions

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：science olympiad · benchmark saturation · data contamination · scientific reasoning

## 一句话总结

本文提出 ScienceArena，一个基于十三项物理、化学、生物奥林匹克竞赛的开放式多步推理基准，通过专家审核的数字化工序与 LLM-as-judge 校准，评估了十四个最新 LLM，发现顶级模型在多个国际竞赛上达到奖牌级水平，而化学与长程一致性仍为关键瓶颈。

## 摘要

> Benchmark saturation and data contamination increasingly obscure genuine scientific reasoning in frontier LLMs. We introduce \textsc{ScienceArena}, an olympiad-style benchmark from thirteen public science competitions in physics, chemistry, and biology, including IPhO and IChO 2025--2026, IBO 2023, USAPhO 2026, and USNCO 2025. Its open-ended, multi-step problems use process-credit rubrics, making faithful scoring difficult. We build ScienceArena through an expert-audited digitization pipeline that converts official exams, figures, solutions, and rubrics into structured items verified by olympiad medalists. To scale evaluation beyond costly human grading, we calibrate LLM-as-judge against medalist ground truth on archived answers from five models across IPhO and IChO; two strong judges stay within one point of expert total scores. Medalist notes show that failures often stem from visual grounding, structure fidelity, and global problem control rather than missing terminology. Evaluating fourteen recent LLMs with interleaved solving, we find that top models obtain medal-equivalent rubric scores on several public international exams, while chemistry and long-horizon consistency remain key bottlenecks. We provide an interactive \href{https://science-arena.onrender.com/}{demo}.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：前沿 LLM 的科学推理能力评估陷入困境。具体表现为：(1) 现有基准（如 MMLU、GPQA 等）逐渐饱和，模型得分接近或超过人类表现，区分度下降；(2) 数据污染严重，由于训练语料包含公开题目，模型可能凭记忆作答而非真正推理，导致评估结果失真；(3) 多数基准采用选择题或短答案，无法测量多步骤、开放式推理过程，而科学推理恰恰包含假设、推导、验证等复杂环节；(4) 科学奥林匹克竞赛提供高分辨率、专家设计的题目，但其开放式答题与过程评分对自动化评估构成挑战，需要可信的评分机制。因此，论文旨在构建一个低污染、高难度、能够区分推理能力并忠实反映过程得分的基准，同时探索可扩展的自动评分方法，以支撑对大模型科学推理能力的可信评估。

Q2: 有哪些相关研究？

论文在引言中提及了一系列相关研究（如 Hendrycks et al., 2021; Li et al., 2023; Rein et al., 2023; Yue et al., 2023; He et al., 2024; Lu et al., 2022; Laurent et al., 2024; Mirza et al., 2025），但摘要和检索片段未展开具体内容。合理推断，这些工作覆盖了通用知识基准（如 MMLU）、科学推理基准（如 GPQA、SciBench）、数学/代码竞赛基准（如 MATH、Codeforces），以及数据污染检测方法。此外，与本研究直接相关的还有：(1) 基于竞赛的评测，如数学奥林匹克（IMO）相关基准，但 ScienceArena 扩展到物理、化学、生物等多个学科；(2) LLM-as-judge 的自动评估方法，特别是对开放式答案的评分，本研究将其应用于科学奥赛场景并对照奖牌获得者进行校准；(3) 交错求解（interleaved solving）的评估协议，可能涉及多模态输入或多轮推理。由于论文尚未公开完整文本，具体文献细节无法核实，但上述方向可作为相关研究的主要脉络。

Q3: 论文如何解决这个问题？

论文通过以下步骤构建 ScienceArena 并评估 LLM：

1. **数据来源与范围**：选取十三个公共科学竞赛，涵盖物理（如 IPhO, USAPhO）、化学（如 IChO, USNCO）、生物（如 IBO）等学科，时间跨度包括 2023 至 2026 年，保证题目新颖性并降低污染风险。

2. **数字化流水线**：设计专家审核的转换流程，将官方考试题目、图表、参考答案和评分标准（rubrics）转换为结构化条目。该流程由领域专家和奥林匹克奖牌获得者参与复核，确保条目忠实于原题且信息完整。

3. **过程积分评分**：采用竞赛官方使用的 process-credit rubric，即依据推理步骤和中间结论给分，而非仅看最终答案。这使得评分能反映模型的多步推理质量，但增加了自动评分的难度。

4. **LLM-as-judge 校准**：为了规模化评估，论文训练/校准 LLM 作为评判者。具体做法是，从五个模型在 IPhO 和 IChO 上的存档答案中，对比 LLM judge 给出的分数与奖牌获得者的真实评分，选择最接近专家评判的模型作为自动评分器。结果显示两个强 judge 的分数与专家总分相差不超过一分，说明其评分可靠性较高。

5. **交错求解协议**：评估十四个近期 LLM 时，采用交错求解（interleaved solving）方式，即模型在解题过程中可以查看题目中的图表、公式等视觉信息，并生成逐步推理过程，而非一次性输出答案。

6. **分析框架**：除了总体分数，还收集奖牌获得者的评注，以诊断模型失败的具体原因（如视觉接地、结构保真、全局控制等），并据此分析模型能力的短板。

该方法的关键优势在于：利用奥林匹克竞赛的权威性和新颖性对抗基准饱和与污染，通过过程评分和专家评注提供更细致的推理能力画像，并通过 LLM-as-judge 的可信校准实现低成本的规模化评估。

Q4: 论文做了哪些实验？

论文进行了以下主要实验：

1. **LLM-as-judge 校准实验**：在 IPhO 和 IChO 数据集上，选取五个模型的存档答题结果（具体模型名称未在摘要中列出），由 LLM judge 对答案进行评分，并与人类奖牌获得者的评分进行比较。通过调整 judge 的提示或参数，挑选出两个与专家评分最接近的 judge（分数差不超过 1 分）。

2. **主评估实验**：对十四个近期 LLM（模型列表未在摘要中给出）在 ScienceArena 全部或部分竞赛题目上进行交错求解测试，记录每个模型的 rubric 得分，并转换为是否达到奖牌等效分数线。

3. **错误分析与诊断**：结合奖牌获得者在评阅模型答案时撰写的书面意见（medalist notes），分析模型在哪些环节失败，例如视觉理解、结构保真、全局问题控制等，从而为能力缺陷提供定性解释。

4. **跨学科对比**：对比模型在物理、化学、生物三个学科上的表现，识别学科间瓶颈（如化学表现相对较弱）。

由于论文尚未全文公开，实验的具体设置（如模型规模、提示模板、得分阈值等）无法核实，上述描述基于摘要和检索片段推断。

Q5: 发现了什么实验现象？

根据摘要和检索片段，论文揭示了以下实验现象：

1. **顶级模型可达到奖牌级水平**：在若干公共国际竞赛（如 IPhO）上，最先进的 LLM 获得了与人类奖牌获得者等效的 rubric 分数，说明这些模型已具备相当强的科学推理能力。

2. **化学是主要瓶颈**：模型在化学竞赛上的表现明显弱于物理和生物，这可能源于化学需要更精细的符号操作、反应机制推理以及专业术语理解。

3. **长程一致性（long-horizon consistency）不足**：在需要多步骤、长链条推理的问题中，模型容易出现前后矛盾或逻辑断裂，即使最终答案正确，中间过程也可能有缺陷。

4. **失败根源并非术语缺失**：奖牌获得者的评注显示，模型的常见失败并非因为不了解专业术语，而是：(a) 视觉接地不足，即无法正确解析图形、图表中的信息；(b) 结构保真问题，即推理过程的结构不符合科学论证规范，如跳跃或冗余；(c) 全局问题控制薄弱，即难以统筹整个问题，可能顾此失彼。这暗示模型的推理缺陷更偏向于感知与规划层面，而非知识层面。

5. **LLM-as-judge 的可靠性**：在合理校准下，选出的两个 LLM judge 与专家总分的偏差不超过 1 分，表明自动评分在聚合分数层面是可信的，但可能对个别答案的细节判断仍存在分歧（需要进一步分析）。

这些观察为提升模型科学推理能力提供了明确的方向：改进视觉与文本交错理解，增强结构化推理的一致性，以及加强化学领域专项训练。

Q6: 有什么可以进一步探索的点？

基于论文的限制作答和发现，可探索的进一步方向包括：

1. **扩展竞赛覆盖范围**：纳入更多竞赛和年份，尤其是不同国家与地区的奥赛，以减少区域偏差，检验基准的通用性；同时可加入实验操作类项目（如需要物理装置或实验室操作的题目），但目前因无法远程开展而未覆盖。

2. **改进视觉接地能力**：针对模型在图形、图表理解上的失败，可结合多模态模型或专门的视觉推理训练，使模型能够更准确地将视觉信息转化为符号推理基础。

3. **增强长程一致性**：研究如何在推理过程中保持全局一致，例如通过自我反思、规划分解或记忆增强机制，减少多步骤推理中的逻辑断裂。

4. **优化自动评分**：虽然 LLM-as-judge 在总分层面接近专家，但仍需研究其在细粒度过程评估中的可靠性，尤其是在复杂推理和创造性解法上，可考虑结合规则评分与 LLM 判断的混合方法。

5. **污染检测与抗污染训练**：由于竞赛题目公开，需研发更稳健的污染检测方法（如改写痕迹、时间戳验证），并探索动态出题或题目生成技术以减少未来污染风险。

6. **诊断性评注的自动化**：奖牌获得者的评注具有极高的诊断价值，但人工成本高。可尝试训练模型自动生成类似的结构化反馈，以大规模揭示模型能力缺陷。

7. **跨学科迁移研究**：分析模型在物理、化学、生物不同学科上的能力相关性，探究是否存在通用的科学推理机制，以及学科差异是否源于知识表示或推理策略的不同。

8. **基准的生态建设**：提供公开的 leaderboard、代码和交互式 demo，鼓励社区参与扩展和验证，并探索将 ScienceArena 作为教育或模型迭代的评测环境。

Q7: 总结一下论文的主要内容

论文《ScienceArena: Benchmarking LLMs on Latest Scientific Olympiad Competitions》针对现有 LLM 科学推理评测中基准饱和与数据污染的问题，构建了一个以奥林匹克科学竞赛为核心的评测基准，并系统评估了当前顶尖模型的表现。

**研究背景**：随着 LLM 在知识类任务上的高分，传统基准难以区分真实推理与记忆，亟需高难度、多步骤、开放式的评测任务。科学奥林匹克竞赛由领域专家命题，具有权威性、难度高、过程可评分等优点，但对自动化评估构成挑战。

**基准构建**：论文从物理、化学、生物三个学科选取十三个公共竞赛（如 IPhO、IChO 2025-2026、IBO 2023、USAPhO 2026、USNCO 2025），通过专家审核的数字化工序将官方试题、图表、解答和评分标准转换为结构化条目，并由奖牌获得者验证。每个问题使用官方 process-credit rubric 评分，以捕捉多步骤推理中的部分得分。

**评估方法**：为降低人工评分成本，论文采用 LLM-as-judge 策略。利用五个历史模型在 IPhO/IChO 上的答案，将 LLM judge 的评分与奖牌获得者的评分进行对比，挑选出两个最接近专家的 judge（总分差 ≤1 分）。之后，对十四个近期 LLM 采用交错求解（即允许模型查看图表并输出中间推理）的方式评估，收集模型答案并由 judge 和奖牌获得者共同评阅。

**主要发现**：(1) 顶级模型在多个国际物理奥赛上达到奖牌等效分数，显示其推理能力接近人类顶尖水平；(2) 化学是普遍弱项，显著低于物理和生物；(3) 模型存在长程一致性问题，复杂多步推理中易出现逻辑断裂；(4) 奖牌获得者的评注揭示失败主要源于视觉接地、结构保真和全局问题控制，而非术语缺失；(5) 校准后的 LLM judge 在总分上与专家一致，但细节评阅仍有局限。

**贡献与局限**：ScienceArena 提供了高分辨率、低污染的科学推理评测资源，并验证了自动评分的可行性。局限包括：仅评估理论部分，实验类题目未覆盖；竞赛覆盖有限，结果可能不具学科普适性；专家评分成本高，大规模扩展受限。

**总结**：论文通过科学严谨的基准构建与评估揭示了当前 LLM 在科学推理上的能力边界，尤其厘清了视觉理解、结构组织与全局规划方面的短板，为后续模型改进和评测研究提供了坚实基础。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文属于 AI for Science 方向（用户画像中权重 0.10），核心是探索 LLM 在科学推理上的能力边界，与用户关注点一致。

## 基本信息

- 作者：Guangxiang Zhao, Qilong Shi, Xusen Xiao, Wenpu Liu, Yaoming Li, Linfeng Hao, Shuyang Hou, Zijian Guo, Xinrui Zhang, Yuntian Zhao, Zhengyang Wang, Wenrui Liu, Yuhan Wu, Tong Yang, Lin Sun, Xiangzheng Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.30517v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了论文摘要、结论和引言的部分语义检索证据，其余内容（如方法细节、具体实验设定）基于摘要与片段进行合理推断，未能核实论文全文。
