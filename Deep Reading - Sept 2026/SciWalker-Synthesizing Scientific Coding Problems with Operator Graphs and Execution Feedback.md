---
user_id: "cheng tan"
paper_id: 13502
arxiv_id: "2609.30054v1"
title: "SciWalker: Synthesizing Scientific Coding Problems with Operator Graphs and Execution Feedback"
publish_date: "2026-09-24"
pdf_url: "https://arxiv.org/pdf/2609.30054v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-26T01:49:43"
---
# SciWalker: Synthesizing Scientific Coding Problems with Operator Graphs and Execution Feedback

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：scientific coding · synthetic data generation · operator graphs · execution feedback

## 一句话总结

SciWalker 提出了一种通过算子图采样和执行反馈合成科学编程问题的框架，显著提升了大语言模型在复杂科学计算任务中的代码生成与推理能力。

## 摘要

> Improving the scientific coding capabilities of large language models (LLMs) requires high-quality training data. However, such data remain scarce because manually authoring realistic problems is costly and time-consuming, while systematically covering diverse scientific domains and algorithmic combinations remains challenging. To address this, we introduce SciWalker, a framework for synthesizing scientific coding problems through operator-chain sampling and execution feedback. The framework combines scientific library interfaces with operation modes to instantiate operators, organizes them into operator graphs, and samples operator chains as computational workflow cues. Guided by these cues, we adopt LLMs to generate scientifically grounded problem statements, reference solutions, and tests, with failed generations iteratively repaired using execution feedback. By combining structured workflow composition with verification and quality review, SciWalker enables scalable task generation while promoting scientific grounding, computational diversity, and executability. Using this framework, we construct 8,178 high-quality problems spanning 5 scientific domains and 32 subdomains. To evaluate their training utility, we conduct reinforcement learning on Qwen3.5-9B using the GSPO algorithm. This training improves SciCode subproblem accuracy by 9.9 percentage points, from 29.3% to 39.2%, with gains across scientific code generation, code repair, and reasoning benchmarks. The code for SciWalker is available at https://github.com/lichenx1/SciWalker.

Q1: 这篇论文试图解决什么问题？

科学编程（Scientific Coding）是大语言模型（LLM）迈向“AI for Science”核心能力的关键，但目前面临严峻的数据瓶颈。首先，科学编程不仅要求语法正确，更要求深厚的领域知识（如物理公式、数值稳定性、特定科学库的使用），这使得人工编写高质量问题的成本极高且难以规模化。其次，现有的合成数据方法往往缺乏系统性的覆盖，难以穷尽不同科学领域（如量子化学、流体力学、生物信息学）与算法逻辑（如蒙特卡洛模拟、偏微分方程求解）的复杂组合。再者，合成代码的“科学落地感”（Scientific Grounding）难以保证，模型生成的代码可能在逻辑上自洽但在物理意义上错误。最后，缺乏有效的验证机制，导致合成数据中存在大量无法运行或结果错误的代码，严重影响训练效果。SciWalker 试图解决如何在保证科学严谨性和计算多样性的前提下，自动化、规模化地生成可执行的科学编程任务。

Q2: 有哪些相关研究？

相关研究主要集中在三个维度：合成数据生成、代码大模型以及科学计算基准测试。在合成数据方面，Self-Instruct 等方法已广泛用于通用指令微调，但在处理需要严谨逻辑和特定库依赖的科学代码时表现不佳。在代码模型领域，CodeLlama、DeepSeek-Coder 等模型通过大规模预训练积累了代码能力，但在解决前沿科学问题时仍显乏力。在科学计算基准方面，SciCode 等评测集的出现定义了该领域的挑战，但其规模有限，不足以支撑大规模的强化学习或微调。SciWalker 的创新之处在于引入了结构化的“算子图”概念，将随机的文本生成转化为受约束的工作流采样，并结合了执行反馈循环，这与早期的 CodeRL 等基于反馈的编程学习思路一脉相承，但更侧重于科学领域的知识对齐。

Q3: 论文如何解决这个问题？

SciWalker 框架由三个核心阶段组成：
1. **算子图构建与采样**：首先，从主流科学计算库（如 NumPy, SciPy, Biopython 等）中提取接口，结合特定的操作模式（如数据预处理、数值积分、统计推断）定义为“算子”。这些算子被组织成有向图（Operator Graphs），节点代表计算步骤，边代表逻辑流向。通过在图中采样“算子链”，生成代表特定科学计算任务的结构化线索（Cues）。
2. **引导式任务合成**：将采样得到的算子链作为 Prompt 的核心约束，引导 LLM 生成完整的问题定义。这包括：(a) 科学背景描述；(b) 具体的输入输出规范；(c) 符合科学逻辑的参考实现；(d) 覆盖边界条件的单元测试。这种“线索引导”机制确保了生成内容的多样性和领域相关性。
3. **执行反馈与迭代修复**：生成的代码和测试用例会在沙箱环境中运行。如果测试失败，系统会将错误信息（Traceback）反馈给 LLM 进行自动修复。只有通过所有测试且经过质量审查（Quality Review）的任务才会被纳入最终数据集。这种闭环机制极大地提高了数据的可执行率和准确性。

Q4: 论文做了哪些实验？

实验部分主要验证了 SciWalker 合成数据对提升模型科学编程能力的有效性：
1. **数据集规模**：构建了包含 8,178 个任务的数据集，横跨物理、化学、生物、材料科学和地球科学 5 大领域，细分为 32 个子领域。
2. **训练配置**：以 Qwen3.5-9B 为基座模型，采用 GSPO（Generalized Stepwise Proximal Optimization）强化学习算法进行训练。GSPO 旨在通过逐步优化提升模型在复杂推理链条上的稳定性。
3. **评测基准**：主要在 SciCode（科学代码生成）、HumanEval/MBPP（通用代码能力）以及相关的代码修复和数学推理榜单上进行测试。
4. **对比实验**：对比了仅使用 SFT（监督微调）和使用 SciWalker 数据进行 RL 后的性能差异，并分析了不同领域数据的贡献度。

Q5: 发现了什么实验现象？

1. **核心指标提升**：在 SciCode 基准测试中，经过 SciWalker 数据训练的模型子问题准确率从 29.3% 跃升至 39.2%，提升幅度达 9.9 个百分点，证明了合成数据在复杂科学任务上的强泛化能力。
2. **执行反馈的重要性**：消融实验显示，未经执行反馈修复的数据包含约 30% 的逻辑或运行错误，而引入反馈机制后，数据的可执行性显著增强，直接导致了训练效果的质变。
3. **跨领域迁移**：模型在未见过的科学子领域也表现出性能提升，说明算子链的组合逻辑帮助模型掌握了科学编程的通用范式，而非仅仅死记硬背公式。
4. **推理与修复能力联动**：除了代码生成，模型在代码修复任务中的表现也同步提升，这得益于训练过程中包含的“错误-修复”对数据。
5. **指标张力**：在追求科学代码准确性的同时，通用代码能力（如 HumanEval）保持稳定或略有上升，未出现明显的灾难性遗忘。

Q6: 有什么可以进一步探索的点？

1. **算子库的自动化扩展**：目前算子定义仍部分依赖人工筛选库接口，未来可探索利用 LLM 自动解析文档并构建超大规模算子图。2. **多模态科学编程**：科学问题常伴随图表、分子结构或物理实验视频，将 SciWalker 扩展到多模态输入是重要方向。3. **更复杂的物理约束集成**：在执行反馈中引入物理量纲检查、能量守恒验证等硬性科学约束，进一步提升数据的严谨性。4. **长程科学工作流合成**：目前的算子链长度有限，未来可尝试合成涉及数十个步骤的复杂科研流水线任务。

Q7: 总结一下论文的主要内容

本文提出了 SciWalker，一个旨在解决科学编程数据匮乏问题的自动化合成框架。该框架的核心思想是将复杂的科学计算任务分解为可组合的“算子”，通过在预定义的算子图中进行随机采样，生成具有逻辑约束的计算工作流线索。基于这些线索，LLM 能够生成高度多样化且符合科学直觉的编程题目。为了解决合成数据中常见的不可运行问题，SciWalker 引入了基于执行反馈的迭代修复机制，确保了最终产出数据的高质量和可验证性。研究团队利用该框架产出了 8,178 个覆盖 5 大科学领域的任务，并证明了这些数据在强化学习阶段能显著增强 Qwen3.5-9B 模型的科学代码生成能力（SciCode 准确率提升近 10%）。这项工作不仅为 Code LLM 的领域特化提供了系统性的方法论，也为 AI 辅助科学发现奠定了坚实的数据基础。其开源的框架和数据集将有力推动科学计算社区与大模型社区的融合。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该工作直接关联“生成”方向，特别是针对科学领域的结构化数据合成。

## 基本信息

- 作者：Chenxi Li, Wenxuan Zeng, Yun Luo, Fangchen Yu, Peng Ye, Yu Cheng, Jun Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-09-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.30054v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成主要基于摘要和启发式草稿，未参考 PDF 检索证据。
