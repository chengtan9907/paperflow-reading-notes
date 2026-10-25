---
user_id: "cheng tan"
paper_id: 16091
arxiv_id: "2610.18861v1"
title: "Hybrid Reasoning Agents"
institution: "论文首页通讯信息显示 jiaru@stanford.edu 与 jamesz@stanford.edu，Acknowledgments 提到 Zou Lab、iDEA-iSAIL Lab、Stanford NLP Group 与 Together Agent Team，代码托管在 togethercomputer 的 GitHub 组织下，因此合理推断作者机构包括 Stanford University（Zou Lab / Stanford NLP）与 Together AI，以及 iDEA-iSAIL Lab 相关机构；具体每一位作者的单位归属与完整机构列表需从 PDF 首页脚注确认。"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.18861v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.18861v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-18T02:06:31"
---
# Hybrid Reasoning Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：hybrid reasoning agents · latent reasoning · llm agents · tool use

## 一句话总结

提出 HybridAgent（Hybrid Reasoning Agents），一种在统一生成过程中让 LLM 智能体在“文本模式”与“潜在（latent）推理模式”之间动态切换的混合推理框架，通过轻量模块 HybridLink（含连续工作记忆）衔接两种模式，并配套 HybridSFT 与 HybridRL 两阶段后训练算法，在 11 个基准、3 个模型规模上相较前沿 LLM 智能体与智能体框架平均提升 10.4%，同时带来 35.5%–54.7% 的 token 缩减、1.8×–2.6× 推理加速与 2.7×–3.7× 的成本节约。

## 摘要

> 论文的核心问题设定是：能否构建一个“思考超出人类语言、但行动仍通过语言发生”的智能体。现代 LLM 智能体完全依赖文本进行推理与交互，而潜在空间推理（latent reasoning）虽然高效，却缺少工具调用通路与可解释性。HybridAgent 通过一个轻量级模块 HybridLink 把文本与潜在两种生成模式统一起来，允许模型在单个生成过程中动态切换；其连续工作记忆跨多轮保存无损失的混合上下文。在此架构之上，论文设计了两个后训练阶段：HybridSFT 教会主干模型与 HybridLink 协调，稳定“工具集成 + 混合生成”；HybridRL 通过强化学习让模型学会“何时在潜在空间思考、何时通过文本行动”。评测覆盖 3 个模型规模、11 个基准，横跨数学、科学、医学与智能体任务，对比文本型与潜在型方法。结果表明 HybridAgent 一致提升性能约 10.4%，同时实现 35.5%–54.7% 的 token 缩减、1.8×–2.6× 更快的推理速度和 2.7×–3.7× 的成本节省。代码开源在 github.com/togethercomputer/HybridAgent。

Q1: 这篇论文试图解决什么问题？

论文试图解决的核心问题是：主流 LLM 智能体的“思考介质”与“行动介质”被强制绑定为同一种模态（文本）。具体可以拆成三个相互关联的子问题：(1) 效率问题——前沿智能体在复杂任务上把大量中间推理轨迹逐 token 地写成文本，推理成本随任务难度急剧上升，token 消耗和延迟成为规模化部署的瓶颈；(2) 智能体的能力边界问题——latent reasoning 一系工作证明把计算展开在连续隐藏空间里可以更高效地完成内部推理（模型自己的“语言”），但这些潜在推理器被密封在模型边界之内，缺乏跨多轮调用工具的原生通路，因而难以直接用于 agentic 场景；(3) 可监督性与可审计性问题——连续的潜在向量不像解码后的文本那样暴露人类可理解的语义，训练时难以施加细粒度监督，推理时也难以审计与调试。论文的内在主张是：文本与潜在推理不是二选一的对立路线，而应是同一个生成过程中可动态切换的两种模式；智能体应该保留文本的工具交互能力与可检查性，同时获得潜在推理的高效内部计算。因此问题被重新定义为“如何为一个统一生成过程设计架构和训练算法，让模型自己学会在两种介质之间导航”。

Q2: 有哪些相关研究？

相关工作可以从三条线索组织：1) 文本智能体（Text-based LLM agents）：这是当前的主流范式，以 ReAct 这类“推理-行动”循环为代表，智能体的所有中间推理都以文本形式生成，再通过工具与环境互动；引言指出前沿智能体的扩展越来越依赖外部世界交互（不断调用专门工具、观察环境反馈、调整下一步，相关引用包括 Kwa et al. 2026、Sun et al. 2026、Feng et al. 2026、Qu et al. 2025 等），而贯穿所有这些智能体框架的“通用语”是文本——既作为自回归生成的介质，也作为环境交互的介质。2) 潜在推理（Latent reasoning）：与文本路线相对，把计算直接在连续隐藏空间展开，绕过“把每步都解码成语词”的开销；引言引用 Hao et al. 2024（Coconut，用连续潜在思维替代文本思维链）、Wang et al. 2025、Yu et al. 2026、Chen et al. 2025、Zhu et al. 2025b 等，指出这类方法在模型“自己的语言”里做内部推理更高效，但现在仍然是封闭在模型边界之内：缺少跨多轮 agentic 推理中的工具调用通路（Chang et al. 2026），也不容易暴露人类可解释语义，导致训练监督（Chen et al. 2026）与推理期审计困难。3) 混合/切换式推理的早期尝试与 RL 训练：检索证据中出现的参考文献条目 “den-state recurrence: Switchable latent reasoning with on-policy reinforcement learning” 暗示已有把潜在推理与强化学习结合的相邻工作；论文在方法层面自己也对比了 GRPO（Shao et al.）与 ARPO 等 RL 算法在 latent/text 混合场景下的表现。此外 agentic frameworks 一栏对比了 Search-o1、WebThinker、ReAct，baseline 还包含 QwQ-32B、GLM-5.3-Flash、DeepSeekV4.1-Flash、Kimi-K3、GPT-5.6-Luna 等前沿模型，说明评测视野同时覆盖“推理模型当智能体直接使用”和“专门 agentic harness”两条路线。值得注意：论文主张这些工作要么全文本、要么全潜在，没有在统一生成过程中同时保留工具集成与潜在思考的可检查性，这正是 HybridAgent 的生态位。

Q3: 论文如何解决这个问题？

论文的解决方案分架构与训练两层。架构层：核心是 HybridLink——一个轻量级模块，连接文本 token 嵌入空间与模型最后一层隐藏状态（last-layer hidden states）。当模型决定进入潜在模式时，HybridLink 接收最后一层隐藏状态 h，通过线性层 W1 + GELU 加线性层 W2 的变换映射到 token embedding 空间，与残差连接（Residual Connection）相加后作为下一 token 的输入嵌入，实现“文本→潜在→文本”的无缝切换；图示中的特殊控制 token <latent> 与 </latent> 负责标记潜在段的进入与退出，从而让潜在思考段落在生成流中是“可见、可定位”的。同时，HybridAgent 维护一个“连续工作记忆”（continuous working memory），跨多轮对话保存完整的混合轨迹（文本、潜在思考、工具观察结果），实现多轮无损失的混合上下文传递。这一设计使潜在思维直接可检查（inspectable），同时保留工具集成能力。训练层：作者强调不需要从零预训练，而是把标准 agentic 后训练管线适配到混合设定，包括两个互补阶段。(1) HybridSFT（监督微调）：教会主干模型与 HybridLink 协调，在工具调用与思考之间稳定地产生连贯的混合生成，先让混合生成“会说话”；(2) HybridRL（混合强化学习）：不能直接套用 PPO/GRPO 这类流行 RL 算法（heuristic_draft 明确指出这一点，推测原因是离散文本 token 与连续潜在向量的混合动作空间、奖励信号如何在两种模式间传播等问题），因此论文为混合推理专门设计了 RL 目标/奖励（检索片段中出现 r_format、r_tool 两个奖励组件），让模型在训练中学会“何时用潜在空间想、何时用文本行动”。消融证据显示：去掉 r_format 会使性能下降 4.7%–6.9%；去掉 r_tool 的影响在 GAIA、WebWalker 这类 agentic 基准上最明显——说明格式奖励保证混合轨迹结构稳定，工具奖励专门支撑工具调用行为。

Q4: 论文做了哪些实验？

评测设计覆盖三个模型规模（8B、14B 以及另一个规模，从表格看还包括更大的级别；检索证据与表格残片至少完整呈现了 8B-level 与 14B-level），11 个基准，领域含数学（Math）、科学（Science，NS/CE/SF 子类，Easy/Med/Hard 难度分层）、医学（Medicine）与智能体任务（如 GAIA、WebWalker），并列了 Avg 汇总列。Baseline 分三层：前沿 LLM 智能体层（QwQ-32B、GLM-5.3-Flash、DeepSeekV4.1-Flash、Kimi-K3、GPT-5.6-Luna）、智能体框架层（Search-o1、WebThinker、ReAct）、以及 8B/14B 量级的潜在/文本方法层（Qwen3-8B/14B thinking、Coconut、SoftThinking、GRPO、ARPO）。主结果表格显示 HybridAgent (8B) 在多个子集上大幅领先同规模方法，例如数学部分达到 51.3/44.2/25.0、Avg 43.7（对比 GRPO 48.7/25.0/8.3、ARPO 53.9/32.7/16.7），科学部分 Avg 42.5（对比 ARPO 32.0、GRPO 30.0），医学 Hard Acc 22.5 等；14B 级别同样有对应对比行。除了性能，论文还测量了 token 缩减、推理延迟（1.8×–2.6× faster inference）、成本节省（2.7×–3.7×）。方法部分还包含专门的消融实验：HybridLink 层数（1-Layer vs 1-Layer+Res.）——加入残差连接在不同层深上平均带来 4.3% 的提升；奖励组件消融——w/o r_format 性能下降 4.7%–6.9%，w/o r_tool 在 GAIA/WebWalker 上影响最大。检索片段还提到调试（debugging）实验设计：使用第一错误定位（first-error localization, Zheng et al.）评估，并把 GPT-5.6-Luna 作为能力更强的监控模型、与主要依赖可见轨迹（visible trace）而非独立解题的设定做对比——这正是“latent thoughts 可直接检查”这一主张的实验体现。

Q5: 发现了什么实验现象？

实验现象按主题可以归纳为六点。(1) 混合模式的主效应：把潜在推理与文本行动统一到一个生成过程后，8B 的 HybridAgent 在数学 Avg 43.7 vs ARPO 38.8、科学 Avg 42.5 vs ARPO 32.0、医学 Hard Acc 22.5 vs 最优文本/潜在 baseline 的显著提升，说明潜在推理与工具交互不是互相削弱而是互补；一个反直觉点是：在科学基准上 HybridAgent 相对最强 latent baseline（ARPO 32.0）提升超过 10 个点，而它在数学 Easy 上 51.3 < ARPO 的 53.9——简单任务上纯文本基线反而略高，提示混合模式的价值主要在中等/困难任务上兑现（Med 44.2 vs 32.7、Hard 25.0 vs 16.7 都是大幅领先）。(2) 效率与性能同时改善：token 减少 35.5%–54.7%、推理加速 1.8×–2.6×、成本节省 2.7×–3.7×，且这种效率收益不是以性能为代价，而是伴随性能提升——这是论文最想传达的非零和结果。(3) 残差连接的作用：1-Layer HybridLink 加上残差后平均提升 4.3%，且该趋势在不同层深一致，验证了设计动机（推测残差提供了从文本嵌入空间到潜在空间的稳定信息通路，防止深层变换丢失 token 级信息）。(4) 奖励设计的角色分工：去掉 r_format（格式奖励）使性能下降 4.7%–6.9%，说明混合轨迹的结构稳定性（何时进出 latent、格式是否合法）对整体性能是必不可少的；去掉 r_tool（工具奖励）在 GAIA、WebWalker 上伤害最大，说明工具调用行为需要专门奖励来维持，文本 RL 的通用奖励不足以支撑 agentic 能力。(5) 潜在推理的可检查性：即使模型在 latent 模式下思考，其轨迹仍能渲染/检查，且调试实验发现监控模型（GPT-5.6-Luna）主要依赖可见轨迹而非独立解题——这说明潜在思考段落在实践中确实提供了可供审计的语义内容，而非完全不透明。(6) 与前沿大模型的对比：HybridAgent（8B 与 14B 级）在部分基准上能追平甚至超过 GPT-5.6-Luna（Frontier 层平均约 37.9–49.4）等大得多的模型，例如科学 Avg 42.5 vs Kimi-K3 31.4、GPT-5.6-Luna 29.0，医学 Hard Acc 22.5 vs 各 frontier 模型 9.0–23.3 区间，说明小模型的混合推理有追赶大模型智能的潜力。需要说明：表格残片只覆盖部分行列，benchmark 平均的权重方式、置信区间等细节需回原文确认。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向包括：(1) 扩展模型规模与任务面：论文在 8B/14B 及更大规模验证，但更大参数量（如 70B+）下混合模式的收益曲线、潜在思考长度与文本长度比例如何随规模变化，尚待检验；(2) 训练算法深化：目前 HybridRL 已证明不能直接套用 PPO/GRPO，后续可探索专为“离散文本动作 + 连续潜在思考”混合动作空间设计的策略优化算法、潜在空间本身的 reward shaping 或过程监督（process supervision）直接作用于连续向量；(3) 潜在思维的内容控制与可解释性：当前 latent thoughts 可检查但语义透明度有限，可以研究如何在训练时约束潜在段落在人类可理解的概念空间内编码，或者用稀疏编码/可解释性工具（如 SAE）把潜在段翻译回自然语言，让“思考可审计”更强；(4) 更丰富的工具与多模态环境：论文验证了搜索类工具场景（GAIA、WebWalker），可扩展到代码执行、数据库查询、机器人控制等动作空间更多样的环境，以及多模态观察（图像、音频反馈）下的混合推理；(5) 记忆机制的深化：连续工作记忆目前保存混合轨迹，可以进一步研究记忆压缩、遗忘机制、长程规划中潜在记忆与文本记忆的分工；(6) 稳健性与边界条件：混合推理在分布外任务、对抗性提示、工具失败/反馈缺失场景下如何退化，值得系统研究；(7) 多智能体与协同场景：HybridAgent 之间或与纯文本智能体协作时，潜在思考是否要共享、以什么形式共享，是一个开放问题；(8) 面向科学应用：结合用户画像中的 AI-for-Science 偏好，可探索把混合推理用于科学文献推理、实验流程规划、分子/材料设计等需要“长程内部推理 + 外部工具验证”的任务。

Q7: 总结一下论文的主要内容

论文的论证主线：前沿智能体的扩展瓶颈已经从模型自身推理能力转移到与外部世界的交互能力上，但无论 harness 怎么变，文本始终是智能体生成与环境交互的唯一通用介质，所有中间推理都要逐 token 解码成文本；与此同时，潜在推理展示了在连续隐藏空间展开计算的效率优势，却受限于没有工具通路、不可监督、不可审计。作者由此提出一个融合命题：智能体应当“思考超出人类语言，但仍通过语言在外部环境行动”，文本与潜在推理应当作为统一生成过程中可动态切换的两种模式，而不是互斥路线。技术主线：架构上，HybridLink 是连接 last-layer hidden state 与 token embedding 空间的轻量模块（线性+GELU+线性+残差），配合 <latent>/</latent> 控制 token 标记潜在段，使潜在思考在生成流中可见；连续的 working memory 跨多轮携带“文本+潜在思考+工具观察”的完整混合轨迹。训练上，作者明确不重新预训练，而是把标准 agentic 后训练管线适配到混合设定：HybridSFT 先让主干与 HybridLink 学会协同生成稳定的混合轨迹；HybridRL 再让模型学会策略性地选择“在潜在空间思考 vs 用文本行动”。奖励设计包含格式奖励（保证混合轨迹结构合法）与工具奖励（支撑工具调用行为），消融显示两者分别带来 4.7%–6.9% 与 agentic 基准上的显著贡献。实验主线：在 3 个模型规模、11 个基准（数学/科学/医学/智能体任务）上与三类基线对比——前沿 LLM 智能体（QwQ-32B、GLM-5.3-Flash、DeepSeekV4.1-Flash、Kimi-K3、GPT-5.6-Luna）、agentic 框架（Search-o1、WebThinker、ReAct）以及同规模的 latent/text 方法（Coconut、SoftThinking、GRPO、ARPO、Qwen3 thinking）。主结果：性能平均提升 10.4%，token 缩减 35.5%–54.7%，推理加速 1.8×–2.6×，成本节省 2.7×–3.7×。消融与诊断实验揭示了残差连接（+4.3%）、格式奖励、工具奖励各自的贡献，以及“潜在思想可直接检查/调试”这一设计主张的实证支撑。结论上，作者把 HybridAgent 定位为下一代智能体的可扩展高效框架：保留工具集成与环境交互，同时获得潜在推理的效率与可检查性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：如果你是做 LLM agent 的，这篇论文直接挑战“智能体推理必须全文本”的主流范式，提供了 latent reasoning 进入 multi-turn agentic 场景的架构与训练方案。

## 基本信息

- 作者：Jiaru Zou, Yunzhe Qi, Sirui Chen, Federico Bianchi, Shang Zhu, Pan Lu, Jingrui He, James Zou
- 机构：论文首页通讯信息显示 jiaru@stanford.edu 与 jamesz@stanford.edu，Acknowledgments 提到 Zou Lab、iDEA-iSAIL Lab、Stanford NLP Group 与 Together Agent Team，代码托管在 togethercomputer 的 GitHub 组织下，因此合理推断作者机构包括 Stanford University（Zou Lab / Stanford NLP）与 Together AI，以及 iDEA-iSAIL Lab 相关机构；具体每一位作者的单位归属与完整机构列表需从 PDF 首页脚注确认。
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.LG
- 日期：2026-10-13
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.18861v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的 Introduction/Method/Conclusion/References 证据片段（field_evidence_map 与 retrieved_evidence），并结合论文摘要与表格残片进行归纳；部分消融数值、RL 奖励定义与机构归属属于基于片段的合理推断，需回原文核对。
