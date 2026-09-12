---
user_id: "cheng tan"
paper_id: 10424
arxiv_id: "2609.02309v1"
title: "Efficient GUI Agents: A Systems Survey of Observation, Memory, Action, and Runtime Optimization"
publish_date: "2026-09-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.02309v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.02309v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-05T01:32:50"
---
# Efficient GUI Agents: A Systems Survey of Observation, Memory, Action, and Runtime Optimization

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：gui agents · efficiency · observation efficiency · context memory management

## 一句话总结

该综述以端到端系统视角系统梳理 GUI Agent 的效率问题，将效率分解为观察、上下文与记忆、动作及规划器/系统四个技术轴，归纳出选择性读取、全局到局部视觉分配、可恢复记忆、验证感知控制与 GUI/非 GUI 混合运行时等收敛性机制，并指出验证器成本核算、跨基准可比性与低延迟隐私约束下分层协同设计等开放问题。

## 摘要

> GUI agents increasingly operate across websites, mobile apps, and desktop environments, yet the field still reports progress primarily through task success. We argue that practical deployment depends equally on efficiency: how much context, computation, action budget, and runtime overhead an agent consumes while succeeding. This survey studies efficient GUI agents through an end-to-end systems lens that preserves the current technical axes of observation efficiency, context and memory efficiency, action efficiency, and planner-side/system efficiency. For each subsection, we expand the seed literature through targeted search plus backward and forward citation chaining, then synthesize the dominant mechanisms, reported efficiency signals, and new overheads they introduce. Across the literature, recent progress converges on a small set of recurring ideas: selective reading instead of full-context ingestion, global-to-local visual allocation, recoverable memory rather than raw history replay, verification-aware control, and hybrid runtimes that can switch between GUI and non-GUI execution. We conclude by identifying the main open problems, including honest accounting of verifier cost, cross-benchmark comparability, and co-design of observation, memory, and execution layers under real latency and privacy constraints.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：GUI Agent（操作网站、移动应用和桌面环境的智能体）的研究与评测目前几乎只关注任务成功率，而忽略了实际部署中同样关键的另一维度——效率。具体而言，即使一个 GUI Agent 最终能完成任务，也可能消耗过多的上下文标记（token）、计算资源、交互步骤数和运行时开销，导致成本高、延迟大、难以落地。作者认为需要把效率作为与成功率并列的一等公民来研究。该问题可分为四个子问题：(1) 观察效率：Agent 如何以尽可能低的成本（如截图分辨率、DOM 解析量、视觉 token 数量）构建可操作的界面表示，同时不丢失关键信息；(2) 上下文与记忆效率：面对长序列 GUI 任务，如何在有限的上下文窗口内管理历史观察与动作，避免原始历史回放带来的冗余和遗忘；(3) 动作效率：如何用更少的动作步骤、更少的不可逆错误和更少的无效探索完成任务，即减少交互浪费；(4) 规划器/系统效率：模型推理之外的系统开销（如多次调用大模型、验证器额外推理、运行时调度）如何被压缩。论文明确指出，已有工作多在这些轴向上分别优化，但缺少统一系统视角下的梳理、对比与协同设计；更关键的是，这些优化往往引入新的隐性成本（如验证器开销、记忆压缩的重计算成本），若不诚实核算，所谓“高效”可能是假象。最后，跨基准效率指标缺乏一致性，导致不同论文的效率结论不可比。

Q2: 有哪些相关研究？

从摘要和检索到的片段看，相关工作可梳理为以下几类：
1. 传统 GUI 自动化与智能体基础：早期 GUI Agent 依赖可访问性树（accessibility tree）、DOM/HTML 解析或截图+视觉模型来构建界面表示，代表性工作如 Deng et al., 2023; Zhou et al., 2023; Rawles et al., 2024; Xie et al., 2024; Nguyen et al., 2024; Sager et al., 2025（引自引言，具体对应关系未在检索片段中完全展开）。这类工作奠定了观察表示与动作执行的基本范式。
2. 纯视觉 GUI Agent：如 Zhengxi Lu 等人 2025 的 Ui-R1，关注纯视觉 GUI agent 的高效动作预测（出自参考文献片段）——这属于观察效率与动作效率交叉的方向。
3. 推理与效率的平衡：如 Jiafu Chen 等人 2025c 的 “Difficulty-aware reasoning for mobile GUI automation”（出自参考文献片段），体现难度感知推理如何影响动作预测效率，尤其是长序列 GUI 任务中的推理管理。
4. 上下文与记忆管理：可恢复记忆（recoverable memory）而非原始历史回放的思想说明相关工作已探索基于记忆压缩、摘要或检索的上下文管理，但综述未提供具体文献名。
5. 验证与错误纠正：验证感知控制（verification-aware control）暗示相关研究涉及用验证器或自反思来减少不可逆错误，从而提升动作效率——这类工作在 GUI 自动化中常被称为“检查-执行”范式。
6. 混合运行时：GUI 与非 GUI 执行切换（如调用 API、命令行、脚本代替 GUI 点击）相关，类似于 tool-use agent 中的“用 API 替代 UI 操作”，在 RPA 和软件测试领域也有先例。
7. 系统与推理优化：规划器侧的高效推理、缓存、模型路由等属于 NLP/Agent 系统的常见技术，论文将其更名为“planner-side/system efficiency”纳入统一框架。
由于检索证据仅覆盖摘要、引言和参考文献片段，综述正文中各方法的系统对比与更细分类证据不足，需回到原文确认具体文献的归属与结论。

Q3: 论文如何解决这个问题？

论文采用的方法论是“系统化综述 + 引文链扩展 + 机制归纳”，而非提出新算法或新系统。具体做法分四步：
1. 提出端到端系统视角的分析框架：将 GUI Agent 视为一个完整系统，明确其功能流水线（pipeline）：先由观察模块从截图、DOM/HTML、可访问性树或混合解析输出中构建当前界面的可操作表示；随后由规划/推理模块基于该表示与记忆决定动作；动作执行模块负责点击、拖动、调用快捷键或切换应用（引自章节 2.2 证据）；最后可能经过验证或纠错环节。
2. 沿四个技术轴（观察效率、上下文与记忆效率、动作效率、规划器/系统效率）组织文献，而不是按基准或模型架构组织——这种切分反映效率问题发生在流水线的不同阶段，每个阶段有独特的开销来源和优化机制。
3. 对每个子主题采取“种子文献 + 定向检索 + 后向与前向引文链（backward/forward citation chaining）”的扩展策略，保证综述覆盖面既不过度依赖单一关键词，也能追踪思想演化的前驱与后继工作。
4. 综合时不仅记录方法，还记录(a)主导机制（如选择性读取、全局到局部视觉分配、可恢复记忆、验证感知控制、混合运行时），(b)论文报告的效率信号（如 token 减少量、步骤数减少、延迟下降），(c)引入的新开销（如验证器的额外推理成本、记忆恢复的计算成本）。这种“收益—新开销”双向记录是论文区别于普通分类综述的关键。
5. 最后以开放问题清单收束：验证器成本核算、跨基准可比性、真实延迟与隐私约束下的分层协同设计。这表明论文不只是文献堆砌，而是试图建立效率叙事的统一语言。
合理推断：综述可能包含一个“统一效率记账”的示意框架或表格，把观察成本、记忆读/写成本、动作成本、验证成本列成可对比的项（摘要中“honest accounting of verifier cost”支持此推断），但具体表格形式需原文确认。

Q4: 论文做了哪些实验？

作为一篇系统综述（survey），本文没有进行新实验。检索到的证据中没有出现数据集、基准或模型训练实验。从方法论描述可推断，论文的“验证”方式是：(1) 对种子文献集进行定向检索扩大范围；(2) 通过引文链（后向追踪旧文献、前向追踪施引文献）确保检索完整性；(3) 在不同子领域内对比多个研究报告中定性与定量的效率改善数据；(4) 归纳各方法引入的新开销并跨论文讨论。可能存在的“实验性”内容只可能是对文献数据的统计（例如各效率机制在不同基准上的报告值分布），但检索证据库的主干内容显示论文更像纯质性综述。因此，若读者期待数学模型或基准测试，本文不提供；其贡献是提供一个可复用的分析框架和分类体系。

Q5: 发现了什么实验现象？

从摘要和检索片段来看，论文没有直接实验，但作为综述可归纳出跨文献的一致性观察：
- 不同实现的技术路线多样，但功能流水线收敛于相同五段式：观察表示构建 → 决策 → 动作执行 → （可选）验证 → 记忆更新（由 2.2 节证据支持）。
- 即使模型推理本身效率高，GUI Agent 仍可能因浪费交互步骤而不可用（5 节证据），说明动作层面的浪费是一个独立于模型开销的效率瓶颈。
- GUI Agent 完成任务的过程常伴随大量交互与运行时开销，能力提升与成本上升并存（引言证据）。
- 领域内不断出现“纯视觉”“难度感知”“高效动作预测”等具体方案，说明观察成本与推理难度是当前活跃优化点。
- 新优化的引入常伴随新的隐性开销：作者特别警示验证器成本、压缩记忆的恢复代价，以及 GUI/非 GUI 切换本身的切换开销。
- 跨论文的效率数字因基准、环境、模型不同而难以直接对比，这是综述中反复出现的“指标内张力”。
由于无一手定量结果，此处无法报告具体数值趋势（例如某方法减少 token 多少百分比）。

Q6: 有什么可以进一步探索的点？

论文摘要明确列出三方面开放问题，结合系统视角可推导出更多可探索方向：
1. 诚实的验证器成本核算：许多 GUI Agent 采用验证或重新检查的机制（verification-aware control），这会增加额外的大模型调用或规则开销，但现有论文往往只报最终成功率而不报验证阶段的成本。未来应定义“验证税（verification tax）”式的指标，让读者看清为了纠错多付了多少推理预算。
2. 跨基准可比性：不同论文使用不同环境（WebArena/Mind2Web/AndroidWorld 等）、不同观察格式（截图 vs DOM）和不同成功判据，导致效率指标不可比。需要建立标准化的效率报告模板，至少统一报告 token 消耗（按观察/上下文/输出区分）、步骤数、耗时、失败恢复成本等维度。
3. 观察、记忆与执行层的协同设计：单独优化某一层可能在端到端上产生次优；例如更细的观察表示会加重记忆负担，纠错动作会增加记忆写入量。因此应在真实延迟（在线使用时的墙钟时间）和隐私约束（本地设备、不能上传截图）下联合优化各层。
4. 可恢复记忆的具体化：如何设计记忆压缩/摘要/检索引擎，使 agent 能快速恢复关键状态而不用回放全部历史？如何衡量“恢复质量 vs 存储成本”的帕累托边界？
5. 全局到局部视觉分配的自适应策略：如何根据任务难度和界面复杂度动态选择视觉精度，避免全局高分辨率编码带来的固定开销？
6. 混合运行时的切换策略：何时应放弃 GUI 交互改用 API/命令行？切换的触发条件、切换本身的开销、以及跨模态状态同步问题尚未被系统研究。
7. 隐私约束下的高效方案：本地小模型+选择性上传截图的方案能在多大程度上保持效率与成功率？联邦或端侧推理如何影响观察压缩策略？
8. 长序列 GUI 任务的规划效率：如何避免长任务中的重复规划与错误扩散，结合 difficulty-aware reasoning 来调节推理深度？
9. 效率感知的训练与评估：如何训练模型直接优化“效率 + 成功率”的联合目标，而非仅正确动作的交叉熵？
10. 统一效率记账的语言：是否可以定义一组标准化成本单元（如每次观察像素成本、每次工具调用的模型参数成本、每次验证的端到端延迟），供未来论文统一申报？

Q7: 总结一下论文的主要内容

本论文是一篇题为《Efficient GUI Agents: A Systems Survey of Observation, Memory, Action, and Runtime Optimization》的系统综述，目标人群是 GUI Agent 研究者和系统部署者。

论证主线：作者首先指出 GUI Agent 领域当前以“任务成功率”为核心汇报口径，但实际部署需要同时关注效率——即完成任务所消耗的上下文、计算、动作预算和运行时开销。这一论断将效率从“附加属性”提升为“核心评估维度”。论文提出应采用端到端系统视角，而不是孤立看待模型精度。

技术主线：论文将 GUI Agent 的工作机制概括为一个统一功能流水线（2.2 节证据）：观察模块负责从截图、DOM/HTML、可访问性树或混合解析中构建可操作的界面表示；决策与规划模块基于表示和记忆做出下一步动作决策（如点击、拖动、快捷键、应用切换）；动作之后可能还有验证/纠错模块；整个过程伴随上下文与记忆的管理。沿着流水线，效率问题被划分为四个技术轴：
- 观察效率：降低感知和界面表示的构建成本；
- 上下文与记忆效率：压缩与管理跨步骤的历史信息；
- 动作效率：减少完成任务所需的动作数、不可逆错误和无效探索；
- 规划器/系统效率：减少模型推理和系统性运行时开销（包括验证器、调度等）。

文献综合方法：对每个子主题，作者用“种子文献 + 定向搜索 + 前后向引文链”的方式扩展文献集，并记录每类方法的主导机制、报告的效率信号和引入的新开销。

主要归纳：交叉文献后，作者识别出五个反复出现的收敛性机制——(1) 选择性读取（selective reading）代替全上下文摄入；(2) 全局到局部视觉分配（global-to-local visual allocation），即在全局低分辨率浏览后局部高精度查看；(3) 可恢复记忆（recoverable memory）代替原始历史回放；(4) 验证感知控制（verification-aware control），在执行关键或不可逆动作之前增加验证；(5) 混合运行时（hybrid runtimes），根据情况在 GUI 操作与非 GUI 执行（如 API、脚本）之间切换。这五条机制构成论文的核心技术发现。

开放问题：论文最后列出若干未决挑战：诚实核算验证器成本（不能忽略验证本身的开销）、跨基准效率指标的可比性（统一测量协议）、以及真实延迟/隐私约束下对观察、记忆和动作执行三层进行联合协同设计（而不是逐层独立优化）。

实验主线：由于是综述，无新实验。证据来源包括摘要、引言、2.2 节、第 5 节动作效率以及参考文献碎片。文中可见的引文（Deng et al. 2023; Zhou et al. 2023; Rawles et al. 2024; Xie et al. 2024; Nguyen et al. 2024; Sager et al. 2025；以及 Ui-R1: Enhancing efficient action prediction of GUI agents (Lu et al. 2025)、Difficulty-aware reasoning for mobile GUI automation (Chen et al. 2025c)）显示调研对象横跨移动端和桌面端 GUI 自动化。

总体而言，这篇综述的独特性不在于提出新模型，而在于提供一套效率分析语言：它呼吁领域从单一成功率叙事转向多维度成本叙事，并要求每个效率手段都附带宽争条件地报告其副作用。对系统构建者来说，该文可以当作“GUI Agent 效率优化清单”使用，对不同子方向（如纯视觉模型、难度感知推理、验证机制）的定位也能起到定位作用。注意：摘要中没有给出具体定量数字或基准比较，因此本总结反映的是论文的定性主张与框架性贡献。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体研究方向直接相关：以 GUI Agent 为对象，覆盖观察、记忆、动作、运行时四大组件，可作为 agent 系统效率优化的通用框架参考。

## 基本信息

- 作者：Bizhe Bai, Jiakang Yuan, Hongming Wu, Xinyue Wang, Jie Ren, Siyao Chen, Yuchen Ya, Fan Bai, Pai Peng, Huafeng Qin, Tao Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-09-02
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.02309v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中证据（Abstract、Introduction、2.2 节、5 Action Efficiency、References），并结合元数据，但检索证据仅覆盖摘要与少量正文层面，部分归纳为合理推断。
