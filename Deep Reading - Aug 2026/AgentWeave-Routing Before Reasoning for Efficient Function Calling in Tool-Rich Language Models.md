---
user_id: "cheng tan"
paper_id: 9237
arxiv_id: "2608.23078v1"
title: "AgentWeave: Routing Before Reasoning for Efficient Function Calling in Tool-Rich Language Models"
publish_date: "2026-08-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.23078v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.23078v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:18:56"
---
# AgentWeave: Routing Before Reasoning for Efficient Function Calling in Tool-Rich Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：function calling · tool routing · candidate space construction · pre-inference routing

## 一句话总结

本文提出 AgentWeave，一个在语言模型推理之前进行确定性路由的预推理层，通过缩小模型可见的工具候选集来提升函数调用的效率与成功率，并在一组 BFCL 派生任务上验证了候选空间构建策略的可行性。

## 摘要

> Large language models increasingly operate over large collections of tools, functions, APIs, and specialized agents. As the candidate action space grows, a function-calling model must process more schemas, consume more prompt tokens, and distinguish among increasingly similar or irrelevant alternatives. We study a complementary systems strategy: reduce the candidate set before language-model inference while leaving the downstream model unchanged. We introduce AgentWeave, a deterministic pre-inference routing layer that constructs a bounded model-visible action space using eligibility, requirement, capability, and routing signals. We evaluate AgentWeave with a frozen BFCL-derived routing-pressure protocol using the public MadeAgents/Hammer2.1-1.5b model. On 48 fresh BFCL V4 multiple-function tasks, AgentWeave achieves 6/48 (12.5%) native BFCL successes, whereas all-tools, deterministic random top-8, and semantic top-8 baselines each achieve 0/48. The paired success difference is +12.5 percentage points with a 10,000-resample paired bootstrap 95% confidence interval of +4.17 to +22.92 points and exact McNemar p=0.03125. Relative to all-tools exposure, AgentWeave presents 70.18% fewer tools, uses 61.70% fewer input tokens, and exhibits 50.95% lower mean local-model latency. The result is deliberately narrow: this is a BFCL-derived routing-pressure study rather than an official full BFCL leaderboard score, and absolute task success remains low. The evidence nevertheless shows that candidate-space construction can materially affect a fixed model's function-calling behavior and motivates evaluating routing as a distinct stage before model reasoning.

Q1: 这篇论文试图解决什么问题？

1. 核心问题：随着语言模型可用的工具、函数、API 和专用智能体数量爆炸式增长，函数调用面临一个系统性挑战——候选动作空间过大会导致推理效率下降、token 消耗增加，并引入大量相似或无关选项，干扰模型的判别能力。
2. 现有方法大多聚焦于改进模型本身的函数调用能力（如微调、prompt 工程、解码算法），而忽视了系统层面的候选空间构建这一环节。
3. 具体矛盾：模型需要从庞大的工具目录中选出正确的函数，但所有 schema 都暴露给模型时，输入长度和注意力负担显著上升，且容易混淆相似工具。
4. 本文提出的研究问题：能否在模型推理之前用一个确定性的、不改变下游模型的路由层，显著缩小候选工具集，从而改善固定模型的函数调用成功率和资源效率？
5. 问题定位为路由压力（routing pressure）研究：在任务难度很高、候选工具很多的情况下，路由策略是否比全量暴露或简单随机/语义截断更能帮助模型成功？
6. 隐含的范式问题：函数调用应被重新理解为两阶段系统——先构建候选空间，再由模型推理选择；而主流基准和评估往往只度量最终端到端成功率，忽略了中间候选集构建的影响。
7. 论文明确承认这是窄范围研究，不声称解决生产级端到端任务，但强调候选空间构建是一个被低估的干预点。

Q2: 有哪些相关研究？

1. 函数调用与工具学习：相关工作包括 ToolACE（通过工具调用的强化学习赢得 LLM 函数调用点，ICLR 2025）、伯克利函数调用排行榜（BFCL）等。这些工作主要关注如何训练或评估模型更好地进行函数调用。
2. 候选检索与路由：在信息检索和推荐系统中，先检索再精排是常见范式，但将该思路应用在 LLM 工具选择上仍较新。本文与语义检索（semantic top-8）等基线对比，表明确定性规则路由可以超越基于向量相似度的检索。
3. 系统层面优化：与模型无关的 orchestration 层（如介于 source catalog 和现有模型之间）在多智能体系统和工具编排中已有概念，但本文强调预推理、确定性路由与固定模型评估的组合。
4. 工具爆炸与上下文压缩：相关研究试图压缩 prompt 中的工具描述、使用工具摘要或分层组织工具，但本文采用显式路由层来筛选候选工具。
5. 与模型微调方法的区别：本文不提出新的语言模型架构、解码算法或基准评估器，而是保持模型冻结，仅改变输入中的工具集合，这与大多数从模型侧改进的工作形成对比。
6. 与 BFCL 官方评估的关系：本文使用 BFCL V4 派生协议，但明确指出不是官方完整排行榜分数，因此与 leaderboard 评测方法有别。
7. 注意：摘要中提及的基线包括 all-tools、确定性随机 top-8、语义 top-8，但这些基线的具体实现细节在提供的证据中没有完整展开，属于合理推断。

Q3: 论文如何解决这个问题？

1. 整体思想：将函数调用分解为两个阶段——候选空间构建（candidate-space construction）和语言模型推理（language-model reasoning）。AgentWeave 实现第一阶段，保持第二阶段模型不变。
2. 路由层定位：AgentWeave 是位于源工具目录和现有模型之间的编排层（orchestration layer），不替代语言模型，不引入新解码算法，也不修改基准评估器。
3. 信号类型：路由使用四种信号——资格（eligibility）、需求（requirement）、能力（capability）、路由（routing）。这些信号的具体计算方式在提供的证据中未详细展开，但从命名可推断，资格信号可能基于工具是否适用于当前请求（如 API 权限或领域），需求信号从请求中提取需求并与候选描述、能力和提供方分组匹配，路由信号可能用于综合排序和预算控制。
4. 确定性路由：整个路由过程是确定性的，没有学习参数或随机采样，因此可复现且开销低。这与基于嵌入的语义检索不同，后者依赖向量相似度。
5. 有界预算：路由层应用 bounded budget，只将一小部分候选工具暴露给语言模型。该预算大小在论文中未在提供的片段中给出（合理推断为 top-k 形式，比如默认 8，但需原文确认）。
6. 对剩余候选的处理：对于通过资格筛选后的候选，AgentWeave 从请求中派生需求信号，并将其与候选描述、能力、提供方分组进行匹配。这意味着路由考虑了工具的功能描述、能力标注以及提供商分组（例如云服务 API 分组）。
7. 与基线的对照：基线包括 all-tools（暴露所有工具）、确定性随机 top-8、语义 top-8。AgentWeave 与这些基线的差异在于利用结构信息和规则而非随机或嵌入相似度。
8. 实施细节：使用公共模型 MadeAgents/Hammer2.1-1.5b 作为固定下游模型，在 BFCL V4 任务的子集上评测。由于这部分信息主要来自摘要和结论片段，具体算法流程需要参考正文方法部分（本摘要未提供）。

Q4: 论文做了哪些实验？

1. 实验协议：采用 BFCL 派生的路由压力协议（routing-pressure protocol），使用公开的 MadeAgents/Hammer2.1-1.5b 模型，评估固定模型在不同工具候选集条件下的函数调用成功率。
2. 任务集：48 个全新的 BFCL V4 多函数任务（multiple-function tasks）。这些任务对函数调用能力要求较高，包含多个相似或冗余工具，给模型制造压力。
3. 对比条件：
 - all-tools：将所有工具 schema 全部暴露给模型。
 - deterministic random top-8：确定性随机选择 8 个工具。
 - semantic top-8：基于语义相似度选择 top-8 工具。
 - AgentWeave：论文提出的确定性预推理路由。
4. 主要指标：原生 BFCL 成功（即严格匹配函数调用格式和正确工具选择），以及资源指标：工具数量、输入 token 数、平均本地模型延迟。
5. 统计检验：配对成功差异使用 10,000 次重采样配对 bootstrap 计算 95% 置信区间（+4.17 到 +22.92 个百分点），并用精确 McNemar 检验得到 p=0.03125。
6. 结果：AgentWeave 达到 6/48（12.5%）成功率，而三个基线均为 0/48。
7. 效率收益：与 all-tools 相比，AgentWeave 减少 70.18% 工具数量，节省 61.70% 输入 token，平均本地模型延迟降低 50.95%。
8. 注意：论文明确声明这不是官方完整 BFCL 排行榜分数，因此实验规模小，绝对成功率低（12.5%），属于路由压力下的探索性研究。

Q5: 发现了什么实验现象？

1. 主要观察：在相同的冻结模型下，改变候选工具集的大小和构成能显著影响函数调用成功率。AgentWeave 获得 12.5% 成功，而 all-tools、随机 top-8 和语义 top-8 全部为 0%，表明全量暴露不一定最好，甚至可能因为候选过多导致模型无法正确调用。
2. 反直觉点：语义检索（semantic top-8）作为常见信息检索手段，在函数调用路由场景中未能带来任何成功，说明语义相似度可能不足以捕捉工具匹配的关键信息（如请求中的精确参数需求、API 权限等），而确定性规则（AgentWeave）更有效。
3. 随机选择也会导致 0% 成功，说明缩小候选集本身并不能保证成功，路由策略的质量至关重要。
4. 效率与准确率的权衡并不是二选一：AgentWeave 在提升成功率的同时还大幅减少工具数量和 token 消耗，延迟也降低，说明候选空间构建能同时改善效率和准确率。
5. 尽管 AgentWeave 成功率相对提升，但绝对值仍较低（12.5%），说明在 BFCL V4 多函数任务上，1.5B 级别模型即使经过路由也面临较大难度，路由只能部分缓解。
6. 统计检验支持差异的可靠性：置信区间下限为正（+4.17 个百分点），McNemar p=0.03125 小于 0.05，说明成功差异不是偶然。
7. 消融趋势：论文没有提供详细的消融实验（从提供的证据看），但通过对比 all-tools、随机、语义三种基线，可以间接看出路由信号（资格、需求、能力等）起关键作用。
8. 负面/边界观察：论文承认“不确立 AgentWeave 普遍优于语义检索”，因此结果可能只在特定任务分布和路由压力协议下成立。
9. 值得注意的是，baseline 的 0% 成功可能暗示任务本身难度极高，或是评估协议过于严格（例如要求完美函数签名），这使对比出现地板效应，需要谨慎解读绝对提升幅度。

Q6: 有什么可以进一步探索的点？

1. 探索更复杂的路由策略：当前 AgentWeave 是确定性规则，可研究在路由层引入轻量级学习（如排序模型或强化学习）以进一步压缩候选集，同时保持可解释性。
2. 扩展到更多基准和任务：在完整的 BFCL 排行榜、多轮函数调用、混合工具与真实 API 场景中验证 AgentWeave 的通用性。
3. 研究路由与模型微调的协同：如果将路由层与定向微调或指令调优结合，可能带来更大增益。
4. 分析不同信号（eligibility, requirement, capability, routing）的贡献权重，设计消融实验以识别关键因素。
5. 端到端代理系统集成：将 AgentWeave 嵌入实际 Agent 平台（如多智能体框架），评估其在真实任务中的端到端成功率、延迟和成本。
6. 动态预算调整：自适应地确定每个请求暴露多少工具，例如根据复杂度或不确定性调整 top-k 大小。
7. 处理更细粒度路由：从工具级路由扩展到函数参数级路由，或对相似工具进行分组并代表化。
8. 与语义检索的混合方法：论文未声称优于语义检索，未来可探索确定性规则与语义相似度结合的混合路由。
9. 扩展到更大的模型：在 7B、70B 或商用模型上验证路由收益，观察模型能力与候选集大小的交互。
10. 理论理解：研究为什么过度暴露工具会降低模型表现，是否源于注意力稀释、schema 干扰或决策复杂度。

Q7: 总结一下论文的主要内容

本文研究大语言模型函数调用中的候选空间构建问题。随着工具、函数、API 和专用智能体数量激增，模型需要处理更多 schema、消耗更多 token，并区分相似或无关选项。作者提出 AgentWeave，一个确定性的预推理路由层，放置在源工具目录与现有模型之间，利用资格、需求、能力和路由信号构建一个有界的模型可见动作空间。关键思想是将函数调用视为两阶段系统：先构建候选集，再由模型推理选择；路由层不改变模型，而是优化模型的输入。

实验采用 BFCL 派生的路由压力协议，使用公开的 MadeAgents/Hammer2.1-1.5b 模型，在 48 个新鲜 BFCL V4 多函数任务上，AgentWeave 达到 6/48（12.5%）原生成功，而 all-tools、确定性随机 top-8 和语义 top-8 均为 0/48。配对差异 +12.5 个百分点，bootstrap 95% CI [+4.17, +22.92]，McNemar p=0.03125。同时，AgentWeave 比 all-tools 减少 70.18% 工具、节省 61.70% 输入 token、延迟降低 50.95%。

论文明确声明这是路由压力研究而非官方排行榜，绝对成功低，不证明普遍优越性。主要贡献在于提供了一个新视角：候选空间构建可以显著影响固定模型的函数调用行为，且与效率收益正相关。论文还修正了“更多信息总是更好”的直觉，展示了过度暴露工具的负面效果。局限性包括：只有 48 个任务，模型规模小，路由信号未公开具体实现细节，基线选择可能影响对比。总体上，这是一个小而聚焦的系统性工作，为函数调用中的预推理路由打开了研究方向。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对智能体研究方向相关度较高：工具选择是 Agent 核心能力，预推理路由是一种轻量级模块，容易集成到主流 Agent 框架。

## 基本信息

- 作者：Saurav Singla, Aarav Singla, Advik Gupta, Parnika Gupta
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-08-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.23078v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本生成基于 PDF 语义检索证据（retrieved_evidence）并结合摘要和结论片段，部分细节（如路由公式、具体预算值）因原片缺失而标注为合理推断或需原文确认。
