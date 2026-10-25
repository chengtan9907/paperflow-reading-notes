---
user_id: "cheng tan"
paper_id: 16917
arxiv_id: "2610.27700"
title: "Memory as Code: Declarative Retrieval over Codified Long-Term Memory"
publish_date: "2026-10-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.27700.pdf"
pdf_url: "https://arxiv.org/pdf/2610.27700"
abs_url: "https://arxiv.org/abs/2610.27700"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-21T11:40:23"
---
# Memory as Code: Declarative Retrieval over Codified Long-Term Memory

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：long-term memory · LLM agent · declarative retrieval · code abstraction

## 一句话总结

本文提出 Memory as Code (MaC)，将长期记忆构建为三层可执行代码抽象，并把查询编译为声明式检索程序，在 LOCOMO 和 LONGMEMEVAL 上以显著更少的 token 消耗取得最高准确率。

## 摘要

> In long-term memory systems, memories are constructed from interaction his-Oct tories and retrieved in response to queries, enhancing the reasoning ability of
> LLM agents. In complex scenarios, facts are scattered across sessions and their
> relationships evolve over time, making it difficult for existing memory systems20
> to compose them to adapt to diverse queries. Motivated by the modularity and
> composability of code, we propose Memory as Code (MaC), a unified mem-
> ory framework that jointly designs memory construction and retrieval through a
> shared code abstraction. MaC couples codified construction, which establishes ex-
> ecutable memory abstractions, with declarative retrieval, which translates queries
> into structured retrieval programs. Specifically, codified construction builds a[cs.CL] three-layer executable memory structure that progresses from an atomic function
> layer, through a relational layer, to a modular composition layer. Declarative re-
> trieval recasts retrieval from relevance-based fact selection into declarative query
> processing over memory. Experiments on LOCOMO and LONGMEMEVAL show
> relative gains of up to 15.91% in overall LLM-judge accuracy over the strongest
> baseline in each setting, while substantially reducing token consumption, high-
> lighting the effectiveness of memory as code for long-horizon memory tasks.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决长期记忆系统中记忆构建与检索分离设计带来的根本性问题：

1. **事实分散与关系演化**：在真实复杂场景中，同一实体或事件的事实往往分散在多个会话中，且事实间的关系随时间变化（例如某人的工作状态从休假变为流感病假）。现有记忆系统将记忆视为静态事实集合，缺乏对关系动态演化的显式建模，导致查询时需要跨会话组合事实时力不从心。

2. **记忆构建缺乏计算封装**：现有构建方法（如 MemoryOS 的主题分段与摘要、Mem0 的更新操作整合、A-Mem 的动态链接笔记）虽然能组织记忆数据，但只存储事实本身，没有把对事实的常见计算（如统计、比较、聚合）封装为可复用单元。因此，不同查询需要重复执行相同的推理步骤，无法跨查询复用。

3. **检索方式的两难**：预定义检索管道（如固定 pipeline）对多样化查询缺乏灵活性；而 agentic 检索（如逐步 LLM 规划）虽然灵活，但每次查询都需要多次 LLM 调用，推理开销大、延迟高。现有方法将构建与检索视为独立模块，构建阶段不考虑检索需求，检索阶段也无法利用构建阶段封装的抽象。

4. **缺乏联合设计**：论文的核心论点是，记忆构建和检索应当共享一种统一的抽象（代码），使构建阶段封装可复用的计算，检索阶段按需组合这些计算。这种联合设计既能保留灵活性，又能避免逐步规划的昂贵开销。

因此，MaC 的目标是：让记忆系统像代码一样具有模块性、可组合性和可执行性，使查询处理变成对记忆程序的声明式编译与执行。

Q2: 有哪些相关研究？

相关研究可分为以下几类：

1. **长期记忆构建方法**：
 - MemoryOS (Kang et al., 2025)：将对话组织为主题分段并生成摘要，支持记忆的组织与维护。
 - Mem0 (Chhikara et al., 2025)：通过更新操作整合事实，实现记忆的持续演化。
 - A-Mem (Xu et al., 2026b)：构建动态链接的记忆笔记，强调记忆之间的关联。
 这些方法都侧重于记忆数据的组织，但缺乏对存储事实的计算封装，无法将常用操作抽象为可复用函数。

2. **记忆检索方法**：
 - 基于搜索与扩展：结合向量检索与上下文/关系扩展（如 Borgeaud et al., 2022; Liu et al., 2023）来收集相关证据。
 - 预定义管道：使用固定流程（如 Han et al., 2024; Rasmussen et al., 2025）进行检索，但灵活性有限。
 - Agentic 工作流：通过 LLM 逐步规划（如 Yao et al., 2022; Li et al., 2025; Ji et al., 2026）实现灵活检索，但每次查询需要多次 LLM 调用，推理开销大。

3. **代码抽象与程序合成**：本文受代码的模块化与可组合性启发，将记忆视为可执行程序。这与程序合成、声明式编程的思想一脉相承，但在记忆系统领域是新的尝试。

4. **与 RAG 的关系**：传统 RAG 将检索视为相关性事实选择，而 MaC 将其重构为声明式查询处理，即把查询编译为对记忆结构的操作序列，类似于数据库查询优化。

总体而言，现有工作要么只改进构建，要么只改进检索，缺乏两者的联合设计。MaC 的独特之处在于用代码抽象统一两者，使构建阶段产出的可执行函数能被检索阶段直接组合。

Q3: 论文如何解决这个问题？

MaC 的核心思想是“记忆即代码”，通过共享代码抽象联合设计记忆构建与检索。具体包含两个互补技术：

1. **编码化构建（Codified Construction）**：
 - 建立三层可执行记忆结构：
 - **原子函数层**：将单个事实或基本操作封装为原子函数（如 `vacation_f1()` 表示某次休假事实）。
 - **关系层**：表示事实之间随时间演化的关系（如工作状态从休假变为病假），支持关系查询和推理。
 - **模块组合层**：将常用计算组合为可复用的模块（如 `gap_count()` 统计工作空档次数），供不同查询调用。
 - 这种结构使记忆不仅是数据，更是可执行的计算单元，支持跨查询复用。

2. **声明式检索（Declarative Retrieval）**：
 - 将用户查询翻译为结构化检索程序 Pq，程序包含操作符、参数、依赖关系和条件延续。
 - 利用记忆索引（内容索引 + 操作符接口）选择并组合所需的记忆计算，指定操作符参数、依赖和条件分支。
 - 执行器检查并运行程序，中间结果决定后续条件延续，可触发额外的记忆访问。
 - 检索过程包括：源上下文定位、证据组合（合并、过滤、排序、聚合）、验证（启发式检查证据完整性和未解决状态）。
 - 与 agentic 方法不同，MaC 不需要逐步 LLM 规划，而是将查询一次性编译为完整程序，从而降低推理开销。

3. **联合设计**：构建阶段产出的函数库和索引结构直接服务于检索阶段的程序编译，检索阶段的需求也反馈到构建阶段，形成闭环。

整体上，MaC 将记忆系统从“存储-检索”范式转变为“编程-执行”范式，用代码的模块性和可组合性解决事实分散与关系演化问题。

Q4: 论文做了哪些实验？

论文在实验部分设计了三个研究问题（RQ）并进行了系统评估：

1. **数据集**：
 - LOCOMO (Maharana et al., 2024)：评估长对话上的问答。
 - LONGMEMEVAL (Wu et al., 2024)：评估跨多个会话、每个查询对应更长交互历史的记忆。

2. **Baselines**：A-Mem、MemoryOS、Mem0、MRAgent (Ji et al., 2026)。

3. **实现细节**：
 - 硬件：NVIDIA A100 GPU。
 - 在 LOCOMO 上使用三个 LLM backbone：Qwen3.6-27B、ChatGPT-5.6-Luna、DeepSeek-V4-Flash。
 - 在 LONGMEMEVAL 上使用 Qwen3.6-27B。
 - 评估指标：F1 和 LLM-Judge (J) 分数，其中 J 分数由 DeepSeek-V4-Flash 作为评判模型获得。
 - 代码已开源：https://github.com/zengaoting/MemoryasCode

4. **RQ1（总体性能）**：比较 MaC 与 baseline 在不同 benchmark、问题类型和 LLM backbone 上的表现。

5. **RQ2（成本、运行时、存储）**：测量记忆构建和检索过程中的 token 消耗、墙钟运行时间和存储占用。

6. **RQ3（消融研究）**：分别评估可执行记忆和程序化搜索对 MaC 性能的单独及联合贡献。

实验设计覆盖了性能、效率和组件贡献三个维度，且使用了多个 backbone 以验证泛化性。

Q5: 发现了什么实验现象？

基于提供的证据和摘要，可归纳以下实验现象：

1. **总体性能领先**：MaC 在 LOCOMO 和 LONGMEMEVAL 上均取得最高的 F1 和 LLM-judge 分数，相对最强 baseline 的 LLM-judge 准确率最高提升 15.91%。这表明“记忆即代码”的联合设计在长时程记忆任务上具有明显优势。

2. **消融实验的关键发现**：当移除声明式检索（DR），改用固定检索程序（结合词法召回与预定义记忆操作符执行候选记忆函数）时，整体 J 分数下降；启用 DR 后整体 J 提升 6.62 个百分点。这说明按查询自适应地选择和组合检索操作是性能提升的重要来源，而非仅仅依赖可执行记忆库。

3. **Token 消耗显著降低**：摘要明确提到 MaC 大幅减少 token 消耗。虽然具体数字在提供的证据片段中不清晰（存在表格噪声），但可以合理推断，由于避免了逐步 LLM 规划，推理成本显著下降。

4. **跨 backbone 的稳定性**：在 LOCOMO 上使用三种不同 backbone 均取得最佳结果，说明方法对底层模型不敏感，具有较好的泛化性。

5. **注意**：关于各 baseline 的具体分数、不同问题类型的分解、存储和运行时的精确数值，在提供的材料中未完整呈现，需查阅原文确认。

Q6: 有什么可以进一步探索的点？

基于论文的贡献和当前局限，可以探索以下方向：

1. **自动构建记忆函数库**：当前三层结构中的函数和模块可能需要人工设计或依赖 LLM 生成，未来可研究如何从交互历史中自动发现和抽象可复用函数，减少人工干预。

2. **扩展到更多任务和模态**：MaC 目前聚焦于对话记忆，可扩展到多模态记忆（图像、音频）、工具使用记录、代码仓库等场景，验证代码抽象的通用性。

3. **动态演化与在线学习**：记忆结构应随新交互持续更新，未来可研究增量式函数库更新、关系层的时间衰减机制，以及如何避免过时事实的干扰。

4. **程序生成的可解释性与鲁棒性**：声明式检索程序由 LLM 编译，可能产生错误或低效程序。可探索程序验证、错误恢复、以及将程序生成与符号推理结合以提高可靠性。

5. **与 agentic 规划的混合**：MaC 避免了逐步规划，但在极端复杂查询中，完全声明式可能不够灵活。可研究将声明式程序与少量 agentic 步骤结合，平衡开销与适应性。

6. **更大规模与更长时程验证**：在更长的交互历史（如数月或数年）和更大规模记忆库上测试，观察三层结构的扩展性和检索效率。

7. **跨领域迁移**：将 MaC 应用于 AI for Science 场景，如科研文献记忆、实验记录管理，验证其作为通用记忆范式的潜力。

Q7: 总结一下论文的主要内容

本文针对长期记忆系统中事实分散、关系演化以及构建与检索分离导致的问题，提出了 Memory as Code (MaC) 框架。核心动机是：现有记忆构建方法（如 MemoryOS、Mem0、A-Mem）虽然能组织记忆数据，但缺乏对存储事实的计算封装，无法跨查询复用；现有检索方法要么使用预定义管道（灵活性差），要么依赖 agentic 逐步规划（开销大）。作者认为，代码的模块化与可组合性恰好能解决这一矛盾，因此提出用共享代码抽象统一记忆构建与检索。

技术路线上，MaC 包含两个互补组件。第一是编码化构建（codified construction），它将记忆组织为三层可执行结构：原子函数层封装单个事实或基本操作，关系层表示事实间随时间演化的关系，模块组合层将常用计算组合为可复用模块。这种结构使记忆不仅是数据，更是可执行的计算单元。第二是声明式检索（declarative retrieval），它将用户查询编译为结构化检索程序 Pq，程序包含操作符、参数、依赖和条件延续；执行器检查并运行程序，中间结果决定条件分支，可触发额外记忆访问。检索过程包括源上下文定位、证据组合和验证。通过这种方式，MaC 将检索从相关性事实选择转变为声明式查询处理，避免了逐步 LLM 规划的高开销。

实验部分，作者在 LOCOMO 和 LONGMEMEVAL 两个基准上，与 A-Mem、MemoryOS、Mem0、MRAgent 四个代表性 baseline 进行比较。使用 F1 和 LLM-Judge 分数作为指标，在 LOCOMO 上使用三种 LLM backbone（Qwen3.6-27B、ChatGPT-5.6-Luna、DeepSeek-V4-Flash），在 LONGMEMEVAL 上使用 Qwen3.6-27B。实验围绕三个研究问题展开：总体性能、成本/运行时/存储、消融研究。结果显示，MaC 在所有设置中取得最高 F1 和 LLM-judge 分数，相对最强 baseline 的 LLM-judge 准确率最高提升 15.91%，同时大幅降低 token 消耗。消融实验表明，启用声明式检索比固定检索程序提升 6.62 个百分点 J 分数，验证了按查询自适应组合检索操作的重要性。

结论上，作者强调 MaC 通过联合设计构建与检索，实现了记忆的模块化、可组合性和可执行性，为长时程记忆任务提供了高效且灵活的解决方案。论文的主要贡献在于提出了一种新的记忆范式，将代码抽象引入记忆系统，并在多个维度上验证了其有效性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体方向直接相关：长期记忆是 LLM 智能体的核心组件，MaC 提供了一种新的记忆构建与检索范式。

## 基本信息

- 作者：Aoting Zeng, Yuxin Du, Jianwei Wang, Yizhang He, Shan Jiang, Xuemin Lin, Kai Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-10-21
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.27700`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的证据片段（Introduction、Results、Conclusion），并结合摘要与启发式草稿；部分细节（如具体 token 数值、各 baseline 分数）因原文缺失而标注为推测。
