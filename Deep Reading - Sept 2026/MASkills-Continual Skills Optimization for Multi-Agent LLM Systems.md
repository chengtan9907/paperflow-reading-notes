---
user_id: "cheng tan"
paper_id: 10400
arxiv_id: "2609.02094v1"
title: "MASkills: Continual Skills Optimization for Multi-Agent LLM Systems"
institution: "Arizona State University（亚利桑那州立大学；作者 Huaiyuan Yao、Xiaoou Liu、Hua Wei；另有多位作者所属机构在现有证据中未标明）"
publish_date: "2026-09-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.02094v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.02094v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-05T01:29:39"
---
# MASkills: Continual Skills Optimization for Multi-Agent LLM Systems

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multi-agent llm systems · continual learning · agent skills · skill-conditioned credit assignment

## 一句话总结

MASkills 提出一种在技能空间（skill space）而非参数空间中持续优化多智能体 LLM 系统的语言化持续学习框架，通过技能条件信用分配、层级信用聚合与动量平滑优化，让技能库经由精炼、归纳、整合与剪枝不断演化，并在 HotpotQA、LoCoMo、GAIA 上验证了效果。

## 摘要

> LLM-based multi-agent systems have shown strong performance on complex tasks, yet continual improvement from interaction experience remains challenging. Existing self-reflection methods build experience memories, but memories are mostly hard to invoke, refine, or scale, while agent skills offer a more actionable unit: structured procedural knowledge that specifies when to act, how to act, and which resources or tools to use. We introduce MASkills, a continual learning framework that optimizes multi-agent LLM systems through agent skills. MASkills presents a new agent-optimization pipeline that integrates skill-conditioned credit assignment, hierarchical credit aggregation, and momentum-smoothed optimization, enabling agent skill libraries to evolve through refinement, induction, consolidation, and pruning. Experiments on HotpotQA, LoCoMo, and GAIA demonstrate the effectiveness of MASkills across multiple agentic tasks. Our code is available at https://github.com/DaRL-GenAI/MASkills

Q1: 这篇论文试图解决什么问题？

论文关注的核心问题是：多智能体 LLM 系统能否以及如何从自身交互经验中持续改进，而无需重训练模型参数。具体拆解如下：1) 现有基于自反思的经验记忆方法虽然能积累经验，但记忆通常是隐式、难以按需调用、难以细粒度修正，也难以随规模增长而保持可扩展；2) 多智能体系统的策略改进缺乏可解释、可组合、可演化的载体，参数空间优化不适用于闭源或黑盒 LLM，也不利于跨任务迁移；3) 已有 agent 技能概念多为静态预定义或一次性学习，缺乏闭环的评估、信用分配与技能库动态管理机制；4) 在多智能体场景中，团队级奖励难以归因到每个智能体和每条技能，因此需要新的信用分配与聚合方式。论文把问题表述为：如何在去中心化执行、共享团队目标、协作式 Dec-POMDP 设定下，通过技能层面的持续优化实现单智能体策略与团队整体行为的共同改进，并同时保证鲁棒性和泛化能力。检索证据显示，论文将任务实例化为 cooperative Dec-POMDP 环境，智能体共享团队级目标、各自拥有局部观测空间，这暗示其问题框架带有明显的多智能体强化学习形式化色彩。需要说明的是，当前证据仅覆盖摘要与部分章节，完整的问题动机与批评性比较（如与 prompt 优化、memory 压缩方法的具体差异）仍需原文确认。

Q2: 有哪些相关研究？

根据摘要与检索片段可归纳的相关工作脉络包括：1) LLM 多智能体系统：多智能体通过角色分工、协调与长程交互解决复杂任务，但普遍缺乏持续改进机制；2) 自反思 / 经验记忆方法：智能体从错误中反思并将经验存入记忆，问题在于记忆难以可靠唤起、难以修正且扩展性差；3) agent skills / 程序性知识：技能作为‘何时行动、如何行动、使用哪些工具’的结构化单元，比记忆更可操作，但多数工作未形成闭环的持续优化与技能库演化机制；4) 语言空间中的策略优化：论文将技能空间目标实现为‘语言空间中的策略梯度类比’，即去中心化 rollout 产生技能痕迹，语言批评者输出逐智能体、逐技能的反馈，暗示其与 textgrad、反射式策略优化等语言反馈优化路线相关，但检索证据未列出具体引文，无法逐一确认；5) 经验回放与持续学习：论文强调 continual improvement，可能借鉴持续学习中技能库的保留、整合与剪枝思想，但同样缺乏直接文献证据；6) 多智能体强化学习中的信用分配：技能条件信用分配与层级信用聚合属于 MARL 信用分配问题的语言化版本。现有证据不足以覆盖完整 Related Work 章节，以上仅为基于框架术语的合理推断；如需严格定位本工作与 Reflexion、EXPEL、ADAS、TextGrad、Voyager 等具体方法的关系，应回读原文第 2 节。

Q3: 论文如何解决这个问题？

MASkills 的核心思想是将多智能体 LLM 系统的持续改进从‘记忆’转向‘技能’，并从参数空间转向技能空间，实现一个闭环的语言化策略优化流程。依据摘要、引言与方法片段，可重建如下技术路线（部分为合理推断）：1) 形式化设定：所有任务被实例化为协作式 Dec-POMDP 环境，团队由多个专门化 LLM 智能体组成，去中心化执行并共享一个团队级目标；每个智能体有局部观测空间，并在潜在策略推理中调用可复用的技能工件（reusable skill artifacts）。2) 技能表示：技能被定义为结构化程序性知识，包含触发条件（when）、行动方式（how）与所需资源/工具（which tools），这使它比自然语言记忆更可执行、可检验和可编辑。3) 技能条件信用分配（skill-conditioned credit assignment）：系统需要判断团队级结果中哪些智能体、哪些技能真正导致了成功或失败；由于是语言化框架，这一步很可能依赖语言批评者（language critic）对 rollout 轨迹逐技能给出评价。4) 层级信用聚合（hierarchical credit aggregation）：将不同层级（智能体级、任务级、团队级）的信用信号进行聚合，以稳定技能更新的目标。5) 动量平滑优化（momentum-smoothed optimization）：对技能修改方向或技能评分做动量平滑，避免单次 rollout 的噪声导致技能库震荡。6) 技能库演化机制：技能库持续经历四种操作——精炼（refinement，修改已有技能细节）、归纳（induction，从成功/失败轨迹中提炼新技能）、整合（consolidation，合并冗余技能或抽象通用技能）、剪枝（pruning，删除无效或过时技能）。7) 闭环流程：论文将技能空间目标实现为语言化策略梯度类比，具体流程为‘去中心化 rollout 产生技能痕迹；语言批评者发出逐智能体逐技能的反馈；系统据此更新技能库’，再进入下一轮 rollout，形成持续改进循环。8) 与执行解耦：技能更新不修改 LLM 权重，因此适用于黑盒或仅 API 访问的模型。需要提醒：检索证据只提供了第 4 节框架的很少文本，关于四个演化操作的具体触发条件、动量平滑的数学形式、技能间的版本控制、以及‘技能痕迹（skill traces）’如何编码，均需进一步查阅原文 Method 与 Framework 部分确认，不应视作已被完整验证的全部细节。

Q4: 论文做了哪些实验？

依据检索证据，实验章节围绕三个研究问题展开，但仅确认了 RQ1 和 RQ3 的完整表述，RQ2 内容缺失。已知实验安排如下：1) 数据集与任务：论文在 HotpotQA、LoCoMo、GAIA 上进行评估，覆盖多跳问答、长上下文对话记忆、以及涵盖工具使用与推理的通用智能体任务。2) 环境设定：所有任务被实例化为 cooperative Dec-POMDP 环境，团队由专门化 LLM 智能体组成，去中心化执行，共享团队级目标；每个智能体维护局部观测空间。3) RQ1（任务性能）：持续技能优化是否能在多样化任务上提升多智能体 LLM 系统的性能？哪些组件贡献最大？4) RQ2（推断为技能库演化或样本效率相关）：其问题文本未在检索结果中出现；按上下文推测可能涉及技能库的质量、规模变化或训练样本效率，但应视为推测。5) RQ3（鲁棒性与泛化）：评估 MASkills 是否能在不同协调结构和不同底层 LLM 主干下泛化。6) 拓扑鲁棒性实验：在 centralized、decentralized peer、hierarchical 三种通信拓扑下实例化 MASkills，以观察框架对协调结构的适应性。7) 模型鲁棒性实验：更换底层语言模型主干，检验技能库与优化流程是否迁移到不同 backbone。8) 组件/消融分析：根据 RQ1 的表述可知存在组件归因实验，但具体消融对象和对照组未在证据中给出。9) 对照方法、评价指标、训练轮次、超参数等均未在检索片段中出现。建议把实验细节缺失当作明显信息缺口；在精读原文第 5.1–5.4 节时优先补齐。

Q5: 发现了什么实验现象？

由于检索证据只覆盖了实验章节的少量片段，能可靠陈述的实验观察非常有限，主要如下：1) 框架整体有效：MASkills 在 HotpotQA、LoCoMo、GAIA 上展示出有效性，说明技能空间的持续优化能提升多智能体系统性能（依据摘要）。2) 拓扑鲁棒性的初步观察：MASkills 在 centralized、decentralized peer、hierarchical 三种协调结构下均可被实例化，暗示该方法的信用聚合与技能演化机制并不绑定于单一通信拓扑；不过证据只说明框架‘可以实例化’，未给出不同拓扑下的相对性能优劣，不能推出‘同样好’的结论。3) 对底层模型主干的适应性：实验考虑了不同 LLM backbone，但具体趋势与是否出现性能回退未知。4) 组件贡献：论文声称通过 RQ1 研究哪些组件贡献最大，但没有给出消融结果数值，无法判断技能归纳与精炼哪个更关键。5) 任何反直觉结果、失败案例、负结果、scaling trend 或指标间张力在检索证据中均不存在。若你关注‘经验记忆 vs 技能’的对比、技能库规模增长曲线、以及跨 backbone 的性能保持情况，这些是关键但仍在证据覆盖之外的观察点，需要回原文实验部分核实。

Q6: 有什么可以进一步探索的点？

论文自身指出的未来方向结合框架的可扩展性，可以归纳出以下可探索点（前两项来自 limitation 片段，后几项为合理推断）：1) 自适应组织结构：将 MASkills 从固定角色与固定通信拓扑扩展到能随任务动态调整组织结构的场景。2) 竞争性多智能体博弈：当前聚焦协作设置，可推广到竞争或混合动机场景，此时信用分配与技能共享将面临对抗性挑战。3) 大规模去中心化协调：研究技能库在大量智能体、长程任务下的扩展性，包括技能冲突、重复归纳与通信成本。4) 技能库的元优化：探索如何自动决定精炼、归纳、整合、剪枝四种操作的触发时机与频率，形成技能库的‘元策略’。5) 技能的可解释审计与安全：引入技能版本追踪、回滚与影响评估，防止技能库演化引入系统性错误或越狱风险。6) 跨任务迁移与泛化：研究从 HotpotQA 学到的技能能否迁移到 LoCoMo/GAIA 以外的领域，并设计技能复用度与迁移收益的度量。7) 与参数级训练结合：探索技能空间优化与 LLM 微调、LoRA 等参数级更新的协作方式，形成双层持续学习。8) 更复杂的执行模型：去中心化异步执行、动态智能体加入/退出、以及人类-AI 混合团队中的技能学习。9) 评价协议：为技能库演化设计过程指标（如技能命中率、修改成功率、剪枝误伤率），而不只依赖任务最终性能。注意：除前两项外，其余均未在检索证据中直接出现，属于基于框架结构的合理推断。

Q7: 总结一下论文的主要内容

MASkills 旨在解决一个在 LLM 智能体系统中日益重要但尚未被很好回答的问题：多智能体系统如何从自身交互经验中持续改进？论文指出现有自反思方法主要构建‘经验记忆’，然而记忆在调用、精修与规模扩展上存在结构性困难；相比之下，‘技能’作为结构化程序性知识（说明何时行动、如何行动、使用何种资源或工具）是更可操作的策略载体。由此，论文提出 MASkills——一种语言化的持续学习框架，其关键主张是把多智能体 LLM 系统的策略改进从参数空间转移到技能空间：在去中心化执行过程中，每个智能体在其潜在策略推理里调用可复用的技能工件，而系统则通过一个闭环的优化流程持续演化这些技能。技术路线上，MASkills 提出一个全新的智能体优化流水线，整合三个组件：技能条件信用分配，负责把团队级结果归因到具体智能体与具体技能；层级信用聚合，负责在不同层级上汇总信用信号以提高稳定性；动量平滑优化，负责抑制单次 rollout 噪声对技能库更新的扰动。技能库本身通过四种操作演化：精炼（改进已有技能）、归纳（从成功与失败轨迹中提取新技能）、整合（合并冗余或抽象通用技能）和剪枝（删除无效技能）。论文将该过程表述为语言空间中的策略梯度类比：去中心化 rollout 产生技能痕迹；语言批评者给出逐智能体、逐技能的反馈；技能库据此更新，再进入下一轮迭代。由于这种优化完全发生在语言与技能层面，不修改模型权重，因而适用于黑盒或仅通过 API 访问的 LLM，也天然支持跨模型主干使用。实验方面，论文将任务实例化为协作式 Dec-POMDP 环境，由专门化 LLM 智能体组成团队，在去中心化执行下共享团队级目标。评估在 HotpotQA、LoCoMo 和 GAIA 三个基准上进行，覆盖多跳问答、长对话记忆与通用智能体问题求解等类型。实验组织围绕三个研究问题：RQ1 考察持续技能优化能否提升任务性能并识别贡献最大的组件；RQ2 的具体表述未在检索证据中出现，但其存在表明论文还关注技能库演化过程或学习机制方面的追问（具体待原文确认）；RQ3 考察鲁棒性与泛化，包括在不同协调结构（centralized、decentralized peer、hierarchical）以及不同底层语言模型骨干下是否成立。论文的整体论证主线是：先指出记忆作为经验载体的不足，再论证技能是更可行动、可演化、可分配信用的单元；接着提出技能空间的持续优化框架并详细设计其信用分配、聚合、平滑与技能库演化机制；最后通过多种任务、多种拓扑与多种模型骨干的实验证明该框架的有效性和鲁棒性。需要说明的是，当前读取到的 PDF 证据仅覆盖引言、第 4 节框架与第 5 节实验的少量片段，大量诸如具体基线、消融数值、训练预算、技能表示模板、批评者实现细节等内容尚未被检索到，因此以上总结中凡超出摘要和片段的部分（如四种技能操作的触发机制、语言批评者的构造等）属于对框架术语的合理推断，而非对原文文本的逐条复述。总体看，这是一篇把‘技能’从静态工具提升为动态持续学习单元、并以语言化策略梯度方式组织多智能体优化的系统性工作，架构设计清晰，与记忆式自反思形成鲜明对照。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与你的研究方向 agent（权重 0.10）直接相关：论文处理的是多智能体 LLM 系统的持续改进，属于 agent 学习与自适应方向。

## 基本信息

- 作者：Huaiyuan Yao, Xiaoou Liu, Charles Fleming, Tianlong Chen, Hua Wei
- 机构：Arizona State University（亚利桑那州立大学；作者 Huaiyuan Yao、Xiaoou Liu、Hua Wei；另有多位作者所属机构在现有证据中未标明）
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-09-02
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.02094v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次回答基于论文摘要、元数据以及 PDF 语义检索命中的 21 个片段（主要来自 Introduction、4 Framework、5 Experiments、5.5 Robustness、Limitations、Conclusion）生成，已优先采纳检索证据并对证据未覆盖的方法细节与实验数值明确标注为推断或信息缺口。
