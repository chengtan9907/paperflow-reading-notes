---
user_id: "cheng tan"
paper_id: 7411
arxiv_id: "2608.09629v1"
title: "Rethinking Self-Evolving Agents: Do We Still Need Prescribed Optimization Pipelines?"
publish_date: "2026-08-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.09629v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.09629v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-12T01:23:09"
---
# Rethinking Self-Evolving Agents: Do We Still Need Prescribed Optimization Pipelines?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：self-evolving agents · open-ended optimization · prescribed pipelines · LLM as optimizer

## 一句话总结

本文提出开放式优化（OEO），在外部契约固定的前提下允许前沿大模型在线自主编排改进过程，通过14次头对头比较和多项对照实验，论证任务特定的规定管线并非必要，但该结论受优化器能力边界限制。

## 摘要

> Self-evolving agents are usually built around prescribed optimization pipelines: the framework decides how to gather evidence, revise a persistent artifact, select candidates, and stop. We ask whether this task-specific procedure remains necessary when a frontier model acts as the optimizer. We introduce Open-Ended Optimization (OEO), which keeps the objective, permitted interactions, resource budget, data boundary, and evaluation fixed while allowing the optimizer to compose the improvement process online. We compare OEO with two complementary prescribed approaches: SkillOpt, a staged pipeline with bounded edits, and GEPA, a reflective evolutionary search. Across 14 head-to-head comparisons over 8 benchmark-target-model settings, GPT-5.5-driven OEO records 12 wins, 1 tie, and 1 narrow loss of 0.21 percentage points. It uses a median 34.3 percent of SkillOpt's configured target-interaction token budget. A one-shot, zero-interaction control shows that the gains are not explained by a single prior-driven rewrite. However, delegation has a capability boundary: SkillOpt outperforms OEO with a medium optimizer, and a weak optimizer cannot operate through the unchanged OEO interface. In the fully instrumented OEO-SkillOpt pair, trajectory analysis further shows that prescription changes how optimization proceeds more consistently than it changes final behavior. Together, these findings recast prescribed pipelines as capability-dependent scaffolding: essential constraints remain external, but a sufficiently capable optimizer can compose the route from measurable feedback to persistent improvement.

Q1: 这篇论文试图解决什么问题？

该论文试图解决的核心问题是：在自进化智能体中，当优化器本身是能力足够强的前沿大模型时，框架预设的任务特定优化管线（prescribed optimization pipeline）是否仍然必要？具体来说，传统设计把“如何改进”这一高层策略也固化在框架中，框架需要决定如何收集证据（例如失败案例和外部反馈）、如何修订持久工件（如prompt、记忆、技能或工具调用策略）、如何在多个候选改进之间做选择，以及何时终止优化循环。这种预设流程虽然提供了稳定性和可解释性，但也引入了大量人工设计和任务特定假设，可能限制了优化器的自适应能力。

论文将问题进一步抽象为两层结构的分离：一是“外部契约”（external contract），包括优化目标、允许的交互方式、资源预算、数据边界和评估协议；二是“任务特定的元策略”（task-specific meta-policy），它组织改进过程的具体步骤。作者追问：在相同外部契约下，框架是否必须规定任务特定的元策略？换言之，是否可以把元策略的编排权完全交给一个足够强的前沿模型，让它在线自行组合证据收集、工件修订、候选选择和停止条件？这种思路的动机在于，前沿模型已经具备很强的规划、反思和工具使用能力，也许不再需要人类预先为每个任务设计详细的优化流程。

更深一层，论文还关注这种“委派”的能力边界：如果优化器不是前沿模型，而是中等或较弱能力的模型，那么完全开放的OEO接口是否还适用？传统规定管线作为“脚手架”的价值是否会随优化器能力变化而变化？实证结果显示，中等优化器下规定管线反而占优，弱优化器甚至无法操作OEO接口，说明流程委派应当以优化器能力为前提。这个问题的研究意义在于：它重新划分了智能体框架与模型能力之间的分工边界，为设计自适应于模型等级的自进化系统提供了经验指导。

Q2: 有哪些相关研究？

检索证据中，相关工作部分明确提到了“无需权重更新的持久改进”（persistent improvement without weight updates）这一研究方向，并列举了三个代表性系统：Reflexion通过自然语言存储自我反思，在解决新任务时重用这些反思；ExpeL从过往交互中抽取可复用的自然语言洞察，形成经验库；Voyager通过与环境交互逐步构建可执行的技能库，并用技能库推动探索。这些系统的共同点是将“如何从经验中改进”的过程以明确的算法结构内置于框架中，属于论文所说的“规定管线”范畴，但规定粒度各有不同。

论文选取了两个互补的优化管线作为对比基线：SkillOpt（分阶段管线，对编辑施加有界限制）和GEPA（反思式进化搜索，依赖反思和进化算子）。这两种方法在设计拓扑上差异很大——一种偏重有序阶段和受限修改，另一种偏重搜索与反思循环——因此可以检验OEO的结论是否依赖于特定的规定方式。

更广泛地说，LLM as optimizer 是近期热门方向，相关工作可能还包括基于LLM的prompt优化、代码搜索、自动提示生成等。但由于检索片段有限，论文中是否系统综述这些工作无法完全确认。可以合理推断，论文的对比设置有意覆盖了不同搜索拓扑，从而论证“OEO的结果不是针对某一种管线设计”。此外，与强化学习、上下文优化等方向也存在潜在联系，但证据不足，这里不展开。总体而言，该论文将自身定位在自进化智能体的“流程设计”层面，与直接调参式优化区分开，重点关注持久工件（技能、记忆、提示）的改进机制。

Q3: 论文如何解决这个问题？

论文的核心方法是提出开放式优化（OEO）框架，并将它与两种规定的优化管线进行系统性比较。OEO的运作原则是：外部契约（objective, permitted interactions, resource budget, data boundary, evaluation）保持完全固定，而优化器可以在线自行编排整个改进过程，包括如何收集证据、如何诊断失败、对持久工件做出哪些修改、如何选择候选、以及何时终止。换言之，OEO不是把“优化过程”写成固定代码，而是为优化器提供一个完全开放的接口，要求它在外部契约框架内自主规划并执行改进路线。

为了使对比具有说服力，论文选取了两个规定管线作为对照：
1. SkillOpt：分阶段管线，每个阶段规定具体操作，并对编辑做有界限制（bounded edits）。
2. GEPA：反思式进化搜索，通过反思和进化操作循环改进候选解。
这两种方法代表了“强过程规定”的两种不同拓扑，从而可以检验OEO的结论是否对特定管线设计敏感。

实验设计方面，论文采用多个基准-目标模型设置（8个设置，14次头对头比较），使用GPT-5.5作为OEO的优化器，与SkillOpt和GEPA分别比较。评估指标包括最终任务性能、优化过程中消耗的交互token总量等。为排除“先验驱动的单次重写”这一简单解释，论文设计了“一次性零交互对照”（one-shot, zero-interaction control）——即只允许优化器给定一次初始改写但不与后续反馈交互。

为了探索能力边界，论文还引入了中等能力和弱能力的优化器，考察同一套OEO接口是否仍然可用。最后，在完全仪器化的OEO-SkillOpt组合上进行了轨迹分析，统计优化过程中编辑幅度、对早期修改的修订次数，以及所选技能覆盖的样本范围，从而比较规定与开放策略对优化轨迹的影响。

Q4: 论文做了哪些实验？

根据摘要与检索证据，论文包含以下实验层次：

1. 主对比实验：在8个基准-目标模型设置下，对OEO与SkillOpt、OEO与GEPA进行14次头对头比较。优化器为GPT-5.5。结果为12胜1平1负，其中唯一一负是落后0.21个百分点（即GEPA唯一领先场景）。这个实验直接检验OEO是否能在相同外部契约下达到或超过规定管线。

2. Token预算测量：测量OEO实际消耗的目标交互token量，并与SkillOpt配置的目标交互token预算作比较。结果显示OEO的中位消耗仅为SkillOpt配置预算的34.3%，说明OEO在达到同等或更优性能的同时显著节约了交互开销。

3. 一次性零交互对照：该控制实验限制优化器只能进行一轮先验驱动的改写，不能与后续环境反馈互动。如果OEO的性能优势主要来自单次重写，那么控制组应接近OEO的表现；结果显示并非如此，从而说明OEO的收益来自在线的、多轮反馈驱动的过程。

4. 能力边界实验：使用中等能力优化器替换GPT-5.5，发现SkillOpt反而优于OEO；使用弱能力优化器时，模型无法通过未修改的OEO接口进行有效操作。这表明OEO不是无条件优于规定管线，其有效性依赖于优化器达到一定能力门槛。

5. 轨迹分析（OEO-SkillOpt对）：论文对8个设置下的OEO与SkillOpt优化过程进行了完全仪器化分析，统计了最大编辑幅度、对早期修改的撤销/修订次数、以及最终所选技能覆盖的样本重叠度。这部分实验旨在理解规定对优化过程本身而非最终结果的影响。

6. 论文还提到“我们进一步检验这个结果的两种解释”，说明在main结果之后设计了消融或机制验证实验，但具体内容在摘要和检索片段中未展开，需要查阅原文确认。

Q5: 发现了什么实验现象？

从已有证据中可以提取以下实验现象：

- OEO在绝大多数对比中胜出：在14次头对头比较中取得12胜1平1负，胜率约86%。唯一一次负场差距极小（0.21个百分点），表明即使失败也与规定管线十分接近。这支持了前沿优化器能够自行编排改进过程并达到甚至超过规定管线性能的结论。

- 显著的token效率优势：OEO只消耗了SkillOpt配置的目标交互token预算的中位数34.3%，却能达到与规定管线相当或更好的效果。这个反直觉的结果说明，开放式的过程编排不但没有浪费预算，反而可能通过更自主的路线规划避免了无效探索。

- 一次性零交互对照排除了“单次先验重写”的解释，说明OEO的多轮反馈循环是增益的核心来源，而非模型本身在一次提示中就能生成最优改进。

- 能力边界现象非常明显：优化器从中等切换到弱等时，OEO的优势迅速逆转甚至无法工作。这意味着OEO接口的设计对优化器的规划与执行能力有隐含要求，中等能力模型需要外部结构性支持（如SkillOpt）来补偿其自主编排能力的不足，而弱模型则可能需要完全不同的接口简化。

- 轨迹分析揭示了“路径-结果”分离：在所有8个仪器化OEO-SkillOpt设置中，OEO相比SkillOpt做出更宽泛的最大编辑，并且更频繁地修订自己早期的修改，但最终选定的技能往往解决重叠的样本。这说明规定管线在改变优化过程（技能空间中的轨迹）方面比改变最终行为更一致。换句话说，虽然两种策略走了非常不同的改进路径，最终达到的功能行为却高度相似。这一现象暗示自进化系统的最终行为可能主要由外部契约和任务本质决定，而过程中的规定主要影响探索形式和可解释性。

- 负结果方面：GEPA虽然在与OEO的对比中整体落后，但它的唯一领先场景的差距极小，说明反思式进化搜索在特定设置下仍有竞争力。同时，中等能力模型下的SkillOpt胜出构成了对“无规定”主张的重要限制。

Q6: 有什么可以进一步探索的点？

基于论文的核心发现和未解决问题，以下方向值得进一步探索：

1. 自适应程序支持机制：既然规定管线的价值随优化器能力变化，未来可以设计一种根据能力自动调节规定强度的机制——当检测到优化器在线规划表现不佳时，动态注入结构化的提示或流程片段，从而在开放性和稳定性之间取得平衡。

2. OEO接口的轻量化或分级化：弱优化器无法使用未修改的OEO接口，这提示可以设计分级的OEO接口（例如提供子目标提示、默认候选生成模板、或更细粒度的反馈指引），让弱模型也能部分受益于开放式优化，同时逐步训练其自主编排能力。

3. 外部契约的进一步自动化：论文假设外部契约固定不可变，但契约本身也是人为设计的。未来可以尝试让优化器参与契约修订，或学习最优的契约参数（如资源分配、数据边界），前提是保持一定的安全约束。

4. 更广泛的比较矩阵：目前主要对比了SkillOpt和GEPA两种规定管线。未来可以加入更多优化范式，如基于蒙特卡洛树搜索的算法、beam search式候选生成、多智能体辩论等，以检验OEO的普适性。

5. 轨迹分析的量化与因果验证：论文发现规定更一致地影响路径而非结果，未来可以进一步定义轨迹的拓扑指标（如编辑距离、技能覆盖率、自修订率），并在更多设置中建立路径-结果因果模型，甚至利用轨迹差异做可解释性分析。

6. 跨任务泛化：论文使用了8个基准-目标模型设置，但具体任务类型未在摘要中透露。未来可以在代码生成、数学推理、对话策略、科学实验设计等更多领域验证OEO，尤其可以探索其在AI for Science中的应用——例如自动优化实验协议或文献挖掘策略。

7. 安全与可控性：完全开放的自我优化可能带来不可预测的行为偏移。未来需要研究如何在保持开放性的同时确保外部契约不可侵犯，以及如何监控优化器在探索中是否违反隐含的伦理边界。

8. 多智能体协作场景：OEO的开放接口可以自然地扩展到多智能体系统中，让多个优化器共同改进共享工件或技能库，此时需要研究额外的协调机制是否构成新的规定层级。

Q7: 总结一下论文的主要内容

该论文重新审视了自进化智能体的传统设计原则。自进化智能体的目标是从经验中获得持久改进，而无需更新模型权重；常见的做法是让智能体修正其prompt、记忆、技能库或工具使用策略。过去的研究（如Reflexion、ExpeL、Voyager）把这种改进机制内嵌为框架的一部分，形成了“规定优化管线”：框架决定如何收集证据、何时诊断、如何修订工件、如何选择候选、以及何时停止。论文的核心发问是：当优化器本身变为能力极强的前沿大模型（例如GPT-5.5）时，这种任务特定的程序性脚手架是否已经变得多余？

为了回答这一问题，作者提出了开放式优化（Open-Ended Optimization, OEO）框架。OEO不预设任何任务特定的改进流程，而是将注意力集中在“外部契约”上：目标函数、允许的交互种类、资源预算（如token预算）、数据访问边界、以及评估协议全部由外部固定；优化器则在盒内自由组合出具体的改进过程——可以自行决定收集哪些证据、做何种修改、在多个候选之间如何权衡、以及何时收手。这种设计把“优化智能”完全交给了模型，而框架只负责约束问题的边界。

实验部分选取了两种互补的规定管线作为对照：SkillOpt（分阶段、有界编辑）和GEPA（反思式进化搜索）。作者在8个基准-目标模型设置上进行了14次头对头比较，统一使用GPT-5.5作为优化器。结果显示OEO以12胜1平1负的压倒性优势胜过规定管线，唯一的败仗仅以0.21个百分点微弱落后。更值得注意的是，OEO达到这一表现所消耗的目标交互token中位数仅为SkillOpt配置预算的34.3%，显示出显著的资源效率。一个一次性零交互的对照实验进一步证明，OEO的优势不能归因于简单的、由先验知识驱动的单次重写；它必须来自多轮交互反馈的动态优化。

然而，论文并没有止步于展示“开放优于规定”的简单结论。能力边界实验揭示了一个关键的对称性：当优化器换成中等能力的模型时，SkillOpt反超OEO；当优化器进一步降至弱模型时，它甚至无法操作OEO的未修改接口。这表明OEO对优化器的自主规划能力有隐含要求，规定管线恰恰充当了中等能力模型的有效记忆和过程支持，因此应当被理解为“能力依赖的支架”而非过时的负担。

论文还通过轨迹分析深入窥探了规定与开放两种模式的机制差异。在8个完全仪器化的OEO-SkillOpt对比中，OEO执行了更激进的大幅编辑，并且更频繁地回滚自己早期的修改；但最终选出的技能却与SkillOpt选出的技能高度重叠，解决了很多相同的样本。论文由此得出一个非常耐人寻味的结论：规定的管线主要改变的是优化器穿过技能空间的路径，而最终的评估行为却并不因为路径不同而显著分化。换言之，在外部契约和任务结构共同作用下，不同的过程编排可能收敛到相近的解决方案；规定更多地影响过程的可解释性、稳定性和调试难度，而非天花板性能。

综合来看，这篇论文通过严格的实验和细致的轨迹分析，将自进化智能体的设计讨论从“哪种流程更好”推进到“流程应该在什么条件下交给模型”。它的核心贡献在于提出OEO这一干净、可复用的开放范式，并给出了前沿模型下开放优于规定的强证据，同时也公正地划出了能力边界，避免过度宣称。对于构建下一代自适应智能体系统而言，这篇论文提供了一个重要的原则：外部契约与内部元策略的分离，以及根据优化器能力选择规定程度的设计准则。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接与智能体（agent）方向相关（用户画像权重0.10），特别是自进化智能体和LLM作为优化器的前沿问题，适合关注自主智能体的研究者。

## 基本信息

- 作者：Hui Xue, Fan Yang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.09629v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了论文摘要、引言、相关工作、讨论和结论的PDF语义检索证据，并结合元数据信息进行了归纳与推断。
