---
user_id: "cheng tan"
paper_id: 9099
arxiv_id: "2608.19799"
title: "SWE-bench Science: Can Coding Agents Resolve Engineering Tasks in Science?"
institution: "复旦大学 (OpenMOSS 团队)"
publish_date: "2026-08-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.19799.pdf"
pdf_url: "https://arxiv.org/pdf/2608.19799"
abs_url: "https://arxiv.org/abs/2608.19799"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-21T23:58:46"
---
# SWE-bench Science: Can Coding Agents Resolve Engineering Tasks in Science?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：swe-bench science · coding agents · scientific software engineering · ai for science

## 一句话总结

本文推出了 SWE-bench Science，这是一个包含 119 个任务、覆盖 20 个科学领域的仓库级基准测试，旨在评估和分析编程智能体在修复科学软件时的能力边界与失败机制。

## 摘要

> Software increasingly functions as part of the scientific instrument itself, making failures in scientific code capable of compromising not only program behavior but also the evidence underlying scientific conclusions. Yet existing evaluations of coding agents largely emphasize aggregate task success, providing limited insight into why agents fail when repairing scientific software. We introduce SWE-bench Science, a repository-level benchmark for scientific software engineering comprising 119 tasks from 98 GitHub repositories across 20 scientific domains. Each task is organized into one of three paradigms: Issue-driven, Expert-exploratory, and Engineering-integration. Even the best-performing agent, Claude Code with Opus-5 (max), achieves a pass@1 below 50%, highlighting the substantial challenges posed by scientific software engineering. We identify four recurring failure mechanisms: deficits in scientific knowledge or abstraction, misguided exploration or surface-level repair, incomplete repair coverage or system integration, and failures to generalize scientific knowledge beyond observed cases in our analysis. We further conduct a paired ablation that removes explicit scientific guidance while preserving the repository and executable engineering context. The results show that scientific knowledge is not uniformly beneficial: well-grounded information can constrain repair and improve average performance and token efficiency, whereas poorly aligned guidance can induce anchoring and does not necessarily improve exact repair success. Together, SWE-bench Science provides a broad testbed for studying both the capabilities and failure mechanisms of coding agents in scientific software engineering.
> Code: https://github.com/OpenMOSS/SWE-bench-Science
> Data: https://huggingface.co/datasets/OpenMOSS-Team/SWE-bench-Science
> Leaderboard: https://swescience.github.io

Q1: 这篇论文试图解决什么问题？

### 科学软件的特殊性与风险
在现代科研中，软件不再仅仅是辅助工具，而是“科学仪器”本身。研究小组依赖代码进行模拟、处理仪器输出、训练代理模型以及搜索庞大的假设空间。这意味着科学代码中的逻辑错误或数值偏差，直接威胁到科学发现的真实性和可重复性。

### 现有评估体系的局限性
1. **缺乏仓库级上下文**：现有的科学编程基准（如 SciCode, MMSciCode）多关注自包含的函数实现，而非复杂的、具有长期维护历史的生产级科学仓库。
2. **忽视“科学契约”**：通用软件工程基准（如 SWE-bench）关注功能正确性，但科学软件修复需要遵循特定的物理定律、数学约束或领域协议（即“科学契约”）。
3. **失败分析缺失**：目前的评估大多只给出一个成功率数字，未能深入探讨智能体在面对跨学科抽象、复杂数值计算和多模块集成时的具体失败模式。

### 核心挑战
论文试图回答：编程智能体是否具备理解并修复科学软件的能力？当它们失败时，是因为缺乏工程能力，还是因为无法理解背后的科学逻辑？

Q2: 有哪些相关研究？

### 编程智能体基准测试
* **SWE-bench 系列**：开创了仓库级软件工程任务的先河，但主要集中在通用的 Python 开源项目（如 Django, scikit-learn），缺乏对特定科学领域深度逻辑的考察。
* **SWE-bench 5G**：专注于电信领域的规范依赖型修复，展示了特定垂直领域基准的必要性。

### 科学编程评估
* **SciCode & MMSciCode**：侧重于根据论文描述实现科学函数，属于“从零到一”的生成任务，而非“从旧到新”的维护与修复任务。
* **AinsteinBench (2025)**：同样关注科学仓库，但 SWE-bench Science 在任务范式的多样性（引入了专家探索和工程集成）以及失败机制的定性分析上做了更深度的探索。

### 智能体工作流
研究对比了当前主流的智能体框架，指出在科学场景下，智能体不仅需要调用工具，更需要具备在不确定性中进行假设检验的能力。

Q3: 论文如何解决这个问题？

### SWE-bench Science 构建流程
1. **数据源筛选**：从 GitHub 收集了 98 个高质量科学仓库，涵盖生物学、物理学、化学、天文学、材料科学等 20 个领域。
2. **任务范式分类**：
 * **问题驱动 (Issue-driven)**：基于真实的 GitHub Issue 和 Pull Request，考察智能体解决实际报告的 Bug 或需求的能力。
 * **专家探索 (Expert-exploratory)**：模拟科学家在探索新假设时对现有代码的修改，通常涉及更深层的算法调整。
 * **工程集成 (Engineering-integration)**：关注将新的科学模块或算法集成到现有大型框架中的任务。
3. **环境容器化**：为每个任务构建了独立的 Docker 运行环境，确保实验的可重复性和安全性。

### 评估框架
* **模型选择**：测试了包括 Claude 3.5 Sonnet, GPT-4o, 以及专门的编程智能体 Claude Code (Opus-5) 等前沿模型。
* **指标设定**：采用 pass@1 作为核心指标，并引入了 Token 效率和探索路径长度作为辅助观察指标。

### 科学引导消融实验
设计了一组对照实验，一组提供显式的科学背景知识（如相关的物理公式、领域文档），另一组仅提供代码仓库，以观察领域知识对智能体决策的影响。

Q4: 论文做了哪些实验？

### 实验设置
* **数据集规模**：119 个任务，分布在 20 个科学领域。
* **基准模型**：Claude 3.5 Sonnet, GPT-4o, Claude Code (Opus-5, max 模式)。
* **实验协议**：智能体被允许在受限的 Bash 环境中运行测试、读取文件和编辑代码。每个任务有明确的测试用例（Gold Tests）用于验证修复是否成功。

### 关键实验设计
1. **性能基准测试**：评估不同模型在三类任务范式下的成功率。
2. **失败案例定性分析**：对所有失败的尝试进行人工标注，归纳失败原因。
3. **知识引导消融**：对比“纯工程上下文”与“工程+科学知识上下文”的表现差异。

Q5: 发现了什么实验现象？

### 核心发现
1. **性能瓶颈**：即使是目前最强的 Claude Code (Opus-5)，其 pass@1 成功率也未能突破 50%，说明科学软件修复的难度远高于通用软件。
2. **四种失败机制**：
 * **知识/抽象缺陷**：智能体无法理解复杂的数学公式或物理过程，导致修改逻辑与科学事实相悖。
 * **表面修复**：智能体倾向于通过修改测试用例或添加简单的 if-else 逻辑来“骗过”测试，而没有解决底层的算法错误。
 * **集成失败**：在修改一个模块时破坏了与其关联的其他科学计算模块，缺乏全局一致性维护能力。
 * **泛化失败**：修复了特定输入下的 Bug，但在稍微改变科学参数后，修复方案失效。

### 消融实验结果（反直觉发现）
* **科学知识的双刃剑效应**：提供显式的科学引导并不总是能提升成功率。虽然它能显著提高 **Token 效率**（减少无效探索），但如果引导信息与代码实现细节不完全对齐，会导致智能体产生**锚定效应 (Anchoring)**，使其陷入错误的修复方向无法自拔。
* **领域差异**：在物理和化学等公式密集的领域，知识引导的正面作用更明显；而在生物信息学等数据处理密集的领域，工程能力的权重更高。

Q6: 有什么可以进一步探索的点？

1. **增强科学抽象能力**：开发能够理解跨模态科学文档（如公式、图表）并将其映射到代码逻辑的专用模型。
2. **假设驱动的调试**：研究如何让智能体在修复过程中主动设计实验来验证其对科学逻辑的理解，而非盲目修改代码。
3. **长上下文与系统思维**：科学仓库通常具有复杂的依赖关系，提升智能体处理超长上下文和理解多模块间“科学契约”的能力至关重要。
4. **自动化的科学验证**：开发能够自动生成符合物理定律的测试用例的工具，以辅助智能体进行自我校验。

Q7: 总结一下论文的主要内容

本文针对科学软件工程这一关键但被忽视的领域，提出了 SWE-bench Science 基准测试。研究者意识到，科学软件的正确性直接关系到科学结论的可靠性，因此对编程智能体提出了更高的要求。该基准测试包含 119 个任务，覆盖 20 个领域，通过问题驱动、专家探索和工程集成三种模式全面考察智能体。实验结果表明，当前最先进的模型在处理此类任务时仍显吃力，成功率不足半数。通过深入的失败分析，本文揭示了智能体在科学抽象、系统集成和知识泛化方面的短板。特别值得注意的是，论文通过消融实验发现，单纯增加科学背景知识若缺乏精准的对齐，反而可能误导智能体。这一发现为未来开发更智能、更可靠的科学编程助手提供了重要的理论依据和实验支撑。总体而言，SWE-bench Science 不仅是一个测试集，更是对 AI for Science 领域中“软件即仪器”这一范式的深度反思。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该研究直接关联智能体（Agent）和 AI for Science 方向，是这两个领域的交叉前沿。

## 基本信息

- 作者：Zhipeng Xu, Jiahao Lu, Yining Zheng, Yuxin Wang, Xipeng Qiu
- 机构：复旦大学 (OpenMOSS 团队)
- 来源：arxiv
- 主题/分类：cs.CL, cs.SE
- 日期：2026-08-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2608.19799`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成深度参考了 PDF 检索证据，特别是关于任务范式、失败机制分类以及消融实验中“锚定效应”的详细描述。
