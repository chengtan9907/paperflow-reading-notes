---
user_id: "cheng tan"
paper_id: 3173
arxiv_id: "2607.07779"
title: "From Solvers to Research: Large Language Model-Driven Formal Mathematics at the Research Frontier"
publish_date: "2026-07-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.07779.pdf"
pdf_url: "https://arxiv.org/pdf/2607.07779"
abs_url: "https://arxiv.org/abs/2607.07779"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-11T00:18:54"
---
# From Solvers to Research: Large Language Model-Driven Formal Mathematics at the Research Frontier

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language model · formal mathematics · interactive theorem proving · AI for mathematics

## 一句话总结

本文主张AI4Math系统应从解决预定义问题的求解器转向能够处理开放式研究数学的形式推理研究代理，并系统回顾了现有数据集、自动形式化和证明合成方法，分析了核心局限性，提出了未来路线图。

## 摘要

> Recent developments in AI for Mathematics (AI4Math), especially Large Language Model (LLM)-driven theorem provers, has achieved remarkable success in formal proof generation for well-defined mathematical problems through Interactive Theorem Proving (ITP) languages. However, current systems remain fundamentally limited in tackling frontier research mathematics, such as discovering new theorems or resolving open conjectures, which are often open-ended, under-specified, and involve multiple layers of abstraction. We argue that the next leap in AI4Math systems requires a decisive shift from predefined problem-solvers to research agents that can address frontier mathematical challenges with rigorous formal mathematical reasoning. In this position paper, we provide a systematic review of the field, covering datasets, auto-formalization, and proof synthesis. More importantly, we identify core limitations of existing systems in serving as mathematical research agents, examining issues across datasets, relational structure, mathematical exploration, tool ecosystem, and human-AI collaboration, outlining a strategic road-map for the future of AI4Math.

Q1: 这篇论文试图解决什么问题？

当前LLM驱动的形式定理证明器在预定义问题（如国际数学奥林匹克问题）上表现出色，但在前沿数学研究任务如发现新定理、解决开放猜想方面能力严重不足。这些任务具有开放性、未完全指定性和多层抽象性，超出当前系统能力。本文旨在分析这一差距的根本原因，并倡导从'问题求解器'向'研究代理'的范式转变，为实现这一转变提供系统性的回顾和战略路线图。具体而言，论文指出当前系统在五个方面存在核心局限：形式数学数据覆盖不足且结构单一、缺乏数学命题间关系建模、探索交互环境缺失、工具生态系统碎片化、人机协作机制不成熟。

Q2: 有哪些相关研究？

论文系统回顾了AI4Math领域的相关工作，涵盖三大方面：形式数学数据集（如MiniF2F、ProofNet、Mathlib等）、自动形式化方法（将自然语言数学转化为形式代码，如使用LLM进行形式化翻译）、以及证明合成技术（包括基于搜索的方法、利用神经网络的证明生成、以及结合两者）。此外，论文还讨论了现有系统在数学研究代理方面的尝试与不足，例如一些工作尝试将LLM与证明搜索结合，但效果仅限于预定义问题。论文也提及了自动形式化中的挑战，如自然语言歧义、形式化步骤缺失等。最后，论文指出相关领域如程序合成和交互式证明辅助工具的发展为未来研究代理奠定了基础。

Q3: 论文如何解决这个问题？

本文作为立场论文，没有提出具体的新算法或系统，而是通过系统性回顾和批判性分析，提出了一套从求解器向研究代理转变的战略框架，具体包括五大支柱：1) 形式数学数据的扩展与结构优化：需要更大规模、覆盖前沿数学的数据集，并引入关系结构（如引理依赖、数学概念层次）以支持推理；2) 数学命题间关系结构的建模：构建知识图谱或关系网络，使代理能够理解数学概念间的依赖和类比；3) 支持数学探索的交互式环境：开发允许人类数学家与AI协同探索、假设检验、可视化证明的交互平台；4) 统一的工具生态系统：整合现有形式化工具（如Lean、Coq、Isabelle）并提供一致接口，降低使用门槛；5) 高效的人机协作设计：明确人类在抽象概念和直觉方面、AI在搜索和验证方面的互补角色，设计混合智能系统。论文强调这五方面共同构成了实现研究代理的路线图，并建议社区优先推进端到端研究工具链的构建。

Q4: 论文做了哪些实验？

本文为立场论文，未提出新的实验设置或开展新的实验验证。但论文在回顾部分综合分析了现有系统的实验表现，例如：LLM定理证明器在IMO形式化问题上的成功案例，以及它们在开放问题（如Erdős猜想）上的失败尝试。论文引用了一些具体例子说明当前系统在需要创造性推理或多步抽象的问题上表现不佳。此外，论文还讨论了自动形式化评测的结果，指出形式化过程中存在的忠实性问题。总体而言，实验证据来自于文献中的已有工作，本文未进行新实验。

Q5: 发现了什么实验现象？

由于本文为立场论文，无新实验观察。但通过文献综合分析，论文揭示了以下现象：1) 当前最先进的LLM证明器在标准基准（如MiniF2F）上取得较高准确率，但在开放性问题（如千年难题、Erdős问题）上几乎无法产生有效证明；2) 自动形式化步骤常引入歧义或丢失原始数学思想，影响证明的正确性；3) 证明搜索过程缺乏对数学直觉的引导，导致在复杂问题上搜索空间爆炸；4) 人类与AI协作的初步尝试表明，人类提供的高层策略能显著提升证明成功率，但现有接口和工具支持不足。这些观察突出了从求解器转向研究代理的必要性。

Q6: 有什么可以进一步探索的点？

论文基于核心局限分析，提出了多个未来探索方向：1) 开发端到端的研究支持工具，从猜想生成、自动形式化、证明搜索到人机协作验证；2) 构建更全面、结构化、覆盖前沿数学的形式数据集，特别是包含未解决问题列表；3) 研究自动形式化的忠实性改进，结合交互式校对；4) 探索关系型数学知识表示，支持类比推理和引理重用；5) 设计支持渐进式探索的数学实验环境；6) 推动形式化工具的统一化和易用化；7) 深入人机协作模式，研究如何分配创造性任务与形式验证任务；8) 研究AI的数学创造力，包括概念理解和抽象能力。论文强调这些方向需要跨领域合作，包括数学家、计算机科学家和认知科学家。

Q7: 总结一下论文的主要内容

本文是一篇关于AI4Math的立场论文，核心论点是当前AI4Math系统虽然已经在预定义的形式数学问题（如IMO问题）上取得显著成功，但距离成为能够辅助前沿数学研究的'研究代理'仍有根本性差距。论文主张AI4Math的下一步发展应该从'问题求解器'转向'研究代理'，即能够参与开放性、未完全指定的数学探索与发现的系统。

论文首先回顾了AI4Math领域的三个核心组成部分：形式数学数据集、自动形式化技术以及证明合成方法。在数据集方面，现有资源如MiniF2F、ProofNet等主要覆盖标准竞赛问题和经典定理，缺乏对研究前沿的覆盖；自动形式化方面，LLM展示了从自然语言翻译为形式代码的能力，但存在歧义、忠实性和完整性挑战；证明合成方面，结合搜索和LLM生成的方法在预定义问题上表现优异，但在需要抽象推理和创造性的问题上受限。

然后，论文深入分析了当前系统作为研究代理的五大核心局限：(1) 数据集层面：规模小、覆盖窄、缺乏层级和关系结构；(2) 关系结构层面：数学命题之间的依赖、推论、类比关系未被有效建模；(3) 数学探索层面：现有环境不支持逐步探索、假设提出和交互式验证；(4) 工具生态系统层面：Lean、Coq等系统碎片化，缺乏统一接口和工作流支持；(5) 人机协作层面：人类与AI的角色分工不明确，协作工具原始。这些局限共同导致现有系统无法处理开放式的前沿数学问题。

基于上述分析，论文提出了一项战略路线图，旨在弥合求解器与研究代理之间的鸿沟。该路线图围绕五个支柱构建：扩展和结构化形式数学数据、构建关系型数学知识库、开发交互式数学探索平台、统一工具生态系统以及设计高效的人机协作机制。论文特别强调最终目标不应是完全自主的定理证明，而是能够增强人类数学能力的协作系统。

最后，论文呼吁社区关注AI4Math在抽象概念理解、数学创造力和端到端自动形式化等根本性挑战，并鼓励开展跨学科合作以实现向研究代理的转变。本文作为综合性立场论文，为AI4Math的未来发展方向提供了清晰的系统分析和建设性建议。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本文的系统回顾和批判性分析方法适用于其他AI for Science领域（如生物信息学、材料科学），可以借鉴其发现差距和提出路线图的框架。

## 基本信息

- 作者：Eric Jiang, Xiao Liang, Yikai Zhang, Yingjia Wan, Mengting Li, Haikang Deng, Alexander K. Taylor, Justin Baker, Rushil Raghavan, Junyi Zhang, Ying Nian Wu, Andrea L. Bertozzi, Kai-Wei Chang, Raghu Meka, Matthew Sottile, Nanyun Peng, Amit Sahai, Terence Tao, Wei Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-07-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.07779`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，并优先使用了field_evidence_map指定的证据片段。
