---
user_id: "cheng tan"
paper_id: 6200
arxiv_id: "2608.02287v1"
title: "SKT: Skill-Use Training at Scale via Verified Synthetic Data Generation"
publish_date: "2026-08-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.02287v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.02287v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-05T01:51:03"
---
# SKT: Skill-Use Training at Scale via Verified Synthetic Data Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agent skills · synthetic data generation · verification · skill-use training

## 一句话总结

SKT 提出一种基于验证的合成数据流水线，从大规模 Agent Skills 库中自动构造技能约束任务并生成可执行的已验证轨迹，通过监督微调系统性提升语言模型智能体的技能识别、应用与协同能力，并配套构建了 held-out 基准 SkillEval。

## 摘要

> Agent Skills have become an important mechanism for equipping language-model agents with reusable procedural knowledge. However, providing skills alone does not guarantee that current models can identify, apply, and coordinate them effectively. To improve models' skill-use capabilities, we introduce SKT, a verified data synthesis pipeline that constructs skill-grounded tasks and executable trajectories from large collections of Agent Skills. SKT selects suitable single-skill and multi-skill configurations, synthesizes tasks through rule-based and agent-based verification with feedback-guided repair, and retains only successful trajectories that substantively use every required skill. Using 2,000 public skills, SKT produces 4,000 task packages and 27,164 verified trajectories. Based on the same pipeline and a disjoint task pool, we further construct SkillEval, a held-out executable benchmark for evaluating skill use. Experiments across diverse models, benchmarks, and agent harnesses show that supervised fine-tuning on SKT-generated trajectories consistently improves skill-use performance. Verification ablations, cross-harness evaluation, and scaling experiments further show that these gains depend on high-quality supervision, extend beyond a single agent interface, and increase with broader skill coverage. Together, these results establish verified data synthesis as an effective and scalable approach to skill-use training.
> Hugging Face: https://huggingface.co/datasets/Artemis0430/skilleval-v1
> Contact: {zhangchen1,bailei}@pjlab.org.cn

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：当前语言模型智能体在拥有大量 Agent Skills（可复用的程序性知识模块）的情况下，仍然不能可靠地完成“技能使用（skill use）”任务，即无法稳定地识别在什么场景下该用哪个技能、如何正确调用技能、以及如何在多技能任务中协调多个技能的调用顺序与参数。作者指出“提供技能本身并不能保证模型会使用技能”，这指向一个关键的训练-推理错位：技能库是静态的外部资源，而模型内部的程序性知识（何时、为何、如何调用）是缺失的。

具体而言，问题可以拆解为三个层次：
1. 技能识别（skill identification）：模型需要从大量技能中判断当前任务需要哪个或哪些技能；
2. 技能应用（skill application）：模型需要按照技能的接口规范正确填充参数、组织调用；
3. 技能协同（skill coordination）：在需要多个技能配合的任务中，模型需要规划技能的执行顺序、处理技能间的依赖与信息传递。

现有方法通常只提供技能描述或示例，缺乏大规模、可验证、覆盖多技能组合的训练信号。真实用户任务标注成本高，且难以保证覆盖技能组合的多样性；而普通合成数据又容易产生不可执行、不忠实于技能语义的轨迹。因此，核心难点在于如何低成本地生成大量“既真实可执行、又严格约束在指定技能上”的训练数据，并且能够自动验证轨迹的正确性，避免模型从错误轨迹中学习。

此外，该问题还隐含一个评估困境：训练数据与评测数据若同源，容易高估模型能力；需要构建与训练数据来源不重叠的 held-out 基准，才能可信地衡量技能使用能力。论文因此同时关注数据生成方法和评测方法，属于“数据-评测一体化”的系统性问题。

Q2: 有哪些相关研究？

根据摘要和检索证据，本文的相关研究背景可以归纳为以下几个方向：
1. **Agent Skills 与工具使用**：Agent Skills 作为可复用程序性知识的载体（如 Claude 的 Agent Skills、各类技能库），已有大量工作致力于构建技能库、技能检索和技能编排，但多数工作默认模型已经具备使用技能的能力，本文指出这一假设不成立。
2. **合成数据用于智能体训练**：利用 LLM 生成训练轨迹（如 self-instruct、GPT-4 生成 agent 轨迹）是常见做法，但缺乏对轨迹正确性的强验证，导致模型学习到幻觉或不一致行为。SKT 的关键区别在于引入“验证+修复”闭环，只保留通过验证的轨迹。
3. **可执行基准（executable benchmarks）**：已有 SkillsBench（Li et al., 2026b）、AgentSkillOS-bench（Li et al., 2026a）、MolBench-Bind（Zhang et al., 2026）等基准评测智能体的技能使用，但这些基准或覆盖有限、或与训练数据同源，SKT 提出用同一流水线在不相交技能池上构建 SkillEval，降低数据泄漏风险。
4. **验证驱动的方法**：基于规则验证和基于智能体验证（LLM-as-judge / verifier）在代码生成、数学推理中已有应用，本文将其系统性地引入技能使用轨迹的过滤与修复。
5. **多技能组合与组合泛化**：相关研究探索了任务组合、子目标分解和工具组合，但多技能协同训练数据稀缺，SKT 通过多技能配置合成任务来补足这一块。

注意：由于检索证据只给出了少量片段，上述相关工作的具体名称和结论细节来自摘要中引用的文献名（Li et al., 2026a/b 等），具体方法对比需要阅读原文才能完整掌握。

Q3: 论文如何解决这个问题？

SKT 是一个三阶段的数据合成流水线，具体结构如下（基于摘要和结论片段，部分细节为合理推断）：

**阶段一：技能选择（Skill Selection）**
从大规模技能库（如 2,000 个公开技能）中筛选适合用于任务合成的技能，并且考虑技能间的组合性、多样性和可组合性，构建一个高质量、覆盖全面的技能池。选择时会权衡技能的独立性、接口清晰度和可验证性，以便后续生成的任务既能充分使用技能，又能被自动验证。

**阶段二：任务合成与验证修复（Task Synthesis with Verification and Repair）**
基于选定技能池，SKT 合成“技能约束”任务（skill-grounded tasks），任务复杂度可控，涵盖从单技能场景到多技能组合场景。每个任务不仅包含结构化的规格说明（如目标、约束、技能要求），还包含可执行的参考轨迹。合成过程采用“规则验证 + 智能体验证”双通道：
- 规则验证：检查任务/轨迹是否符合预设的语法和逻辑规则（如技能调用格式、参数类型、必需技能是否被调用）；
- 智能体验证：利用 LLM 作为验证器，判断任务是否可解、轨迹是否忠实于技能语义、是否真正使用了所有必需技能。
- 反馈引导修复（feedback-guided repair）：当验证失败时，将验证器给出的错误反馈回送给生成器，对任务或轨迹进行修复，再重新验证，形成“合成-验证-修复”循环。

**阶段三：轨迹过滤（Trajectory Filtering）**
只保留通过验证的、成功执行且“实质性地使用了每个必需技能”的轨迹。这里的“实质性使用”意味着技能调用不是表面提及而是真实参与了任务完成过程，从而保证训练信号的高质量和忠实度。

**产出规模**：基于 2,000 个技能，生成 4,000 个任务包（task packages）和 27,164 条已验证轨迹。任务包可能包含同一任务的不同变体或配套评测脚本。

**SkillEval 构建**：利用同一 SKT 流水线，但从一个与训练任务池不相交（disjoint）的技能池/任务池中构建 held-out 可执行基准，用于评估模型在未见技能组合上的泛化能力。

**训练与评测**：使用 SKT 生成的轨迹对多种基础模型进行监督微调（SFT），然后在 SkillsBench、MolBench-Bind、AgentSkillOS-bench 和 SkillEval 四个基准上评测。评测使用多种 agent harness（智能体框架），以检验跨接口的泛化性。

注意：论文正文方法部分的图（images/a5d792...jpg）未在文本证据中呈现，阶段细节（如任务表示、验证规则的具体形式、修复提示模板）需要查阅原文图 1 和方法章节。

Q4: 论文做了哪些实验？

根据摘要和检索到的实验相关证据，论文的实验设计如下：

1. **训练数据**：使用 SKT 流水线从 2,000 个公开技能生成 4,000 个任务包和 27,164 条已验证轨迹，用于监督微调。
2. **评测基准**：在四个基准上评估技能使用能力：
 - SkillsBench（Li et al., 2026b）
 - MolBench-Bind（Zhang et al., 2026）
 - AgentSkillOS-bench（Li et al., 2026a）
 - SkillEval（本文构建，使用 SKT 从 held-out 技能池生成）
3. **模型与框架**：实验覆盖“diverse models”（多种模型）和“agent harnesses”（多种智能体框架），说明论文对不同基础模型（可能是不同规模或不同系列的 LLM）和不同 agent 接口/框架都进行了测试。
4. **验证消融（Verification ablations）**：通过对比有无验证/修复环节的合成数据训练效果，验证“高质量监督”对技能使用提升的因果贡献。
5. **跨框架评测（Cross-harness evaluation）**：在一种框架下训练的模型，迁移到其他 agent harness 上评测，检验技能使用能力是否泛化到不同接口。
6. **规模扩展实验（Scaling experiments）**：改变技能覆盖范围（如使用不同数量的技能或任务包），观察性能变化趋势，验证“数据规模/技能覆盖面扩大时性能持续提升”的假设。

具体数值结果、baseline 模型名称、微调超参数等在检索证据中未提供，需要查阅原文实验部分。

Q5: 发现了什么实验现象？

基于摘要和证据片段，论文报告的主要实验现象包括：
1. **一致性提升**：在不同模型、不同基准、不同 agent harness 上，SKT 轨迹微调一致地提升了技能使用性能，说明该方法具有跨设置稳健性。
2. **验证消融的正效应**：去除验证或修复环节后性能下降，表明“只保留高质量已验证轨迹”是增益的关键因素，而不是简单的合成数据数量。
3. **跨框架迁移**：在一种 agent harness 上训练得到的技能使用能力可以迁移到其他接口，说明模型学到的是相对通用的技能调用能力，而非对特定框架的过拟合。
4. **规模扩展趋势**：随着技能覆盖范围扩大，性能进一步提升，提示数据多样性（而非单纯数量）的重要性。
5. **SkillEval 上的表现**：在 held-out 任务池构建的基准上模型仍有提升，说明增益不是来自与训练数据同源的泄漏，而是真正的技能使用泛化。

注意：这些是摘要内明确表述的现象；具体数值（如提升几个百分点）、失败案例和指标间张力在检索证据中未出现，需要从原文图表中获取。

Q6: 有什么可以进一步探索的点？

基于论文的设定和现有结果，可进一步探索的方向包括：
1. **更大规模技能池**：当前使用 2,000 个技能，扩展到数万甚至数百万技能时，流水线的可扩展性、任务多样性以及技能选择策略需要进一步优化。
2. **更复杂的技能组合**：当前任务涵盖单技能和多技能，但多技能协同的复杂度上限（如需要 5 个以上技能、技能间有严格依赖或并行关系）值得探索。
3. **验证器的自动化和可靠性**：基于 LLM 的验证器本身可能出错，如何构建更可靠的验证器（如引入可执行环境、更细粒度的规则、多验证器投票）是重要方向。
4. **从 SFT 到偏好优化**：将 SKT 的验证信号引入 RLHF/DPO 等偏好优化，或直接作为 reward model 训练数据，进一步提升技能使用能力。
5. **在线学习和技能库演化**：当技能库新增或更新技能时，如何增量生成训练数据并避免灾难性遗忘。
6. **跨领域和跨语言泛化**：当前技能池可能偏重通用数字任务；探索科学领域（如生物、化学）或低资源语言的技能使用训练。
7. **评测基准的扩展**：SkillEval 目前与训练数据同源（但任务池不相交），是否可以用真实用户任务或人工标注构建更接近真实分布的基准。
8. **技能使用与规划/推理的交互**：技能使用能力与模型底层推理能力（如长程规划、错误恢复）的关系，以及是否可以用 SKT 数据增强这些能力。
9. **验证信号的可解释性**：分析验证器拒绝/修复轨迹的主要原因，从而发现技能库或任务合成中的系统性缺陷。

这些方向属于合理推断，论文本身未明确列出 future work 列表。

Q7: 总结一下论文的主要内容

本文针对语言模型智能体的“技能使用”能力不足问题，提出了一个名为 SKT（Skill-Use Training via Verified Synthetic Data Generation）的验证式数据合成流水线，并基于此构建了训练数据集和 held-out 基准 SkillEval。

**论证主线**：作者首先指出，Agent Skills 作为可复用程序性知识的外化载体虽然越来越普遍，但模型自身并没有内化“何时、如何、为何使用技能”的程序性知识。单纯扩充技能库或在 prompt 中附加技能描述，并不能让模型可靠地识别、应用和协同技能。因此需要直接面向“技能使用”这一行为进行训练。

**技术主线**：SKT 流水线包含三个关键环节。第一，技能选择：从 2,000 个公开技能中挑选适合任务合成的技能，并考虑单技能与多技能配置的多样性、组合性和可验证性。第二，任务合成与验证修复：生成技能约束的任务（包括结构化规格说明），通过规则验证和智能体验证来判断任务是否可解、轨迹是否正确，并利用验证反馈进行迭代修复，直到通过验证。这一环节强调“反馈引导修复”，显著区别于一次性合成后简单过滤的做法。第三，轨迹过滤：只保留成功执行且实质性使用每一个必需技能的轨迹，确保训练信号的高质量。最终得到 4,000 个任务包和 27,164 条已验证轨迹。

为验证方法的有效性，作者使用同一流水线但构建一个与训练任务池不相交的任务池，生成 SkillEval 基准，避免数据泄漏。随后在 SkillsBench、MolBench-Bind、AgentSkillOS-bench 和 SkillEval 四个基准上，对多种基础模型和多种 agent harness 进行监督微调评估。实验结果显示一致性的性能提升。验证消融实验证明“验证+修复”是增益的关键；跨 harness 评估说明能力泛化不局限于特定接口；规模扩展实验显示扩大技能覆盖范围能进一步提高性能。

**实验主线**：论文通过四个维度的实验构建证据链：
- 主实验：多模型 + 多基准 + 多 harness 的 SFT 效果；
- 消融：验证环节有无的对比；
- 迁移：跨 harness 的评估；
- 规模：技能覆盖范围的 scaling 趋势。

**结论**：验证式数据合成是一种有效且可扩展的技能使用训练方法。本文的主要贡献包括：（1）提出 SKT 流水线，支持可解性、可验证性和复杂度控制；（2）构建 SkillEval held-out 基准；（3）通过大量实验证明方法的有效性和泛化性。

总体而言，本文属于“数据为中心”的系统性工作，将合成数据生成、验证机制和评测构建统一在同一框架下，为后续 Agent 技能使用训练提供了可复用的基础设施。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent 方向直接相关，系统性地解决了智能体训练中技能使用数据稀缺的问题。

## 基本信息

- 作者：Zelin Tan, Yiqun Zhang, Hao Li, Zhiyao Cui, Hejia Geng, Shao Zhang, Hangfan Zhang, Yang Chen, Xiaosong Wang, Lilong Wang, Zhenfei Yin, Shuyue Hu, Chen Zhang, Lei Bai
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.02287v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要基于论文摘要和少量检索到的片段（Abstract、Conclusion、Introduction），方法细节和实验数值证据不足，已结合合理推断补充并标注。
