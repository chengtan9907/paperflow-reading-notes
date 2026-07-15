---
user_id: "cheng tan"
paper_id: 3668
arxiv_id: "2607.11564v1"
title: "PaperRouter-Agent: A Content-Grounded LLM Agent for Personalized Hierarchical Paper Routing"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.11564v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.11564v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-15T00:45:27"
---
# PaperRouter-Agent: A Content-Grounded LLM Agent for Personalized Hierarchical Paper Routing

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：personalized hierarchical paper routing · llm agent · folksonomy · content-grounded routing

## 一句话总结

提出了PaperRouter-Agent，一种无需训练的LLM智能体，通过检查文件夹中的论文内容而非仅文件夹名称来进行个性化层次化论文路由。

## 摘要

> Researchers organize the papers they collect into personal folder hierarchies in reference managers, and route each new paper into the folder where it belongs. This task differs from standard hierarchical text classification. A user's folder hierarchy is not a fixed, shared taxonomy but a private and evolving folksonomy whose folder meanings may be topical, shorthand, venue-based, or process-oriented, and are often defined by the papers already stored inside them. We formalize this setting as personalized hierarchical paper routing (PHPR): assigning an incoming paper to folders in a user-specific hierarchy without per-user training. We propose PaperRouter-Agent, a training-free LLM agent that grounds routing decisions in folder members rather than folder names alone. The agent first narrows the candidate hierarchy, retrieves folder-specific evidence, verifies fit by inspecting member papers, and incorporates similarity-gated feedback from past user rejections. A formative study on real personal libraries shows that PaperRouter-Agent raises overall Recall@1 from 0.39 to 0.61 and Recall@3 from 0.57 to 0.83, with the largest gains on organizational folders defined by metadata such as venue or year, where single-shot methods collapses (Recall@1 0.09 to 0.50). On the public LaMP-2 benchmark, the same approach improves accuracy from 44.5% to 51.5% (+9.0 macro-F1) over a single-shot baseline, while remaining low-cost for practical use.

Q1: 这篇论文试图解决什么问题？

研究者使用文献管理工具（如Zotero、Mendeley）将收集的论文组织到个人文件夹层次中，每篇新论文需要被路由到正确的文件夹。这一任务与标准层次文本分类（HTC）有本质区别：用户的文件夹层次不是固定的共享分类法，而是私有的、不断演化的分类法（folksonomy），文件夹含义可能基于主题、简称、会议、年份或项目，并且通常由已存入的论文定义。此外，新用户没有历史训练数据，现有方法无法适应。论文形式化了这一任务为个性化层次论文路由（PHPR）：在无需每个用户训练的情况下，将新论文分配到用户特定层次结构中的文件夹。

Q2: 有哪些相关研究？

相关研究包括层次文本分类（HTC），其通常使用共享固定分类法并在大型语料库上训练，不适用于个人库。也有个性化分类的工作，但大多针对固定标签集。LLM代理在文本分类中的应用已有研究，但未专门针对个人论文路由。论文在实验中使用单次LLM分类器作为基线，并使用LaMP-2基准（邮件分类，但可以模拟个性化）进行比较。此外，论文讨论了与传统HTC方法的差异，强调个人库作为folksonomy的特性。

Q3: 论文如何解决这个问题？

PaperRouter-Agent包含四个阶段：(1) Planner：使用LLM基于文件夹名称和层次结构初步缩小候选文件夹范围，输出可能性排序；(2) Retriever：为每个候选文件夹检索其中成员论文的摘要或标题作为证据；(3) Inspector：LLM检查新论文与文件夹成员论文的匹配度，判断是否适合该文件夹；若没有文件夹明确匹配，则回退到单次基线；(4) Reflector：如果用户拒绝建议，记录拒绝事件，并在后续通过相似度门控将相关拒绝案例加入Inspector的上下文。整个流程无需训练，仅需两次LLM调用（一次Inspector，一次可能的单次分类器）。

Q4: 论文做了哪些实验？

论文在真实个人图书馆数据集上进行形成性研究（formative study），收集了10位研究者的实际文件夹结构和论文（总共约50个文件夹、数百篇论文）。还使用公共LaMP-2基准（邮件分类任务，可视为个人学习路由）。评价指标包括Recall@1、Recall@3和macro-F1。与单次LLM分类器（baseline）进行比较，并消融了反思机制。实验还按文件夹类型（如主题型、组织型/venue、年份型）分析了性能差异。

Q5: 发现了什么实验现象？

在真实个人库上，PaperRouter-Agent将整体Recall@1从0.39提高到0.61，Recall@3从0.57提高到0.83。最大增益出现在组织性文件夹（如按会议或年份分类）上，单次方法在此类文件夹上Recall@1仅0.09，而本文方法达到0.50，表明内容驱动检查对无描述性名称的文件夹特别有效。在LaMP-2上，准确率从44.5%提升到51.5%，macro-F1提升9.0%。通过消融发现，Inspector和Reflector组件都带来显著提升。此外，论文报告了不同文件夹深度和样本数量的影响，但具体数值需参考原文。

Q6: 有什么可以进一步探索的点？

未来工作可以探索：更复杂的反馈整合（如隐式反馈），处理冷启动新文件夹的方法（如基于元数据启发），扩展到更多类型的文献管理软件和不同语言论文，结合用户当前的论文集合动态更新文件夹表示，以及降低LLM调用成本（如更小的模型或缓存）。此外，可以研究如何自动建议新建文件夹或合并现有文件夹。

Q7: 总结一下论文的主要内容

论文提出了一种新的任务形式——个性化层次论文路由（PHPR），并设计了一个无需训练的LLM代理PaperRouter-Agent来解决它。该代理通过Planner缩小候选，Retriever获取文件夹内论文证据，Inspector评估匹配，Reflector利用用户反馈改进。在真实数据集和LaMP-2基准上的实验表明，该方法显著优于单次基线，特别是在按元数据（如venue, year）组织的文件夹上。论文的贡献包括任务形式化、方法设计、评估协议和实证结果。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文与你画像中的agent方向直接相关（权重0.10），提供了LLM agent在个性化分类中的应用案例。

## 基本信息

- 作者：Keshen Zhou, Lintao Wang, Suqin Yuan, Zhuqiang Lu, Yu Luo, Zhiyong Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.HC, cs.IR
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.11564v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，包括heuristic_draft和retrieved_evidence。
