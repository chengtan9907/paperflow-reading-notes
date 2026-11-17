---
user_id: "cheng tan"
paper_id: 21856
arxiv_id: "2611.17955"
title: "WebArena-Pro: A Heterogeneous, Multimodal, Reproducible Benchmark for Web Agents"
institution: "McGill University / Mila – Quebec AI Institute（依据通讯作者邮箱 xing.han.lu@mail.mcgill.ca 与 siva.reddy@mila.quebec 推断；完整作者单位未在给定片段中列出，合作者单位需查原文）"
publish_date: "2026-11-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.17955.pdf"
pdf_url: "https://arxiv.org/pdf/2611.17955"
abs_url: "https://arxiv.org/abs/2611.17955"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-16T23:30:19"
---
# WebArena-Pro: A Heterogeneous, Multimodal, Reproducible Benchmark for Web Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：web agents · benchmark · multimodal agents · reproducible evaluation

## 一句话总结

WebArena-Pro 提出一个覆盖 20 个自托管 Web 应用、300 个任务、把音频和视频与文本、视觉并列为观察模态的可复现 Web agent 基准，并用 rubric DAG 与容器隔离评测揭示当前最强 agent 仅达 62.3% 成功率。

## 摘要

> Web agents powered by large language and vision-language models are increasingly applied to realistic browser work that spans heterogeneous applications, multimodal content, and stateful workflows. However, existing reproducible web-agent benchmarks cover only a small number of web applications drawn from a few software categories, and restrict modality to text and vision. Live benchmarks broaden site coverage but sacrifice reproducibility, since pages and data drift between runs. Moreover, existing benchmarks do not meaningfully evaluate whether agents can understand and use audio and video content embedded within web tasks. To address these gaps, we introduce WebArena-Pro, a benchmark comprising 300 tasks across 20 self-hosted web applications in six domain categories, spanning distinct interface conventions, workflows, and data models. Across the evaluated agents, the best performance is achieved by both Codex with GPT 6.1 Sol and Claude Code with Claude Opus 5.5 at 62.3%, followed by OpenCode with Kimi 3.1 Pro at 60.7%. The strongest GenericAgent configurations, Gemini 3.1 Pro omni and Gemma4 31B, both achieve 30.3%. Among reproducible, human-curated web agent benchmarks, WebArena-Pro provides the broadest application coverage and the most comprehensive multimodal support to date. The benchmark treats audio and video as core observations alongside text and vision, with dedicated actions for extracting information from each. WebArena-Pro runs each task in isolation and supports reproducible, parallel evaluation. Tasks are authored through a dedicated annotator interface, filtered by LLM-assisted triage, and finally validated by humans before release. Project page: https://webarena-pro.github.io

Q1: 这篇论文试图解决什么问题？

一、核心问题：Web agent 的能力测量落后于其应用面。论文要解决的不是某个模型能力不足，而是评测供给不足：真实浏览器工作已经横跨通信、生产力、旅行、设计媒体、业务运营、分析等多个软件类别，涉及不同界面惯例、数据模型和有状态工作流，但可复现基准往往只覆盖少数应用，导致结论外推困难。

二、覆盖缺口：现有自托管基准多从少数软件类别抽取应用，任务类型和交互模式相对单一；agent 在 Gmail 或电商站点上的高分，不能说明它能处理 CRM、BI 面板、设计工具、身份管理或媒体库。WebArena-Pro 用 20 个应用、6 个领域类别和跨站点任务来扩大覆盖，试图把“异构性”变成可测量的变量。

三、可复现性与真实性的张力：真实网站基准能覆盖长尾站点，但页面、数据、A/B 实验和登录状态会在运行间漂移，难以做严格对照、消融和复现。论文选择自托管应用与容器隔离，以环境可控性换取可复现性；代价是自托管克隆与真实互联网之间存在 domain gap，这一点是合理推断，也是此类基准的共性边界。

四、多模态缺口：现有基准主要把网页视为文本（accessibility tree）加视觉（截图），很少要求 agent 真正理解网页中的音频和视频。论文把 audio/video 提升为与文本、视觉并列的核心观察，并提供 transcribe、extract_frames、watch、listen 等动作，试图测量“媒体内容依赖”的任务能力。

五、任务有效性与难度校准：基准不仅要“有任务”，还要避免任务太简单、太模糊或不可复现。论文用标注者接口写题、LLM 辅助 triage 剔除一次就成功的题、第二标注者复核可行性并定 rubric、发布前人工验证，这是在控制任务难度和可评分性。

六、评测可复核性与归因：开放网页任务的最终状态难以用单一字符串匹配，论文用 action/outcome rubric 组成 DAG，并以确定性 grader、启发式 grader、LLM judge 的顺序仲裁。这解决了“正确但路径不同”的部分问题，但也引入 rubric 设计主观性、LLM judge 可靠性和全节点通过过于严格等新问题。

七、隐含假设：论文假设自托管应用矩阵能代表真实 Web 工作的主要异构性；假设预录音频和离散视频帧足以测量媒体理解；假设 rubric 分解能覆盖任务成功的主要条件。这些假设合理但未被完全证明，尤其是跨站点状态、真实网站的长尾 UI 和连续时间媒体交互。

Q2: 有哪些相关研究？

一、自托管、可复现的 Web agent 基准。领域内较早的 MiniWoB++、WebShop 偏向合成或单一任务；WebArena 把多个自托管站点组织成可复现环境；VisualWebArena 强调视觉 grounding；WorkArena/BrowserGym 关注企业软件与统一交互接口；Mind2Web 侧重离线真实网站轨迹。它们的共同优点是环境可控、可重复，缺点是应用数量和软件类别有限，模态通常停留在文本与视觉。WebArena-Pro 的定位是把应用覆盖扩到 20 个、6 个类别，并加入音频/视频。

二、真实网站或活环境基准。WebVoyager 一类工作直接在真实网站上跑任务，覆盖长尾站点和真实 UI，但页面和数据会漂移，运行间难以严格比较，也不适合做大规模消融。论文用自托管加容器隔离来换回可复现性，这是与 live benchmark 的核心权衡。

三、多模态 GUI/Web agent。GUI agent 研究通常使用截图、accessibility tree、OCR、坐标点击和工具调用；多模态 benchmark 也逐步加入视频、图表和文档。但把音频和视频作为网页任务的“核心观察”而非附属素材，仍然少见。WebArena-Pro 的 transcribe、extract_frames、watch、listen 动作，实际上把媒体理解变成了 agent 工具链的一部分。

四、评测与验证器。端到端成功率是常见指标；开放任务常用 LLM-as-judge、rubric、后端状态检查或人工评估。论文的 action/outcome rubric DAG 和 deterministic→heuristic→LLM judge 的仲裁顺序，属于“可复核优先、开放结局兜底”的设计。它与纯 LLM judge 的差别在于：能用后端状态或精确字符串判定的 rubric 不交给模型。

五、企业工作流与跨应用任务。CRM、BI、工单、招聘、LMS、身份管理等应用具有不同数据模型和权限边界；跨站点任务还要求状态在应用间传递。WebArena-Pro 把这些纳入同一基准，试图测量长程、有状态、异构工具协调能力。

六、差异与缺口。相较相邻工作，WebArena-Pro 的差异化是“广应用覆盖 + 音频/视频一等模态 + 容器隔离并行 + 人工验证 rubric”。但给定片段没有展开逐项 baseline 对比，相关工作的具体引用和每个基准的覆盖数字需要回原文 Table 1 核对；上述部分名称为领域背景补充，不应视为论文原文的完整引用列表。

Q3: 论文如何解决这个问题？

一、应用矩阵与任务空间。WebArena-Pro 由 300 个任务、20 个自托管 Web 应用、6 个领域类别组成。从引言表格可辨识的应用包括 Slack、Gmail、X/Twitter、Dropbox、Google Drive、Notion、1Password、Google Flight、Airbnb、Spotify、Figma、Google Photos、YouTube、PowerBI、Pipedrive、Mint、Handshake、LMS、Zendesk、Salesforce；类别覆盖通信、生产力、旅行、设计与媒体、业务运营、分析。不同应用有不同界面惯例、工作流和数据模型，任务既有单站点也有跨站点、有状态的操作。

二、任务构造流水线。任务通过专用标注者接口编写；候选任务先经过 LLM 辅助 triage：用一个 LLM-based agent 试跑，把第一次就正确完成的任务标记为 trivial 并丢弃，以保证难度。通过 triage 的任务由第二标注者在真实应用上复核，确认任务可执行、最终确定评分 rubric，并确认任务描述与预期结果一致；发布前再做人工验证。这个流水线同时控制任务有效性、难度和可评分性。

三、多模态观察与动作空间。基准把音频和视频作为核心观察，而不仅是文本和截图。动作空间提供 transcribe() 把音频/视频转成文本、extract_frames() 以固定帧率抽帧、watch() 与 listen() 访问媒体内容；网页观察仍以 accessibility tree 为主，配合视觉信息。这样 agent 必须决定何时读页面、何时看视频、何时听音频、何时转写，媒体理解成为任务求解的一部分。

四、隔离执行与并行复现。每个任务在容器隔离中运行，任务之间做环境 reset，避免状态污染；同一套自托管基础设施支持并行评测。与真实网站相比，页面和数据不随外部漂移，因而可复现、可对照、可做消融。代价是自托管克隆与真实站点存在 domain gap，这是合理推断。

五、Rubric DAG 与 grader 仲裁。每个任务配一组人工撰写的 criteria，标记为 action 或 outcome：action rubric 检查是否执行了必要步骤（搜索、过滤、提交表单等），outcome rubric 检查网站最终状态或最终答案，问答题还带参考答案。rubric 被组织成有向无环图，一个节点依赖的节点失败则该节点计失败。每个节点可用一个或多个 grader：确定性 grader 查询网站后端状态或检查答案字符串；启发式 grader 匹配 URL 和动作模式；LLM judge 读取轨迹与 rubric。若 rubric 有明确唯一结果，确定性 grader 可单独判定；否则按 deterministic→heuristic→LLM judge 顺序尝试，任一 grader 通过即节点通过。

六、成功判定与部分完成。任务只有在所有 rubric 节点通过时才计成功，主指标是任务成功率；为捕捉失败任务中的部分进展，论文还报告 partial-completion score，即每任务通过的 rubric 比例。这个设计在严格性和信息量之间做了权衡。

七、评测协议。主实验在 50 步预算下比较 12 个模型骨干与 4 种 agent harness；同时报告跨站点/单站点、媒体内容依赖子集和媒体访问消融，并记录 API 成本。方法章节在给定文本中为空，以上细节主要来自摘要、Introduction、Results 片段和 Conclusion，部分实现细节需回原文核对。

Q4: 论文做了哪些实验？

一、实验矩阵。论文在 300 个任务上评测 12 个模型骨干与 4 种 agent harness 的组合，主实验采用 50 步预算。可见的 harness 包括 Codex、Claude Code、OpenCode 和 GenericAgent；可见的模型包括 GPT 6.1 Sol、Claude Opus 5.5、Kimi 3.1 Pro、Qwen 3.8 Max Prime、Gemini 3.1 Pro omni、Gemma4 31B 等。论文没有在给定片段中给出完整的 12×4 矩阵，需查主表。

二、主指标。每个轨迹由 rubric-based verifier 评分；任务成功要求所有 action/outcome rubric 节点通过，成功率为成功任务占比。另报告 partial-completion score，衡量未成功任务中通过的 rubric 比例。这个双指标设计既保留二值成功，又避免把“做了大半但最后一步失败”完全抹平。

三、主结果。最佳成绩为 Codex+GPT 6.1 Sol 与 Claude Code+Claude Opus 5.5 的 62.3%；OpenCode+Kimi 3.1 Pro 为 60.7%（摘要；草稿片段写作 60.3%，存在不一致）；最强 GenericAgent 配置 Gemini 3.1 Pro omni 与 Gemma4 31B 均为 30.3%。作者据此强调仍有明显 headroom。

四、分组与消融。论文区分单站点与跨站点任务，结论指出跨站点任务更难；还构造 media-content-dependent subset，审计任务元数据、缩略图和评分标准，选择真正依赖音频/视频内容的任务。媒体访问消融通过移除或限制 watch/listen/transcribe/extract_frames 等能力，检验媒体访问对成功率的影响；结论称消融说明访问音频和视频内容的重要性。给定片段没有给出具体消融差值，不能编造。

五、任务难度控制实验。候选任务在 triage 阶段用 LLM-based agent 试跑，第一次就正确完成的任务被丢弃；这相当于一次“难度过滤”，使基准避免被简单任务拉高成功率。该设计也会改变任务分布，使剩余任务更偏向多步骤、跨应用或需要媒体理解的情形。

六、成本实验。论文记录 300 任务评测的 API 成本，包含重试和恢复运行，不含托管基础设施：GenericAgent 约 USD 800（Claude Opus 4.7）；Claude Code+Opus 5.5 为 USD 386；Codex+GPT 6.1 Sol 为 USD 166；OpenCode+Qwen 3.8 Max Prime 为 USD 689；OpenCode+Kimi 3.1 Pro 为 USD 340。LLM judge 使用 Qwen 3.6 27B（经 OpenRouter），每次 300 任务评测低于 USD 4。

七、验证器实验。论文用 rubric DAG 和三类 grader 做自动验证，并用人工验证任务与 rubric。给定片段没有报告 grader 与人工判断的一致性、LLM judge 的偏差分析或跨模型 judge 的稳健性，这些是需要回原文/附录确认的评测可靠性证据。

Q5: 发现了什么实验现象？

一、能力上限远未饱和。最强组合 62.3%，意味着约四成任务失败；作者在结论中明确说 heterogeneous、multimodal web agents 仍有 considerable headroom。这个数字比“某个模型又涨了几个点”更重要：它说明在广应用、多模态、有状态条件下，当前 agent 仍不可靠。

二、harness 效应非常大。编码 agent harness（Codex、Claude Code、OpenCode）达到 60%–62% 左右，而最强 GenericAgent 配置只有 30.3%，接近两倍差距。合理解释是 coding harness 自带工具调用、文件/终端操作、长程规划和错误恢复脚手架；但论文 limitation 也提醒，比较的是模型、harness、工具配置的组合，不能把差异单纯归因于模型。这是实验中最重要的“混淆变量”之一。

三、跨站点任务显著更难。结论明确指出 cross-site tasks 比 single-site tasks 更具挑战性。跨站点要求 agent 在多个应用间传递状态、处理不同 UI 惯例和数据模型，接近真实知识工作；这也说明单站点成功率会高估整体能力。

四、媒体访问消融揭示多模态缺口。作者报告 media-access ablations 证明访问音频和视频内容的重要性；换言之，不能听/看媒体的 agent 在 media-content-dependent 子集上会退化。具体退化幅度未在给定片段中给出，但方向是清晰的：音频/视频不是装饰，而是任务信息的一部分。

五、成本-性能不是单调关系。Codex+GPT 6.1 Sol 以 USD 166 达到 62.3%，Claude Code+Opus 5.5 以 USD 386 达到 62.3%，OpenCode+Qwen 3.8 Max Prime 成本 USD 689（分数在可见片段中未完整给出），GenericAgent+Opus 4.7 约 USD 800 却只有 30.3% 量级。这说明 harness 架构和工具使用效率可能比单纯堆模型更影响单位成本收益；但成本含重试/恢复，跨配置比较需谨慎。

六、验证成本相对可控。LLM judge 用 Qwen 3.6 27B，300 任务评测低于 USD 4，远低于 agent 推理成本。这意味着评测瓶颈主要在 agent 运行而非 judge；但也意味着 judge 的能力有限，复杂 rubric 的判定可靠性需要额外验证。

七、rubric 严格性与部分完成的张力。任务成功要求所有 rubric 节点通过，可能把“接近正确但漏一步”判为失败；partial-completion score 用来补足这一信息。实验中如果出现成功率低但部分完成率高的情况，说明 agent 能完成大部分子目标、卡在关键收尾步骤；给定片段未给具体分布，需查附录 B.2。

八、难度筛选改变任务分布。LLM triage 丢弃第一次就成功的任务，使基准偏向多步骤、跨应用、媒体依赖或长尾操作。这提高了区分度，但也意味着成功率不能直接与未筛选基准对比；跨基准比较必须考虑任务筛选和 50 步预算。

九、评测归因需谨慎。作者自己指出性能差异不能一般性地归因于模型本身；模型版本、harness、工具、重试策略、步数预算和 judge 都可能影响结果。阅读主表时应优先看同一 harness 内的模型对比，以及同一模型跨 harness 的对比，而不是只看总排名。

Q6: 有什么可以进一步探索的点？

一、把评测环境转为训练环境。论文明确指出，同一套自托管基础设施可以做并行训练环境，rubric-based verifier 提供 per-task reward，用于强化学习或 rejection sampling。关键挑战是 reward 稀疏、rubric 全通过过于严格、跨站点长期信用分配，以及训练/评测任务污染。

二、鲁棒性与安全性研究。可控环境适合扰动页面，测试 UI drift、布局变化、文案改写、prompt injection、恶意媒体内容和数据外泄。特别是 1Password、Gmail、Salesforce 这类应用涉及凭证与敏感数据，agent 的权限边界和安全策略值得单独评测。

三、更丰富的时间模态。当前只覆盖预录音频片段和离散视频帧，未来可加入 live voice dialogue、streaming video、连续时间轴、实时字幕和交互式播放。这样能测量 agent 在动态媒体中的在线理解、打断、重播和时序推理。

四、扩大应用与领域覆盖。20 个应用、6 个类别仍是有限样本；可加入移动端 Web、垂直行业软件、开源 SaaS、非英语界面、无障碍界面和低带宽场景。也需要报告每个领域的任务数与统计功效，避免某些类别只有少量任务。

五、改进验证器与部分信用。可探索更便宜、更确定的 backend verifier、结构化状态 diff、可执行 rubric、人工抽检与 LLM judge 校准；同时研究 partial credit 的权重、关键节点失败与次要节点失败的区分，以及多路径正确解的等价类。

六、解耦模型、harness 与工具。未来工作应做 compute-matched、tool-matched 的对照，标准化 harness 接口，报告重试次数、步数、token 成本和失败类型，让“模型能力”与“脚手架能力”可分离。

七、污染与动态基准。公开任务可能被训练数据吸收；可维护私有 held-out 集、版本化任务、自动生成但人工验证的变体，以及定期刷新的应用版本，以保持区分度。

八、跨站点状态与错误恢复。真实工作流需要跨应用传递实体、处理登录/权限、从失败中恢复；可设计更长程、多会话、多用户协作任务，测量记忆、计划和恢复能力。

九、多智能体与人机协作。可把任务扩展为 agent 与人类或其他 agent 分工，测量通信、委派、冲突解决和审计追踪；这与企业工作流更接近。

十、成本与可及性。当前 300 任务评测成本从 USD 166 到 USD 800 不等；未来可研究缓存、轻量 verifier、分层评测和早停策略，让学术团队也能做大规模可复现评测。

Q7: 总结一下论文的主要内容

WebArena-Pro 是一篇基准论文，核心论证主线是：Web agent 的应用面已经扩展到异构应用、多媒体内容和有状态工作流，但可复现评测仍停留在少数应用、文本/视觉两种模态，导致能力测量与现实工作脱节。作者不是提出新模型，而是构建一个更广覆盖、可复现、多模态的测量基础设施。

第一，问题定义。现有自托管基准应用少、软件类别窄；真实网站基准覆盖广但页面和数据漂移，运行间不可比；音频/视频在网页任务中普遍缺位。论文要同时解决覆盖、复现和多模态三个缺口，并把“异构性”作为评测变量。

第二，基准构成。WebArena-Pro 包含 300 个任务，覆盖 20 个自托管 Web 应用和 6 个领域类别。从引言可辨识的应用包括 Slack、Gmail、X/Twitter、Dropbox、Google Drive、Notion、1Password、Google Flight、Airbnb、Spotify、Figma、Google Photos、YouTube、PowerBI、Pipedrive、Mint、Handshake、LMS、Zendesk、Salesforce；类别覆盖通信、生产力、旅行、设计与媒体、业务运营、分析。任务跨不同界面惯例、数据模型和工作流，既有单站点也有跨站点。

第三，任务构造。作者通过专用标注接口写题；候选任务先由 LLM-based agent 试跑，第一次就成功的题被 triage 丢弃；通过筛选的任务由第二标注者在真实应用上复核，确认可执行、最终确定 rubric，并做发布前人工验证。这个流程同时控制难度、有效性和可评分性。

第四，多模态观察与动作。基准把音频和视频作为核心观察，与文本、视觉并列；动作空间提供 transcribe()、extract_frames()、watch()、listen()，分别用于转写、固定帧率抽帧、观看和聆听媒体。网页观察以 accessibility tree 为主，配合视觉信息；agent 必须主动决定何时读取媒体。

第五，执行与复现。每个任务在容器隔离中运行，任务间 reset 环境，支持并行评测，避免页面漂移和状态污染。代价是自托管克隆与真实互联网存在 domain gap，这是合理推断。

第六，验证协议。每个任务配有人工撰写的 action/outcome rubric；rubric 组成 DAG，依赖节点失败则下游失败。grader 分三类：确定性 grader 查询后端状态或精确字符串；启发式 grader 匹配 URL/动作模式；LLM judge 读取轨迹和 rubric。明确唯一结果可由确定性 grader 单独判定；否则按 deterministic→heuristic→LLM judge 顺序尝试，任一通过即节点通过。任务所有节点通过才算成功；另报告 partial-completion score。

第七，实验。论文在 300 任务、50 步预算下比较 12 个模型骨干与 4 种 agent harness。最佳为 Codex+GPT 6.1 Sol 与 Claude Code+Claude Opus 5.5 的 62.3%；OpenCode+Kimi 3.1 Pro 为 60.7%；最强 GenericAgent 配置 Gemini 3.1 Pro omni 与 Gemma4 31B 为 30.3%。结论指出跨站点任务比单站点难，media-access 消融说明音频/视频访问的重要性，整体仍有明显 headroom。

第八，成本与讨论。300 任务评测的 API 成本从 Codex 的 USD 166、Claude Code 的 USD 386、OpenCode+Kimi 的 USD 340、OpenCode+Qwen 的 USD 689 到 GenericAgent 的约 USD 800 不等；LLM judge 成本低于 USD 4。作者提出未来可把环境用于 RL/rejection sampling、UI drift 与 prompt injection 鲁棒性研究，以及 live voice/streaming video 等更丰富时间模态。

第九，局限。论文自述限于预录音频和离散视频帧，缺少 live voice/streaming；比较的是模型+harness+工具组合，不能一般性归因于模型；300 任务、20 应用仍是有限样本；rubric 与 LLM judge 的可靠性、全节点通过的严格性、50 步预算和自托管 domain gap 都需要后续验证。

总体而言，这篇论文的贡献是把 Web agent 评测从“少数应用、文本/视觉、单站点”推进到“20 应用、6 类别、音频/视频一等模态、容器隔离可复现”，并用大规模 harness×model 矩阵给出当前能力上限。它的价值主要在测量设计和实证现象，而不是新算法；阅读时应重点关注任务分布、rubric 判定和 harness 混淆，而不是只看 62.3% 这个总数。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接对应 agent 方向：这是 Web/GUI agent 的评测基准，关注异构应用、多模态观察、长程状态和 harness 设计。

## 基本信息

- 作者：Imene Kerboua, Fatemeh Pesaran Zadeh, Xing Han Lù, Weijian Qi, Alexander Miller, Junyi Song, Yunjia Tian, Dongjin Kang, Seyeon Choi, Marzia Nouri, Ewen Gueguen, Matteo Boglioni, Fengyuan Liu, Zeyi Liao, Mengqi Yuan, Yue Li, Alexandre Lacoste, Alexandre Drouin, Spandana Gella, Huan Sun, Gunhee Kim, Siva Reddy
- 机构：McGill University / Mila – Quebec AI Institute（依据通讯作者邮箱 xing.han.lu@mail.mcgill.ca 与 siva.reddy@mila.quebec 推断；完整作者单位未在给定片段中列出，合作者单位需查原文）
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.LG
- 日期：2026-11-16
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.17955`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本报告优先参考了 PDF 语义检索命中的 Introduction/Results/Conclusion 片段与摘要；方法章节在给定文本中为空，故方法细节主要来自摘要、结果段和结论并标注为合理推断；摘要的 60.7% 与草稿片段的 60.3% 存在不一致，正文以 60.7% 为准，模型版本与日期也需核对正式论文。
