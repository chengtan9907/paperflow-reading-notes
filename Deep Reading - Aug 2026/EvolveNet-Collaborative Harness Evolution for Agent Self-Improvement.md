---
user_id: "cheng tan"
paper_id: 6907
arxiv_id: "2608.04968"
title: "EvolveNet: Collaborative Harness Evolution for Agent Self-Improvement"
publish_date: "2026-08-06"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.04968.pdf"
pdf_url: "https://arxiv.org/pdf/2608.04968"
abs_url: "https://arxiv.org/abs/2608.04968"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-06T11:20:50"
---
# EvolveNet: Collaborative Harness Evolution for Agent Self-Improvement

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm agent · harness evolution · collaborative evolution · program aggregation

## 一句话总结

EvolveNet 提出一种协作式 harness 演化范式，将经验提取从中心优化器下沉到数据本地的代理部署，只把各部署学到的程序改编聚合回共享 harness 并重新分发，从而在不更新模型权重的情况下让多个代理共享彼此的操作经验，并在五个基准上一致取得提升。

## 摘要

> The capabilities of an LLM agent depend not only on its model but on the harness: the executable program that constructs context, invokes tools, verifies results, and recovers from failure. Recent work shows that evolving the harness yields persistent improvements without updating model weights. Existing approaches, however, assume that all execution experience can be routed to a single optimizer, which evolves one harness along a sequential trajectory. Real agent ecosystems violate that assumption: users, organizations, and environments generate isolated streams of experience that cannot be pooled, so the experience most worth learning from is exactly the experience that cannot be directly centralized. We introduce EvolveNet, a paradigm of collaborative harness evolution that moves experience extraction to the data. A shared harness is broadcast to data-local agent deployments, each of which evolves it on its own workload. Only the resulting program adaptations are composed into an updated shared harness and redistributed, so that every participating agent inherits operational experience discovered by the others. By shifting the aggregation boundary from raw workloads to learned adaptations, EvolveNet keeps workloads local and allows multiple evolutionary searches to proceed concurrently with reduced serial depth. Because independently modified programs cannot be averaged like model parameters and may conflict when composed, EvolveNet introduces scope-typed, evidence-guided program aggregation. Across five settings spanning text-to-SQL, data-science coding, competitive programming, software engineering, and agentic workflows, EvolveNet improves the shared harness in all five, with the largest gains under heterogeneous workloads, and ablations attribute the improvement to composition of adaptations from different agents rather than to selecting among them.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：在真实代理生态中，如何让多个无法共享原始数据的代理共同改进同一个可演化的 agent harness，并避免集中式 harness 演化范式所固有的假设失效。具体而言，问题包含三层：

1. 集中式假设与真实生态冲突：现有 harness evolution 方法（如 He et al., 2025; Lee et al., 2026; Zhang et al., 2026; Nie et al., 2026）假定可以把所有 workload、执行轨迹和行为反馈集中路由到单一优化器，由它沿一条串行轨迹演化一个 harness。但用户、组织和环境各自产生隔离的经验流，出于隐私、合规、带宽或所有权原因无法直接汇聚；换言之，最值得学习的经验恰恰是那些无法被直接集中的经验。

2. 程序不可平均且组合会冲突：即便把演化任务下放到数据本地，各代理独立修改出的 harness 程序也不能像模型参数那样直接取平均或加权融合。程序是离散符号对象，独立演化出的两个版本可能在组合时发生语义冲突、接口不匹配或行为退化。因此需要设计一种不依赖数值平均、能处理组合冲突的聚合机制。

3. 协作中的信息不对称与证据不足：当只允许跨边界传输程序文本、相对公共基线的改动（delta）以及逐项行为判定（per-item behavioral verdicts）时，聚合方缺乏对每个改动背后完整上下文的理解，如何依据有限证据判断一个改编是否值得纳入共享 harness，是一个关键挑战。

论文试图通过把聚合边界从原始工作负载上移到“学到的改编”，并借助 scope-typed、evidence-guided 的程序聚合，让多个演化搜索并发进行、减少串行深度，同时保持工作负载本地化，从而在“隐私/数据隔离”与“共同进化”之间取得平衡。

Q2: 有哪些相关研究？

根据摘要和检索到的引言片段，相关研究可分为以下几层：

1. Harness evolution（harness 演化）是近期兴起的方向：已有工作（He et al., 2025; Lee et al., 2026; Zhang et al., 2026; Nie et al., 2026）表明，演化可执行的 harness（而非更新模型权重）能带来持续改进。这些工作构成了 EvolveNet 的直接基线。检索证据指出它们“largely follow a centralized paradigm”，即 workload、执行轨迹和行为反馈必须集中到单一优化器，忽略真实生态中经验流隔离的问题。

2. Agent self-improvement 的广义研究：代理能力不仅来自模型，还来自上下文构造、工具使用、结果校验和失败恢复等程序环节；这类研究与测试时计算、agent 记忆、工具学习等方向相互交叉。EvolveNet 属于不更新权重、通过修改程序来提升代理的一支。

3. 联邦/协作式学习的范式联系：虽然论文没有直接引用 federated learning，但“共享工件广播-本地演化-仅聚合改编”的结构与联邦学习的思路有相似之处；区别在于这里聚合的不是参数而是程序文本，且因程序不可平均而需要专门设计聚合规则。这一联系属于合理推断，原论文摘要未展开。

4. 多代理协作的机制差异：检索到的局限片段明确说明，EvolveNet 的协作是“由工件中介的，而不是交互式的”——客户端在一轮内既不互相通信也不协调，只有通过共享工件才能获得他人经验。这与多智能体对话、议会式辩论、共享记忆等交互式协作范式形成对比；相关工作应放在“artifact-mediated collaboration”这一更细的脉络中。

由于本文可获取的全文内容有限，related work 部分只能依靠摘要和片段归纳；具体方法名、各方法的优缺点和对比实验细节需要回原文确认。

Q3: 论文如何解决这个问题？

EvolveNet 的解决方案在框架层面包含四个关键设计：

1. 协作式 harness 演化范式（collaborative harness evolution）：把经验提取从中心优化器移到“数据侧”。一个共享 harness 被广播给多个数据本地的代理部署，每个部署在自己的 workload 上独立运行演化。每个参与代理只负责自己本地数据的经验提取，原始数据从不离开本地。

2. 边界传输的最小信息协议：跨边界传播的不是原始 workload 或执行轨迹，而是（a）程序文本本身，（b）相对公共基线的改动 delta，以及（c）逐项行为判定（per-item behavioral verdicts）。这样既保留了判断一次修改是否有效所需的证据，又把敏感数据留在本地。

3. 共享工件的聚合与再分发：各本地部署演化后产出的程序改编被聚合为一个更新后的共享 harness，再次分发给所有参与代理。这样每个代理都能继承其他代理发现的操作经验（operational experience），且由于只有改编向上汇聚，本地 workload 的隐私边界得以维持。

4. scope-typed、evidence-guided 程序聚合：这是处理“程序不可平均”的关键机制。EvolveNet 为每个改编赋予作用域类型（scope type），并利用逐项行为判定作为证据来引导聚合，从而判断不同代理的改编是否可以安全组合、是否冲突、谁的经验更适用于共享 harness。该机制使多个演化搜索可以并发进行，显著降低串行深度。

5. 并行性与组合优于选择的设计取向：论文刻意让多个演化搜索同时推进，而不是把所有经验串行地喂给单一优化器；消融表明，改进来源于把不同代理的改编组合起来，而非从多个候选中挑选一个最佳。这意味着聚合规则倾向于保留和融合多方知识，而不是做 top-1 选择。

需要说明的是，检索到的片段只给出了框架层面的描述，scope-typed aggregation 的具体实现（如作用域如何定义、证据如何加权、冲突如何消解）在本文证据中未完全展开，属于需要阅读原文方法章节确认的细节。

Q4: 论文做了哪些实验？

论文在五个设置上验证了 EvolveNet，每个设置对应一个代表性 benchmark：

1. 文本到 SQL：BIRD。
2. 数据科学编码：DS-1000。
3. 竞赛编程：LiveCodeBench。
4. 软件工程：SWE-bench。
5. 代理工作流：ClawEval。

主结果显示，协作式演化在全部五个设置上都改进了共享 harness，具体提升为：BIRD 提升 13.4 分，DS-1000 提升 13.0 分，LiveCodeBench 提升 33.4 分，SWE-bench 提升 20.0 分，ClawEval 提升 8.3 分；每个提升在对应基准上都通过了配对检验，说明差异在统计上显著。

论文还专门考察了异构工作负载场景。摘要和引言片段都强调“在异构工作负载下增益最大”，并提到“当各部署拥有互不重叠的专业知识时（disjoint forms of expertise）增益最大”。这提示实验设计很可能包含同构 vs 异构部署对比，即各本地 workload 分布相同时效果较小、分布差异大时效果更明显。

消融实验用于归因收益来源：paired ablations 表明，改进来自不同代理改编的“组合”（composition），而不是“选择”（selecting among them）。也就是说，如果仅仅从多个本地演化结果中挑一个最好的，收益会显著下降。

由于本文可获得的证据仅限于摘要加若干片段，实验部分缺少以下细节：每个 benchmark 使用的具体 harness 初始版本、基线方法（如集中式 harness evolution 的复现）、参与部署的数量、演化轮数、数据划分方式、消融的具体配置、以及与直接集中式方法的等资源对比。这些都需要回原文补充。

Q5: 发现了什么实验现象？

从现有证据可归纳出以下实验现象：

1. 一致性提升：EvolveNet 在全部五个 benchmark 上均改善了共享 harness，说明“数据本地演化 + 改编聚合”的框架在非常不同的任务族（SQL、编码、竞赛、软件工程、工作流）中都能泛化，而不是只对某一类任务有效。

2. 异构负载是增益放大的条件：最大的改进出现在异构工作负载下，尤其是各部署拥有“不相交的专业知识”时。这支持了论文的核心论点——如果经验可以轻易集中，集中式方法或许够用；恰恰是隔离且互补的经验流让协作式演化带来额外收益。

3. 组合优于选择：消融实验把收益归因于不同代理改编的组合，而非在多个改编中做挑选。这是一个反直觉的发现：它暗示共享 harness 的改进不是“找一个专家版本”，而是“把多个专家的局部修补拼接起来”——这为程序聚合机制的存在提供了直接证据。

4. 统计显著性普遍成立：每个 benchmark 的提升都通过了配对检验，说明 13.4、13.0、33.4、20.0、8.3 这些增量不是噪声，尤其 LiveCodeBench 上 33.4 的涨幅表明竞赛编程任务的 harness 还有很大优化空间。

5. 需要谨慎解读的部分：片段中没有报告“集中式基线在相同数据可见性下的表现”，也没有报告通信/计算开销、聚合冲突率、失败案例等指标。因此现象层面仍有空白：哪些类型的改编容易冲突、哪些任务上聚合增益较小（例如 ClawEval 只提升 8.3）都值得追问。

Q6: 有什么可以进一步探索的点？

基于论文的框架和已披露结果，可以从以下方向进一步探索：

1. 机制深化：scope-typed、evidence-guided 程序聚合的具体设计还有很大挖掘空间。例如：作用域类型如何自动推断？证据如何加权？当两个改编修改同一段代码时，有哪些冲突消解策略？是否可以引入可学习的聚合器？

2. 异构性边界：论文在异构负载下获得最大增益，那么“异构度”是否存在最优区间？如果各部署的任务完全不相交，共享 harness 是否仍能有效迁移？如果部分重叠，会不会产生负迁移？这需要设计连续变化的异构度实验。

3. 交互式协作的加入：当前协作由工件中介，客户端不直接通信。未来可以探索“工件 + 一轮轮有界通信”的混合模式，看能否在保持隐私边界的同时减少聚合的盲目性。

4. 隐私与安全的正式化：论文用“数据本地化”回避了集中，但程序改编本身可能泄露数据信息。需要研究差分隐私、安全聚合或程序文本脱敏与 harness 演化效果的权衡。

5. 扩展到模型权重联合演化：既然 harness 可以协作演化，是否可以把 workflow 层面的改编与模型权重的联合微调结合起来，形成“程序 + 权重”的双层协作？

6. 收敛性与理论分析：多个演化搜索并发时，共享 harness 的收敛行为、振荡风险、以及不同聚合规则下的理论保证目前未见披露；这是一个值得建立理论模型的方向。

7. 失败模式与诊断：什么条件下聚合让共享 harness 变差？组合冲突的典型模式是什么？有没有“本地改进但全局退化”的情况？这些失败案例可以帮助设计更稳健的聚合规则。

8. 跨领域联系：这种“工件中介的协作”思路可以类比开放科学中的可重复工件、软件生态中的 package 组合、或众包程序合成，未来可借鉴各自的经验教训。上述方向中，除论文明确提到的并发搜索和串行深度减少外，其余均为基于框架的合理推测，具体可行性需要以原论文的完整实验为参考。

Q7: 总结一下论文的主要内容

EvolveNet 提出了“协作式 harness 演化”这一新范式，用于解决多代理环境中共享经验无法集中时的代理自我改进问题。论文首先指出，LLM 代理的能力不只取决于模型权重，还取决于 harness——那个负责构造上下文、调用工具、校验结果、失败恢复的可执行程序。近期研究已经证明，通过演化 harness 可以在不更新模型权重的情况下获得持续改进，但这些工作大多假设可以将所有 workload、执行轨迹和行为反馈集中到一个优化器，沿一条串行轨迹演化单一 harness。这一假设在真实世界中不成立：用户、组织和环境产生的是互相隔离的经验流，无法集中池化；而最值得学习的经验，恰恰是那些无法被直接集中的经验。

为了突破这一限制，EvolveNet 把经验提取的动作从中心下放到数据本地。具体地，一个共享 harness 被广播给多个数据本地的代理部署；每个部署在自己私有的 workload 上独立演化 harness。跨越数据边界的不是原始数据或执行轨迹，而是程序文本、相对公共基线的改动 delta、以及逐项行为判定。这些信息被送回聚合端，组合成一个更新后的共享 harness，再重新分发，使每个参与代理都能继承其他代理发现的操作经验。通过这种方式，EvolveNet 将聚合边界从“原始工作负载”上移到“学到的改编”，让 workload 保持在本地，并允许多个演化搜索并发推进，从而降低串行深度。

程序不是参数，无法求平均，独立修改的程序在组合时可能冲突。为此 EvolveNet 引入了带作用域类型、由证据引导的程序聚合（scope-typed, evidence-guided program aggregation）。聚合依据每个改编的作用域和逐项行为判定，决定如何安全地组合来自不同代理的修改；消融实验进一步表明，改进的收益来自“组合”而非“选择”——即把不同代理的改编融合起来，而不是从它们中挑一个最佳版本。

实验覆盖五个差异很大的场景：BIRD（文本到 SQL）、DS-1000（数据科学编码）、LiveCodeBench（竞赛编程）、SWE-bench（软件工程）、ClawEval（代理工作流）。主结果显示，协作演化在这五个基准上全部带来显著提升，涨幅分别为 13.4、13.0、33.4、20.0 和 8.3 个百分点，每个都通过了配对检验。论文还特别强调，最大增益出现在异构工作负载下，也就是各部署拥有互不重叠专业领域时。

论文的协作方式是“由工件中介”的：客户端之间不直接通信，也不在当前轮内协调，一个代理发现的经验要通过共享工件才能被其他代理获得。这使得框架适用面很广，但也排除了交互式协作带来的即时协调能力。综合来看，EvolveNet 的主要贡献在于：首次把 harness evolution 从单代理串行优化推广为多代理并行协作；设计了适用于“程序不可平均”场景的、带证据引导的聚合机制；并通过五个 benchmark 证明协作式演化在异构环境中比集中式假设更契合真实生态。需要指出的是，当前可获得的摘要和片段并未披露 scope-typed 聚合的具体实现细节、集中式基线的等资源对比、以及通信/计算开销；这些需要读取完整论文才能做出最终评价。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文主题与用户画像中的 agent 方向直接相关，尤其是“agent self-improvement”和“harness evolution”这一前沿问题。

## 基本信息

- 作者：Jun Nie, Yonggang Zhang, Qianshu Cai, Yiu-ming Cheung, Xinmei Tian, Bo Han
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG
- 日期：2026-08-06
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.04968`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 retrieved_evidence 中来自摘要、引言、结论、preliminaries 和 main results 的检索片段，并结合 heuristic_draft 与论文元数据进行了中文重写与补充推断。
