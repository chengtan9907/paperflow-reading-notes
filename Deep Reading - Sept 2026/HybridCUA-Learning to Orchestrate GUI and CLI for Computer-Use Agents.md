---
user_id: "cheng tan"
paper_id: 14366
arxiv_id: "2609.38008v1"
title: "HybridCUA: Learning to Orchestrate GUI and CLI for Computer-Use Agents"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.38008v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.38008v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-07T01:32:28"
---
# HybridCUA: Learning to Orchestrate GUI and CLI for Computer-Use Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：computer use agent · gui and cli orchestration · reinforcement learning · hybrid trajectory data

## 一句话总结

HybridCUA 提出让 computer-use agent 同时编排 GUI 点击与通用 shell 命令的混合交互范式，通过构建三类轨迹数据和 CLI-aware 奖励的两阶段训练，在 OSWorld 上达到 53.6% 准确率，较基础模型提升 14.8 个百分点，并展现出跨操作系统和跨接口的泛化能力。

## 摘要

> 论文指出现有 computer-use agents (CUAs) 的两条路线各有缺陷：纯 GUI 交互虽然通用，但动作序列长、效率低且容易发生级联错误；GUI 加应用程序专用 API/工具虽然高效，但每个应用都要单独开发，难以规模化。作者主张下一代 CUA 应将 GUI 与命令行界面 (CLI) 结合，用 GUI 的通用性覆盖视觉交互，用 shell 命令的高效可编程性完成批量操作，两者互补。关键难点在于当前模型不知道在任务执行中何时以及如何使用 CLI。为此，论文开发了一条数据构建流水线，生成三类轨迹：纯 GUI、纯 CLI、以及 GUI 与 CLI 交错执行的轨迹，并构建了包含 5K 混合轨迹和 3K 已验证 RLVR 任务的 HybridCUA-8K 数据集。训练框架分两个阶段：先在构造轨迹上进行监督微调，再用带有 CLI-aware 奖励的强化学习鼓励模型有选择且可靠地使用 CLI。实验表明，从 Qwen3.5-9B 初始化的 HybridCUA-9B 在 OSWorld 上达到 53.6% 准确率，比基础模型提高 14.8 个百分点，并在 WindowsAgentArena 上提高 4.0 个百分点，验证了 GUI–CLI 混合范式在跨平台泛化上的有效性。

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决 computer-use agent (CUA) 在交互方式上的根本性两难：
1. 纯 GUI 路线（基于 MLLM 的点击和按键）具有很强的通用性，不依赖特定应用接口，但动作序列长、执行慢，而且长序列中错误会级联放大，导致任务完成率低。
2. GUI + 应用专用 API/工具 路线通过调用应用级接口显著提速，但每接入一个应用就要单独开发和维护工具，工程成本高，难以跨应用扩展。
3. 编程智能体（coding agents）的成功提示 CLI 是一个可行的出路：CLI 随操作系统自带，能把一段很长的 GUI 操作序列压缩成一条命令，兼具通用性和效率。然而，直接把 shell 暴露给能力强的大模型并不会自动带来收益——论文图 2a 显示，一旦允许使用 CLI，四个代表性模型在 OSWorld 上的准确率反而下降 2.5 到 11.5 个百分点；图 2b 进一步显示，Qwen3.5-27B 和 EvoCUA-32B 只有 15.0% 和 0.15% 的步骤真正路由到 CLI，说明模型既不会判断使用 CLI 的时机，也不会把 shell 访问转成有效行动。
4. 因此，核心问题不是“要不要 CLI”，而是“何时用 GUI、何时用 CLI，以及如何在 GUI 上下文中可靠执行命令”。这需要将界面选择本身变成可学习的、受训练信号约束的行为，而当前方法没有做到。

Q2: 有哪些相关研究？

论文将相关研究划分为三个谱系：
1. 纯 GUI agents：基于多模态大语言模型 (MLLMs) 通过屏幕截图、点击、按键等低层动作操作图形界面。论文引用 Qin et al. (2025)、Wang et al. (2025b, OpenCUA)、Xue et al. (2026, EvoCUA)、Lu et al. (2026b) 等，这类方法通用但动作序列长、容易在长动作链中出错。
2. GUI + 应用专用 API/工具：通过调用每个应用定制的工具或 API 来加速特定任务，引用 Jia et al. (2025)、Yang et al. (2025, UltraCUA)、Hu et al. (2026, ToolCUA)、Lai et al. (2025, AutoGLM-OS-9B) 等；这类方法效率高但牺牲了通用性，每个应用都要单独构建工具。
3. CLI/编程智能体：OpenClaw Contributors (2026)、Claude Code Team (2026)、OpenAI (2026) 等编程智能体证明了命令行界面可以把长 GUI 操作折叠成单条命令；论文据此论证 CLI 与操作系统绑定，具有天然通用性。
4. 新近的 GUI–CLI 混合方向：论文提到已有工作开始探索 GUI–CLI 工作流，包括基准、环境和 agent 系统（Li et al., 2026; Yang et al., 2026; Zhou et al., 2026），并指出最接近的两项工作主要在“如何将 GUI 与 CLI 结合”的具体机制上与本文不同。由于预印本正文在检索片段之外未提供更细致的对比细节，我们无法进一步复述其差异，这部分需要查阅原文。

Q3: 论文如何解决这个问题？

论文提出的解决方案包含两条主线，分别面向数据与训练：
1. 数据构建流水线（Data Construction Pipeline）：
 - 生成三类轨迹：纯 GUI（仅点击/按键）、纯 CLI（仅 shell 命令）、以及 GUI 与 CLI 交错执行的混合轨迹。
 - 该流水线在 GUI 上下文中锚定界面选择和命令执行——检索证据显示其构造了约 5,000 条轨迹，使模型能学习“什么操作适合用 CLI 表达、什么操作必须在 GUI 内完成”（例如 VLC 中与播放器自身状态相关的操作必须留在 GUI，而快照、批量处理可用 cvlc 命令行）。
 - 产物为 HybridCUA-8K 数据集：包含 5K 混合轨迹和 3K 经过验证的 RLVR（reinforcement learning with verifiable rewards）任务，后者为在线强化学习提供可自动验证的奖励信号。
2. 两阶段训练框架：
 - 第一阶段：在上述三类轨迹上做监督微调（SFT），论文报告为 2 个 epoch，作为 warm-up 让模型初步习得混合动作空间。
 - 第二阶段：使用 GRPO 做在线 agentic 强化学习，训练数据来自 3K RLVR 任务。关键设计是 CLI-aware 奖励：在轨迹层级引入 λCLI = 0.1，在步骤层级引入 λexec = 0.3，前者约束 agent 对 CLI 的选择性使用（不滥用也不回避），后者约束命令行执行的实际可靠性（命令要真正执行成功）。
 - 论文明确把界面选择变成显式的训练信号，而非仅依赖模型自发涌现。
3. 实现与基础模型：从 Qwen3.5-9B 初始化，使用 verl + Megatron-LM 做 SFT，用 slime + Megatron-LM + SGLang 做 RL 与 rollout。动作空间、具体交互格式、完整训练配置在附录 B/C/D 中给出，正文检索片段未包含这些细节。

Q4: 论文做了哪些实验？

论文的实验设计覆盖基准、基线和消融三个维度：
1. 主基准：OSWorld，采用官方评估协议，报告任务准确率（Acc.）和平均步数（Avg. Steps），每任务上限 50 步。
2. 基线分组：
 - 纯 GUI agents：Qwen3.5、OpenCUA (Wang et al., 2025b)、EvoCUA (Xue et al., 2026)。
 - GUI–API agents：ToolCUA (Hu et al., 2026)、AutoGLM-OS-9B (Lai et al., 2025)、UltraCUA (Yang et al., 2025)。
 - 同一基础模型 Qwen3.5-9B 的两种动作空间变体：仅 GUI vs. GUI–CLI，用于隔离混合接口带来的增益。
3. 跨分布泛化测试：
 - OSWorld-MCP：衡量向 MCP 使能环境的迁移能力；
 - WindowsAgentArena (Bonatti et al., 2024)：衡量跨操作系统的泛化能力。
4. 消融与归因：论文对 SFT 与 RL 进行对比，以分离两阶段各自的贡献；并孤立 CLI-aware 奖励中的各信号分量。检索证据显示：RL 相对 SFT 每任务平均节省 5.8 步，即 29.3% 的轨迹长度缩减，说明 RL 在提升完成率的同时还能进一步缩短动作序列。
5. 行为分析：通过图 5、图 6、图 7 分别展示领域级准确率与 GUI/CLI 步数占比、操作需求层面的接口偏好，以及 essay-formatting 任务中的 GUI–CLI 协作案例。需要注意的是，OSWorld-MCP 上的具体数值、消融中各分量的具体收益、以及 WindowsAgentArena 的准确率数值在摘要和检索片段中没有完整给出，需要查阅论文正文与表格。

Q5: 发现了什么实验现象？

实验揭示的主要现象与反直觉结果如下：
1. 直接暴露 CLI 反而掉点：将 shell 开放给四个代表性 MLLM agent 后，它们在 OSWorld 上的准确率普遍下降 2.5–11.5 个百分点（图 2a）。这反直觉地说明“能力强的大模型 + 命令行”不等于更好的 computer-use agent，模型需要经过专门的训练才能把 CLI 变成有效工具。
2. 界面选择严重失衡：Qwen3.5-27B 只有 15.0% 的步骤经过 CLI，EvoCUA-32B 更是只有 0.15%，几乎完全依赖低效的 GUI 长序列，进一步印证现有模型不知道“何时用 CLI”。
3. 混合训练带来显著增益：HybridCUA-9B 在 OSWorld 上达到 53.6% 准确率，较基础 Qwen3.5-9B 提高 14.8 个百分点；在 WindowsAgentArena 上提高 4.0 个百分点。
4. RL 相比 SFT 同时优化了效率：RL 阶段在 SFT 基础上每任务平均再节省 5.8 步（轨迹长度减少 29.3%），说明强化学习对“少用 GUI、巧用 CLI”的奖励信号是有效的，而不是单纯堆训练数据。
5. 领域与操作层面的接口偏好：图 5 显示 OS 类任务中 CLI 占 84%（shell 直接访问文件与系统设置），而 Chrome 类任务中 GUI 占 74%（交互集中在网页内容上）；图 6 进一步显示内容编辑有 85.8% 的步骤走 CLI、结果验证有 98.4% 走 CLI，而空间调整类操作 58.4% 使用 GUI——模型在“直接数据操作/状态检查”与“需要视觉反馈的定位布局”之间形成了有意义的区分。
6. 跨 OS 泛化不是简单记忆命令：结果显示模型在 WindowsAgentArena 中会发出 PowerShell 命令，尽管训练时只用过 Linux shell。这提示学到的核心能力是“什么时候该委派给 shell”这种元决策，而不是死记具体命令；命令语法本身似乎也具备一定迁移性。
7. GUI–CLI 可在同一任务内协作：论文以 essay-formatting 为例展示了 GUI 负责聚焦 Writer 窗口、打开菜单（步骤 7），CLI 用 python-docx 直接修改文档格式（步骤 13 等），两种接口在同一轨迹中交替推进任务，证明二者不是互斥而是互补的。

Q6: 有什么可以进一步探索的点？

基于论文现有结果与信息缺口，可以进一步探索的方向包括：
1. 跨操作系统命令的显式扩展：既然模型能在只见过 Linux shell 的情况下泛化出 PowerShell 使用行为，未来可针对 Windows（PowerShell/CMD）、macOS（zsh）等构建更丰富的跨平台 CLI 数据，量化异构 shell 之间的迁移边界。
2. 更细粒度的界面切换策略：当前界面选择发生在步骤或轨迹层级，可探索在单个动作内部进行 GUI 与 CLI 的混合编排，或根据实时环境反馈（如命令报错、GUI 控件加载延迟）动态切换接口。
3. 奖励设计的扩展：论文使用 λCLI 和 λexec 两个系数，未来可加入对非法命令、危险操作、隐私敏感操作的安全惩罚，或对“该用 GUI 却用了 CLI”的负奖励，以更精细地刻画界面选择的正确性。
4. 错误恢复与失败模式：CLI 命令执行失败时 agent 如何回退到 GUI、如何根据 stderr 修正命令，这类鲁棒性机制在论文中未见专门分析，值得作为独立研究问题。
5. 数据构建流水线的可扩展性与偏差：论文依赖强模型通过执行器采样 CLI-only 和混合轨迹，这类数据可能存在模型偏好、轨迹分布不均、长尾应用覆盖不足的问题；未来可研究如何用规则、人工反馈或对抗性过滤提高轨迹质量。
6. 更广泛的基准与真实环境覆盖：OSWorld-MCP 的具体收益尚未在摘要中给出，WindowsAgentArena 的跨 OS 结果也只有相对提升；后续应报告完整的 OOD 数值，并考虑把 GUI–CLI 混合范式推广到更复杂的软件（IDE、设计工具、科学计算软件）和更长程任务。
7. 与 MCP/工具生态的结合：论文把混合接口与 MCP 环境做了迁移测试，但 GUI–CLI 与 MCP 工具的关系（何时用工具、何时用 CLI、何时用 GUI）仍未完全理清，存在三路编排的机会。
8. 数据规模与模型规模的 scaling 行为：HybridCUA-8K 的规模并不大，训练更大的模型或扩充数据后，混合策略是否会呈现类似 scaling law 的改善，是一个自然延伸问题。

Q7: 总结一下论文的主要内容

HybridCUA 是一篇关于 computer-use agent（CUA）交互范式的预印本工作，核心主张是：下一代 CUA 不应只在 GUI 内点击按键，也不应依赖每应用定制的 API，而应把 GUI 的通用视觉交互与 CLI 的可编程效率结合起来，让模型学会在两者之间动态编排。

论文从现有 CUA 的两条技术路线切入：纯 GUI 方法（如 OpenCUA、EvoCUA 等）靠 MLLM 把屏幕截图映射为低层点击/按键动作，优点是无需针对应用定制，缺点是长序列执行慢、错误级联；GUI+API 方法（如 ToolCUA、AutoGLM-OS-9B、UltraCUA 等）通过调用应用级工具大幅提速，但每个应用都要单独造工具，无法规模化。编程智能体（如 Claude Code、OpenClaw）的成功让作者看到 CLI 作为第三条路的价值——CLI 随操作系统自带，可以把长 GUI 流程压缩为单条命令，且不牺牲通用性。但论文的实证观察表明，单纯给 MLLM 暴露 shell 反而有害：四个代表模型在 OSWorld 上掉 2.5–11.5 个百分点，且它们很少真正使用 CLI（最低仅 0.15% 的步骤）。这说明瓶颈不在“有没有 CLI”，而在“模型不知道何时、如何使用 CLI”。

针对这一瓶颈，论文提出完整的解决方案，包括数据与训练两部分。在数据侧，作者设计了一条数据构建流水线，产出三类轨迹：纯 GUI、纯 CLI、以及 GUI 与 CLI 交错的混合轨迹；这些轨迹把界面选择与命令执行锚定在具体的 GUI 上下文中（例如 VLC 播放器自身的状态操作必须留在 GUI，批量快照可以用 cvlc 命令行），共计约 5,000 条混合轨迹，并另有 3,000 个可验证奖励的 RLVR 任务，合称 HybridCUA-8K。在训练侧，论文采用两阶段框架：先在全部三类轨迹上做监督微调（2 epochs）作为 warm-up，再用 GRPO 在线强化学习在 RLVR 任务上训练。关键的 CLI-aware 奖励包含轨迹层系数 λCLI=0.1 与步骤层系数 λexec=0.3，分别惩罚/奖励“是否选择性使用 CLI”和“命令是否真实可执行”，从而把界面选择变成显式训练信号。基础模型为 Qwen3.5-9B，SFT 用 verl + Megatron-LM，RL 用 slime + Megatron-LM + SGLang。

实验以 OSWorld 为主基准，基线横跨纯 GUI agents、GUI–API agents、以及相同基础模型上的 GUI-only vs GUI–CLI 动作空间变体；报告任务准确率和平均步数（上限 50 步）。主要结果显示 HybridCUA-9B 在 OSWorld 上达到 53.6%，比基础模型高出 14.8 个百分点；在 WindowsAgentArena 上提升 4.0 个百分点，且模型在未见过的 Windows 环境中能自发使用 PowerShell，表明其学到的是“何时委派给 shell”的元决策而非死记命令。论文还通过消融证明 RL 在 SFT 基础上每任务平均省 5.8 步（29.3% 的轨迹长度缩减），说明强化学习是提升效率的关键。行为分析显示，模型在不同领域和操作类型上形成了合理的接口偏好：OS 任务 84% 的步骤走 CLI、Chrome 任务 74% 走 GUI，内容编辑和结果验证分别有 85.8% 和 98.4% 的 CLI 占比，而空间调整类操作以 GUI 为主（58.4%）；在 essay-formatting 任务中，GUI 负责窗口聚焦与菜单操作，CLI 用 python-docx 直接改文档格式，两种接口在同一轨迹内互补协作。

总体而言，论文的贡献在于：把 GUI–CLI 混合范式形式化为一套“数据构建 + 两阶段训练”的完整框架，用可验证的 RLVR 任务和 CLI-aware 奖励把界面选择变成可学习、可优化的行为，并在主基准与跨 OS 泛化上给出积极证据。局限方面，论文作为预印本尚未提供完整的限制讨论（检索片段只留下关于人工监督的只言片语）；OSWorld-MCP 上的具体收益、CLI-aware 各奖励分量的消融数值、窗口级混合动作的细节等仍需查阅正文与附录确认。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的智能体方向（权重 0.10）直接相关：论文研究的是 computer-use agent 的交互范式与强化学习训练，属于 agent 行为学习的前沿问题。

## 基本信息

- 作者：Tongbo Chen, Junbo Niu, Zhengxi Lu, Niu Lian, Fei Tang, Yuchen Yan, Yike Hong, Yong Du, Yizhou Liu, Bofan Chen, Yongliang Shen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-09-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.38008v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（hash:hash:768，93 个 chunk），并结合作者提供的摘要、Introduction、Results、Discussion、Conclusion 片段进行改写；涉及数据构建细节、消融数值等未被正文片段覆盖的部分已标注为需要回原文确认。
