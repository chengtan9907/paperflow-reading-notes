---
user_id: "cheng tan"
paper_id: 9214
arxiv_id: "2608.22695v1"
title: "Enrich-Retrieve-Rank: Scaling Capability Discovery Beyond In-Context Routing"
publish_date: "2026-08-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.22695v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.22695v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:16:42"
---
# Enrich-Retrieve-Rank: Scaling Capability Discovery Beyond In-Context Routing

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：capability discovery · in-context routing · retrieve-then-rank · tool selection

## 一句话总结

本文提出 Enrich-Retrieve-Rank 流水线，将智能体生态中的能力发现从上下文路由重构为离线富化加在线检索-重排，实验表明在注册表规模从 10 增长到 7278 时，其 Match@1 从 0.81 仅降至 0.39，而上下文路由从 0.85 崩溃至 0.12，并在满规模下以约一半成本比 Search&Pick 高 6.5 个百分点。

## 摘要

> Agent ecosystems now include thousands of MATS components (Models, Agents, Tools, and Skills), yet their discovery still relies on in-context routing. These systems read a registry (names, hints, or descriptions, as context budget permits), pick a candidate, invoke it, and retry on failure. This pattern degrades with scale, and registries are growing fast. We recast capability discovery as search over a registry by defining an offline enrichment step that turns sparse metadata into searchable profiles, and an online retrieve-then-rank pipeline that returns a ranked shortlist without invoking any candidates online. We show that from N=10 to 7,278 capabilities, in-context routing's top-1 accuracy (Match@1) collapses (0.85 to 0.12), while retrieve-then-rank degrades more gently (0.81 to 0.39) because its reranker still ranks the right capability first 0.70-0.87 of the time once retrieval finds it. In the Nova Micro sweep, the crossover is around N=500. We compare against two in-context baselines. Full-Ctx puts the whole registry in the prompt and asks the LLM to pick. Search&Pick gives the LLM a search tool to narrow candidates before it picks. At full scale the pipeline leads Search&Pick by 6.5 percentage points (pp) on Match@1 at about half the cost. It reduces cost 70x versus Full-Ctx. We use a fixed configuration (same enrichment, retriever, and scorer weights) across agent, tool, and skill registries. The pipeline runs in production as the default capability-discovery layer of a large-scale multi-agent platform.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：随着智能体生态中的可复用组件（模型、智能体、工具、技能，统称 MATS）数量爆炸式增长，传统基于 in-context routing 的能力发现方式变得不可扩展。现有系统通常把注册表（名称、提示、描述等）塞进上下文，让 LLM 从中选一个候选并调用，失败后重试。这种方式有几个根本性缺陷：（1）上下文预算有限，无法容纳快速增长的全量注册表；（2）即使能放入，LLM 在数千候选上的选择准确率会急剧下降，论文数据显示从 N=10 到 N=7278，Match@1 从 0.85 跌到 0.12；（3）为了试探候选，系统可能调用不可信端点，产生安全和成本问题；（4）元数据往往是稀疏的，仅有名称和短描述，不足以支撑精确选择。论文把该问题重新概念化为“注册表上的搜索问题”，类比网页搜索：用户不必点击每一个网页，而是通过检索和重排获得一个小的候选列表。目标是在不调用任何候选的情况下，先快速缩小候选集，再精排，使发现过程随规模优雅退化。论文还强调这种新范式必须与现有 in-context 方法在同一基准上可比较地评估，并已在一个生产级多智能体平台中作为默认能力发现层运行。

Q2: 有哪些相关研究？

相关研究可分为几条线。首先是工具选择与调用：MetaTool（Huang et al., 2024b）和 Qu et al.（2024）确认工具选择是智能体失败的主要模式，但没有提出 retrieve-then-rank 的流水线；他们主要聚焦于在少量工具中做选择。其次是智能体路由：AppWorld（Trivedi et al., 2024）在智能体选择场景中保持了某种路由机制，但并未针对大规模注册表设计。再次是系统实践：Anthropic（2025）、Cursor（2024）、LangChain（2024）和 Amazon Web Services（2024）等已暴露包含数千组件的注册表，但他们的发现机制仍以上下文读取为主。还有相关工作涉及图过滤（据检索片段，该工作能改善 269 个工具下的发现，但在较小的人工菜单上不改进，但论文未给出具体名称；合理推断这部分属于工具检索或语义路由的尝试）。总体来看，现有工作一方面证实了能力选择是瓶颈，另一方面没有把发现问题与规模化检索-重排框架系统连接起来，正是本文的切入点。

Q3: 论文如何解决这个问题？

论文的解决方案是一个两阶段流水线：Enrich-Retrieve-Rank。
1. 离线 enrichment：将注册表中原始的稀疏元数据（例如名称、简短提示）转换成更丰富、可搜索的 profile。这一步骤不改变组件本身，只加工描述性信息，可能是通过 LLM 生成结构化描述、关键词、使用样例、输入输出模式等（具体细节论文原文未在摘要中展开，合理推断）。作者强调 enrichment 可以在离线完成，不影响在线延迟。
2. 在线 retrieval：给定用户查询，先用一个检索器从全量注册表中召回一个较小的候选集。该检索器基于向量相似度或类似方法，只依赖 profile 与查询的匹配，不调用任何候选组件。
3. 在线 rerank：用一个重排器对召回的候选进行精细打分，返回排序后的短列表。论文使用的重排器在检索到正确能力时，将其排在首位的概率为 0.70-0.87。全文采用固定配置，即同一套 enrichment、retriever 和 scorer 权重应用于 agent、tool、skill 三类注册表，证明该方法的跨类型通用性。
论文还定义了评估指标 Match@1（首位命中率），并与两个基线对比：Full-Ctx（全量注册表放入 prompt）和 Search&Pick（用搜索工具逐步缩小范围）。此外，论文在 Nova Micro 扫描中测试了从 N=10 到 7278 的规模变化，找到了性能交叉点。整个流水线已作为生产环境默认能力发现层部署。

Q4: 论文做了哪些实验？

论文进行了系统性的规模实验和基线对比：
1. 规模扫描：在 N=10 到 7278 的注册表规模上比较 in-context routing 和 retrieve-then-rank 的 Match@1。结果显示前者从 0.85 崩到 0.12，后者从 0.81 缓降到 0.39。在 Nova Micro 扫描中，交叉点约在 N=500。
2. 三个注册表类型：agent、tool、skill。其中最大的工具基准 ToolRet 包含 7,961 个查询和 7,278 个能力。论文表示 agent 和 skill 基准存在构建限制（§4.2, §4.3），无法独立支撑跨类型结论。
3. 基线对比：在满规模 ToolRet 上，Ours+Titan 的 Match@1 达到 0.397，比 Search&Pick（0.332）高 6.5 个百分点，而 token 成本约为其一半；相比 Full-Ctx，成本降低约 70 倍。
4. 生产部署：该流水线已在一个大型多智能体平台中作为默认能力发现层运行，说明其实际可行性。

Q5: 发现了什么实验现象？

实验观察到的关键现象：
1. 上下文路由的崩溃：从 N=10 到 7278，Match@1 从 0.85 快速跌至 0.12，说明上下文方法无法在较大注册表上保持选择准确率。
2. 检索-重排的平缓退化：同样的规模跨度，retrieve-then-rank 仅从 0.81 降至 0.39，退化更温和；这暗示在召回阶段没有严重漏掉正确候选。
3. 重排器的有效性：一旦检索找到正确能力，重排器将其排在首位的次数占比为 0.70-0.87，说明重排环节是可靠的，瓶颈主要在召回。
4. 交叉点：在 Nova Micro 扫描中，两种范式性能交叉约在 N=500，意味着在小规模下上下文路由仍有优势或相当，超过该规模后检索-重排更胜一筹。
5. 成本优势：在满规模下，Ours+Titan 的 Match@1 比 Search&Pick 高 6.5pp，但 token 成本减半；比 Full-Ctx 成本降低 70 倍。
6. 富化的非线性效果：局限性中提到 enrichment 在文档良好的能力上是中性或负面的，说明对已有高质量描述的组件，额外富化可能引入噪声或无用信息；这也意味着富化带来的主要收益在元数据稀疏的组件上。
7. 跨类型固定配置有效：同一套配置在 agent、tool、skill 上都能工作，但这三个基准在构造上的差异仍需注意（agent、skill 基准有构建限制），所以跨类型的强势结论要谨慎。

Q6: 有什么可以进一步探索的点？

根据现有结果和局限性，可以进一步探索的方向包括：
1. 自动化 enrich：当前 enrichment 的具体方法未完全公开，可以研究如何自动选择富化策略，例如根据元数据质量动态决定是否富化、富化到什么程度，以避免对良好文档造成负面影响。
2. 动态注册表：真实注册表会持续新增、更新、删除组件，离线富化和索引需要增量更新机制，可以考虑在线学习或周期性重建。
3. 跨类型强泛化：论文因 agent 和 skill 基准的构建限制而无法独立地验证跨类型结论，未来可以构建更均衡的 benchmark 来检验 agent、tool、skill 之间的迁移能力。
4. 重排器的可解释性：目前重排器权重固定且具体架构未知，可以探索可解释的排序分数或混合式评分。
5. 与生成式方法的结合：除了检索-重排，还可以考虑用生成模型直接预测候选 ID，或两者结合。
6. 安全与信任：在线检索后仍可能接触不可信端点，可以研究在检索-重排阶段加入安全过滤或权限检查。
7. 更细粒度的成本度量：论文主要报告 token 成本，可以考虑延迟、API 调用次数、端到端成功率等。
8. 应用到 AI for Science：检索-重排框架可以迁移到科学工具、数据集或模型组件的选择上，用户画像中偏好生物科学应用，可以此为契机。

Q7: 总结一下论文的主要内容

论文《Enrich-Retrieve-Rank: Scaling Capability Discovery Beyond In-Context Routing》针对智能体生态系统中能力发现机制的可扩展性问题，提出并验证了一种全新的两阶段流水线。随着 MATS（Models, Agents, Tools, Skills）组件数量从几十增长到数千，传统依赖上下文路由（in-context routing）的方式将整个注册表信息塞入 prompt，再由 LLM 挑选并调用候选，其性能会急剧恶化。论文将此问题类比为网络搜索：用户不应被迫浏览所有网页，而应通过搜索引擎获得一个排序后的候选列表。由此，作者设计了 Enrich-Retrieve-Rank 范式，包含离线 Enrichment、在线 Retrieve 和在线 Rank 三个环节。

在 Enrichment 阶段，系统将原始的稀疏元数据（如名称、短提示）转化为信息密度更高、更适合检索的 profile。这一步骤离线完成，避免在线延迟。在 Retrieve 阶段，系统基于用户查询从全量注册表中召回一个较小的候选集，不调用任何组件。在 Rank 阶段，重排器对候选进行精细排序，返回一个短列表。论文强调，整个流水线使用固定配置（相同的 enrichment、retriever 和 scorer 权重）跨越 agent、tool 和 skill 三类注册表，展示了其通用性。

实验方面，论文构建了一个从 N=10 到 N=7278 的规模扫描。关键的定量发现是：in-context routing 的 Match@1（首位命中率）从 0.85 崩塌到 0.12，而 retrieve-then-rank 仅从 0.81 温和下降到 0.39。在 Nova Micro 扫描中，两者性能交叉点约在 N=500，这为系统设计者提供了切换策略的依据。论文还对比了两个上下文基线：Full-Ctx（将完整注册表放入 prompt）和 Search&Pick（给 LLM 提供搜索工具以缩小候选集）。在最大规模的工具基准 ToolRet 上（7,961 个查询、7,278 个能力），本文方法（Ours+Titan）的 Match@1 达到 0.397，比 Search&Pick 的 0.332 高出 6.5 个百分点，同时 token 成本减半；相对 Full-Ctx，成本降低约 70 倍。此外，重排器的表现也值得注意：一旦检索器命中正确能力，重排器将其排在首位的概率为 0.70-0.87，说明重排是可靠的，性能瓶颈主要在召回阶段。

论文还讨论了与现有工作的关系：MetaTool 和 Qu et al. 已经确认工具选择是智能体失败的主要模式，但未提出检索-重排方案；AppWorld 保留了智能体路由机制，但未面向大规模注册表。一些系统（Anthropic、Cursor、LangChain、AWS）已经暴露了数千组件的注册表，但发现机制仍以上下文读取为主。论文中的生产部署作为一个大型多智能体平台的默认能力发现层，增强了其实际有效性。

局限性方面：最强的结论来自 ToolRet（N=7,278），而 agent 和 skill 基准存在构建限制（§4.2, §4.3），不能独立支撑跨类型的强断言；enrichment 在文档良好的能力上呈中性或负面影响，说明其收益主要集中在元数据稀疏的场景。这些限制了结论的普适性，但也为未来研究指出了方向。

总体而言，该论文通过将能力发现问题转换为注册表检索问题，给出了一个可扩展、成本可控、已在生产运行的解决方案，其规模与退化曲线的定量刻画对构建大规模智能体系统具有实际参考价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文核心主题与你的智能体（agent）方向直接相关（权重 0.10）。

## 基本信息

- 作者：Nazib Sorathiya, Daniel Zhang, Bardiya Akhbari
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.IR
- 日期：2026-08-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.22695v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Abstract、Introduction、Related Work、Results 和 Limitations 片段，并结合启发式草稿进行了中文改写与补充。
