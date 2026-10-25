---
user_id: "cheng tan"
paper_id: 16489
arxiv_id: "2610.26071v1"
title: "CUA-Lite: An Open Platform for Computer Use Agents"
publish_date: "2026-10-19"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.26071v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.26071v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-20T13:06:36"
---
# CUA-Lite: An Open Platform for Computer Use Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：computer use agents · unified environment interface · benchmark integration · model harness

## 一句话总结

CUA-Lite 是一个开放的计算机关机智能体（CUA）开发平台，通过统一环境接口、统一监督数据格式和共享模型 harness 三个标准化抽象，集成了 12 个桌面/浏览器/移动基准、3 万余个可验证任务和约 82K 条 reward 验证过的 rollout，使评估、RL 与 SFT 共享同一基础设施，并发布可复现的参考分数与开源资源。

## 摘要

> Open resources for computer use agents (CUAs) -- environments, datasets, and models -- remain scattered: without a standard, these resources cannot be integrated and scaled; additionally, evaluation, RL, and SFT cannot share one infrastructure. We present CUA-Lite, an open platform for developing CUAs, built around three standardized abstractions: (1) Unified environment interface, under which we integrate 12 benchmarks across desktop, browser, and mobile, plus lightweight VM-free sandboxes that replicate OSWorld's desktop at much lower cost, providing over 30,000 verifiable tasks. (2) Unified supervised data format, into which we convert the major public CUA datasets, from GUI grounding and understanding to full rollouts, and record our fresh rollouts from frontier CUAs on these verifiable environments. (3) Model harnesses shared across evaluation, RL, and SFT, one per model and implemented for 14 model families. Each harness runs its model in these computer environments, producing the rollouts that evaluation and RL consume, or renders stored supervised data into the model's own training format for SFT. We release reference scores across the integrated benchmarks, and show that RL and SFT through the same harness improve open-weight models. We release our dataset, code, and models, and the platform is open to contributions.

Q1: 这篇论文试图解决什么问题？

计算机使用智能体（CUA）的目标是在桌面、浏览器和移动应用的图形界面中自主执行任务。开发一个可用的 CUA 至少需要三样东西：带可验证任务的环境、监督数据、以及将模型接入环境的执行框架（harness）。这三类资源在过去几年已经大量涌出：环境方面有面向桌面、浏览器和移动端的多个 benchmark；数据方面有从 GUI grounding 标签到完整 episode 轨迹的各种数据集；模型方面有专门的 CUA 模型、通用视觉语言模型（VLM）以及自带原生计算机使用工具的 API 模型。然而，这些资源彼此隔离，没有统一标准。每个环境定义自己的动作空间、观测表示和运行方式（例如虚拟机、真实浏览器或移动模拟器）；每个数据集使用各自的字段和标注协议；每个模型拥有自己的 prompt 模板、动作 token 化方式和 rollout 循环。这种碎片化带来三重后果：其一，将一个模型接入一个新环境，开发者必须重新实现动作映射、上下文管理和 rollout loop，形成大量重复劳动，并且难以横向对比不同模型；其二，评估、强化学习和监督微调无法共享同一套基础设施——评估需要环境接口，RL 需要环境与 rollout 代码，SFT 需要数据渲染，三者各自维护一套代码基线，导致训练与评估之间的闭环难以建立；其三，已发表的 benchmark 分数常常没有可运行的评估脚本，无法被复现或作为后续工作的可靠基线。论文要解决的核心问题不是提出某一个更强的 CUA 模型，而是为整个领域提供一个开放、标准化的基础设施，让环境、数据、模型和训练流程能够组合、扩展和复用，从而降低开发门槛并让社区贡献能够积累。进一步看，这个问题还涉及范式选择：与自然语言 agent 不同，GUI agent 必须处理像素观测、动作坐标化和环境反馈，其接口设计直接影响 RL 奖励信号、SFT 数据质量和评估的可信度。CUA-Lite 的目标正是把这套接口统一起来，使同一模型 harness 可以同时服务于三种训练范式，使新 benchmark 接入后能立即被所有模型使用，也使新模型接入后能立即在全部 benchmark 上评估。这是论文试图解决的根本矛盾：个体资源丰富但系统性基础设施缺失。

Q2: 有哪些相关研究？

相关工作按资源类型分布：(1) 环境与 benchmark。桌面端已有多个工作围绕操作系统级 GUI 操作，例如 OSWorld 等提供可验证任务；浏览器端有以网页任务为主的 benchmark 系列；移动端有围绕手机 UI 操作的工作。论文简介部分列出了这些方向的大量引用，但它们各自定义独立的接口和 runtime（虚拟机、浏览器、模拟器），格式互不兼容。(2) 监督数据集。从 GUI grounding 与界面理解（如点选、标注）到完整 rollout 轨迹，公开数据集格式差异极大，缺乏统一 schema，导致数据不能直接用于不同模型的 SFT。(3) 模型与 harness。已有专门 CUA（如 UI-TARS、EvoCUA、MAI-UI、UI-Venus-2 等）和通用 VLM（如 Qwen3-VL 系列），以及 API 模型自带的原生工具。每个模型有自己的 action space、prompt 格式与 rollout 代码，没有一个通用 harness。(4) 评估与可复现性。很多已发表的分数没有附带可运行评估脚本，论文明确表示不声称复现这些结果，而只提供可由 CUA-Lite 配置重跑的参考分数。(5) 平台类工作。CUA-Lite 的定位与既有开放平台不同：它强调三位一体的标准化（环境、数据、harness），而不是单一组件。该方向目前缺乏系统性开源基础设施，本文是对这一缺口的直接回应。(6) RL/SFT 训练。在统一 harness 上，论文演示了 RL（GRPO 类）和基于教师 rollouts 的 SFT 都能改进开放权重模型，并指出“直观的训练任务并不总适合 RL”，这与 reward hacking、任务难度选择等 RL 研究相关。值得注意的边界：本文并非提出新的 agent 能力，而是基础设施。它能与各种现有模型配合，包括专门的 CUA、VLM 和 API 模型，这使它成为对比研究和训练协议研究的中立平台。与仅提供环境的项目不同，CUA-Lite 把数据格式和 harness 也纳入标准化范围，这类似于在 LLM 领域把数据标准、训练框架和评估框架一体化，但在 GUI agent 领域尚属少见。

Q3: 论文如何解决这个问题？

CUA-Lite 的核心方案是定义三个标准化抽象，并在其上构建完整工具链。(1) 统一环境接口：所有环境下采用同一动作空间和观测表示。GUI 动作返回截图以及环境发出的文本；额外工具（如 shell）返回自己的结果，例如命令输出。该接口被 12 个 benchmark 和训练环境共同实现，覆盖 grounding、桌面、浏览器、移动四类任务。为了降低环境成本，论文从 OSWorld 的官方虚拟机镜像逆向工程出应用清单和配置，用 Dockerfile 重建出 Lite.OSWorld 轻量沙箱，不依赖 VM，成本大幅降低，同时保留 OSWorld 的可验证任务。这类“无 VM”沙箱对大规模 RL 尤其重要，因为 RL 需要大量 rollout，虚拟机成本会形成瓶颈。(2) 统一监督数据格式：论文把主要公开 CUA 数据集——从 GUI grounding/界面理解到完整轨迹——转换为一个统一 schema。该 schema 覆盖模型推理通道（reasoning_content）、每一步的自然语言动作描述（action_description）和实际工具调用（tool_call）。在新格式下，研究者可以用统一方式做 SFT 数据混合。论文还记录了 GPT-5.5、Qwen3.5-27B、Qwen3.8-27B 等模型在训练环境上的新 rollouts，并保留 reward 已验证的样本，共约 82K 条 Lite.Samples，每个 step 都保存推理内容、动作描述和 tool_call。(3) 模型 harness：每个模型家族实现一个 harness，共 14 个模型家族。harness 负责在统一环境接口上运行模型生成 rollout（供评估与 RL 消费），或将存储的监督数据渲染为模型自身训练格式（供 SFT 使用）。由于 eval、RL、SFT 共用同一 harness，协议之间天然一致，模型进步可以直接转化为可复现分数。论文强调参考分数可重跑：参考分数表给出各模型在支持 benchmark 上的分数，所有配置随代码发布。对 API 模型，论文按其官方默认行为配置；对开放权重模型默认关闭 reasoning，并单独做 reasoning 开关的消融。为了公平，论文只实现模型自身的 GUI loop，不引入更丰富的外围 harness（如 Codex/Claude Code），以避免混淆模型能力与工具集成能力。

Q4: 论文做了哪些实验？

论文的实验部分由四块组成。(1) 参考分数评测：在 12 个集成 benchmark 上运行 15 个模型——专门 CUA（UI-TARS-1.5、EvoCUA、MAI-UI、UI-Venus-2）、通用 VLM（Qwen3-VL 4B–32B、Qwen3.5 4B–27B、Qwen3.8-27B）和 API 模型（Claude Opus 5、GPT-5.5、GPT-5.6 Sol、GPT-6 Astra）。默认设置是开放权重模型关闭 reasoning、API 模型 medium reasoning，UI-Venus-2 按其官方默认在非 grounding 任务开启 reasoning。分数发布为参考分数，不声称复现先前结果。(2) RL 训练实验：在训练环境上收集 GPT-5.5、Qwen3.5-27B 和 Qwen3.8-27B 的 rollouts，用可验证 reward 筛选后得到约 82K 条 Lite.Samples。随后对开放权重模型做 RL（提到 GRPO 尝试），并比较训练前后在桌面、浏览器、移动 benchmark 上的表现。检索证据显示有 Figure 7 描述 RL 前后对比，并讨论了“选择训练任务”的问题。(3) SFT 蒸馏实验：用教师模型（如 GPT-5.5 等）的 rollouts 对 Qwen3.5-27B 等做 SFT，变化条件包括教师是否开启 reasoning、每个任务使用 1 张或 4 张截图等，评估对比 No SFT 与 SFT 的分数。(4) 推理消融：Figure 5 展示 reasoning 开关对分数和 rollout 长度的影响——Qwen 模型对比 reasoning on/off，GPT 模型对比 medium/xhigh reasoning effort 与 none，每个平台平均基准结果。所有任务的设置细节（如附录 Table F.1）随代码发布。

Q5: 发现了什么实验现象？

根据检索到的 Results 片段，主要实验现象包括：(1) RL/SFT 带来可观的分数提升。在某平台（片段起始被截断，可能涉及桌面）上成功率从 60.0 提升到 72.8（浏览器），移动端 shaped reward 从 34.6 提升到 46.7。这些数字出现在 Figure 7 讨论前后，推测为 RL 训练后的效果，但具体模型和训练配置在检索证据中不完整，需要回原文确认。(2) 训练任务的选择是关键。论文明确指出“The obvious training tasks do not always suit RL”——直观上应该训练的任务未必适合 RL。在桌面 benchmark 上，正式 run（Figure 7）之前尝试 GRPO 出现过失败（具体失败模式未知，推测与 reward 稀疏或环境不可验证有关）。这提示任务难度、奖励函数设计对 RL 效果影响很大。(3) 推理对性能的影响因平台而异。Figure 5 表明对 Qwen 模型，开启 reasoning 在不同平台上的分数变化有正有负；GPT 模型使用更高 reasoning effort 的效果也非一致正向。同时 rollout 长度随推理开启显著增加（GPT 可达 2×–4×，Qwen 约在 0.5×–2× 区间），说明推理带来计算成本与收益之间的权衡。(4) SFT 蒸馏实验显示教师 rollouts 的质量设置（是否开推理、截图数量）会影响学生模型分数。检索证据里有“1 screenshot vs 4 screenshots”的对比，但具体趋势未给出；合理推断是更多截图不一定更好，因为可能引入冗余信息。(5) 可复现性问题。论文承认多数先前分数没有可运行脚本，只发布自己配置下的参考分数，这本身反映了领域现状：分数不可复现是普遍问题。(6) 负结果与边界：harness 只实现 GUI loop，不实现 Codex/Claude Code 等更丰富的工具调用框架，因此某些模型在需要额外工具的场景下可能被低估。

Q6: 有什么可以进一步探索的点？

基于论文所述平台，可进一步探索的方向包括：(1) 扩展 benchmark 覆盖：接入更多桌面应用、浏览器场景和新移动 OS 版本，验证接口抽象能否覆盖长尾 GUI 交互。(2) 改进轻量沙箱：Lite.OSWorld 基于逆向工程重建，后续可探索自动生成 VM-free 沙箱的通用工具链，进一步降低环境成本，并支持并发大规模 rollout。(3) 统一数据格式的深化：将更多非 GUI 数据（如网页 HTML、无障碍树）纳入统一 schema，探索跨模态 grounding 数据混合。(4) RL 研究：系统研究训练任务选择、reward 设计、GRPO 失败案例，形成“训练环境选择指南”；可研究如何从 82K Lite.Samples 中自动挖掘适合 RL 的任务。(5) 推理与成本的帕累托分析：基于 Figure 5 的 reasoning 消融，量化不同任务难度下推理 effort 的最优设置，甚至学习一个动态 reasoning 控制器。(6) SFT 蒸馏协议：研究教师模型推理开关、截图数量、teacher 多样性对蒸馏效果的普遍规律，建立更鲁棒的蒸馏策略。(7) 跨模型迁移：利用统一 harness 研究不同动作空间（坐标点击 vs 语义操作）之间的迁移，或在统一平台比较原生工具调用与纯视觉 grounding。(8) 安全与对齐：可验证任务环境可用于安全评估和红队测试，探索 CUA 在真实环境中执行高风险操作时的约束机制。(9) 社区贡献机制：平台开放贡献，需要设计接口扩展的评审标准，避免标准化变成新的碎片化。

Q7: 总结一下论文的主要内容

CUA-Lite 针对计算机使用智能体领域资源散乱、无法集成和扩展、评估/RL/SFT 无法共享基础设施的问题，提出一个开放平台。平台以三个标准化抽象为支柱：统一环境接口、统一监督数据格式、共享模型 harness。统一环境接口将 12 个覆盖桌面、浏览器和移动端的 benchmark 以及训练环境纳入同一套动作空间和观测表示；GUI 动作返回截图加文本，额外工具返回自身结果。为降低桌面环境成本，论文从 OSWorld 官方镜像逆向工程出应用清单与配置，用 Dockerfile 重建出无 VM 的 Lite.OSWorld 沙箱，使大规模 RL 不再受虚拟机成本制约。统一监督数据格式将公开数据集（从 GUI grounding 到完整轨迹）转换成一个包含 reasoning_content、action_description 和 tool_call 的 schema，并新增约 82K 条来自 GPT-5.5、Qwen3.5-27B、Qwen3.8-27B 等模型的、reward 验证过的 rollout 样本。模型 harness 为 14 个模型家族各实现一个，可生成 rollout 供评估和 RL 使用，或将监督数据渲染成模型训练格式供 SFT 使用，从而打通了三种训练/评估范式。实验部分运行了 15 个模型（含专门 CUA、通用 VLM 和 API 模型）在 12 个 benchmark 上的参考分数，明确声明这些分数可重跑但不声称复现先前结果。论文进一步演示了 RL 和 SFT 通过同一 harness 改进开放权重模型：RL 后浏览器成功率从 60.0 升到 72.8，移动端 shaped reward 从 34.6 升到 46.7；同时记录了训练任务选择失败（GRPO 在某些桌面任务上的负结果）、推理开关对性能与 rollout 长度的非单调影响、以及教师 rollout 设置（推理开关、截图数量）对 SFT 蒸馏的影响。论文还指出 harness 实现边界——只实现 GUI loop 而不含 Codex/Claude Code 等增强工具，避免将工具集成能力与模型能力混淆。整体上，CUA-Lite 的贡献是提供了一套系统性、可复现、可扩展的 CUA 开发基础设施，并开放数据集、代码与模型，邀请社区贡献。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接命中用户画像中的 agent 方向（权重 0.10），且属于系统性平台工作，符合用户偏好“系统性工作优于增量工作”。

## 基本信息

- 作者：Zhanhui Zhou, Weichen Zhang, Haoran Liu, Lingjie Chen, Tianneng Shi, Kevin Lin, Zhengyuan Yang, Lijuan Wang, Dawn Song
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-10-19
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.26071v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，主要来自 Introduction 和 Results 片段；由于论文正文 method/discussion/conclusion 缺失，部分内容基于摘要与片段合理推断。
