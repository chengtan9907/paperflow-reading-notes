---
user_id: "cheng tan"
paper_id: 15491
arxiv_id: "2610.13238"
title: "State-OPD: State-Grounded On-Policy Distillation for Multi-Turn Agents"
publish_date: "2026-10-12"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.13238.pdf"
pdf_url: "https://arxiv.org/pdf/2610.13238v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-12T13:16:30"
---
# State-OPD: State-Grounded On-Policy Distillation for Multi-Turn Agents

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：on-policy distillation · multi-turn agent · interaction state · reasoning distillation

## 一句话总结

本文提出 State-OPD（State-Grounded On-Policy Distillation），以 interaction state（任务指令+已执行动作-观测序列，剥离学生内部推理）为蒸馏单元，在多轮智能体场景下将教师监督锚定在交互状态、在高响应熵状态重采样完整 reasoning-action 响应，从而缓解继承推理对教师监督的扭曲并提升动作多样性；在 ALFWorld、ScienceWorld、WebShop 三个基准上，Qwen3-1.7B 与 Qwen3-4B 学生均取得最高宏平均成功率，其中 Qwen3-4B 从最强基线的 26.07% 提升到 31.74%，并在 ALFWorld Seen 上超越其 Qwen3-32B 教师。

## 摘要

> 论文研究多轮智能体（multi-turn agentic）场景下的 On-Policy Distillation（OPD），即用更强教师模型对学生自采样响应提供稠密监督。作者提出以 interaction state 为分析单元，将其定义为任务指令与已执行动作-观测序列（st = (g, o0, a0, o1, ..., at−1, ot)），与学生内部推理轨迹 z<t 分离。基于该视角识别出学生推理在 multi-turn OPD 中的两种角色：先前轮次的推理会扭曲教师监督，当前轮次推理会塑造同一状态下被采样的动作备选集。在匹配的 interaction state 下，去除先前学生推理可提升教师续写成功率并改变 token 级目标，且越靠后的轮次续写收益越大；在同一状态下重采样完整 reasoning-action 响应比在固定推理轨迹下仅重采样动作后缀带来更大的动作多样性。基于这些发现，作者提出 State-OPD：将教师监督锚定于 interaction state，并把采样预算重新分配到高响应熵状态，在这些状态生成多条完整 reasoning-action 响应，同时保留全历史学生生成（full-history student generation）。在 ALFWorld、ScienceWorld、WebShop 上，State-OPD 对 Qwen3-1.7B 和 Qwen3-4B 学生均取得所有被评估方法中最高的宏平均成功率；Qwen3-4B 从最强基线的 26.07% 提升到 31.74%，并在 ALFWorld Seen 上超越 Qwen3-32B 教师。进一步分析显示 State-OPD 以更少的受监督 turn 响应实现更高任务成功率，显著减少持久性失败动作重复；定性轨迹展示基于交互的决策修正（摘要截断）。

Q1: 这篇论文试图解决什么问题？

论文要解决的核心问题是：把 On-Policy Distillation（OPD）从单轮/短上下文场景扩展到多轮智能体（multi-turn agentic）环境时，沿学生生成轨迹的教师监督会随交互变长而变得不稳定或信息量下降，导致知识迁移不可靠。已有工作把这种退化归因于 rollout 误差累积或共享上下文压缩，并通过课程学习、教师辅助改写历史或跨轮次重分配 rollout/loss 预算来缓解；但这些方法都把学生历史（history）当作混合体处理，没有区分其中的两个组成部分：一是现实发生的动作-观测记录（realized action–observation record），二是学生对该记录的内部推理（reasoning）。论文指出，历史中的推理对监督有两种相反的作用：先前轮次的推理（prior/inherited reasoning）会扭曲教师对当前动作的监督目标；当前轮次的推理（current reasoning）则决定了在同一 interaction state 下被采样到的动作备选集合。如果不把二者拆开，就无法理解多轮 OPD 监督退化的真正机制，也难以设计有针对性的采样与监督策略。因此论文要解决的问题是：如何以 interaction state（而非完整历史）为组织单元，设计一种既能避免继承推理扭曲教师目标、又能拓宽同一状态下动作多样性的 on-policy 蒸馏框架。此外，论文还隐含回应一个效率问题：在给定监督预算下，如何把采样资源分配到最有信息量的状态（高响应熵状态），而不是均匀地沿所有 turn 展开，从而以更少的受监督响应获得更高的任务成功率。

Q2: 有哪些相关研究？

相关研究按几个脉络展开：（1）On-Policy Distillation（OPD）本身：通过教师对学生自采样响应提供稠密监督来 post-train 语言模型，代表工作包括 Agarwal et al., 2024; Gu et al., 2024; Lu and Lab, 2025; Jia et al., 2026，论文认为这是从强模型向前沿小模型迁移推理能力的有效范式。（2）多轮智能体 OPD 扩展：近期多条工作线把 OPD 引入 agentic 任务（Wang et al., 2026b; Li et al., 2026a; Liao et al., 2026; Zhou et al., 2026b; Sun et al., 2026; Wang et al., 2026a），在这些设定中每轮学生生成 reasoning trace 和可执行动作，环境返回 observation（Yao et al., 2023 的 ReAct 风格；Zhou et al., 2024）。（3）针对多轮监督退化的已有解法：一类通过 curriculum 或 teacher assistance 重塑 rollout 历史（Wang et al., 2026b; Li et al., 2026a; Liao et al., 2026），另一类在轮次间重分配 rollout 与 loss 预算（Zhou et al., 2026b）；已有分析把退化归因于复合 rollout 误差（compounding rollout errors）或共享上下文压缩（shared-context compression，Wang et al., 2026b; Zhou et al., 2026b）。（4）互补性的多轮对话蒸馏工作（论文引言提到 complementary work on multi-turn dialogue，推测涉及对话场景的蒸馏或监督信号设计，但摘要截断未给出具体引用）。（5）教师模型：实验使用 Qwen3-32B 作为冻结教师，Qwen3-1.7B 与 Qwen3-4B 作为学生（Yang et al., 2025），属于 Qwen3 系列。论文的差异化定位是：不直接修改历史内容或预算分配本身，而是先界定 interaction state 与内部推理的边界，从机制层面解释推理的两个角色，再据此设计 grounded supervision 与重采样策略；这个视角与已有把退化归因于误差累积/上下文压缩的解释形成竞争关系，可以被视为对已有解释的细化或替代。

Q3: 论文如何解决这个问题？

State-OPD 的解决方案分三层：第一层是形式化 interaction state。论文定义 st = (g, o0, a0, o1, ..., at−1, ot)，即任务指令加完整的已执行动作与观测序列，但显式省略学生推理 z<t；不同语义轨迹若执行动作和观测相同则共享同一 st。Proposition 1（Interaction-state sufficiency）论证：若每个响应仅通过其可执行动作 at = ψ(rt) 影响环境、且所有环境反馈都记录在观测中，则 st 保留 ht 关于潜在环境状态 ξt 的全部信息，即 p(ξt | ht) = p(ξt | st)。这为把监督锚定在 st 上提供了理论依据。第二层是机制发现驱动的设计。在第 3.2 节，论文在匹配的 interaction state 下做受控比较：去除先前学生推理（prior reasoning）后，教师续写成功率提升，且教师 token 级目标改变；越靠后的轮次，这种续写收益越大，表明继承推理随轨迹变长而越发扭曲教师监督。第 3.3 节显示：在同一状态重采样完整 reasoning-action 响应，比在固定推理轨迹下仅重采样动作后缀产生更多不同动作，说明当前推理的变化能暴露动作备选空间。第三层是具体算法：State-OPD 保留根轨迹（root trajectory）中的全部响应，在选定状态上从同一历史 hi,t 重新生成完整推理与动作来扩充响应池；对每个根响应 ri,t（长度 Li,t）计算响应熵（response entropy），把采样预算重新分配到处于根轨迹最高熵一半的状态，每个这样的状态在参考重数 M = 4 的基础上额外分配 β = 2 个响应（论文固定 β = 2，M = 4，跨环境和模型规模不变），使每个状态的监督总数约为 6 个。教师监督目标通过 anchor depth 与 teacher tokens 构造（RKL 相关图示显示在训练步上比较基于 ht 与基于 st 的 reverse KL 及其差值 ΔRKL，合理推断训练目标是对学生响应逐 token 计算教师分布的 KL 或类似稠密监督）。训练中学生仍以 full history 生成（retaining full-history student generation），因此推理能力不会被截断历史削弱。所有方法用 veRL 实现，32 张 96GB GPU，每配置单一训练种子 42。

Q4: 论文做了哪些实验？

实验设计包括以下部分：（1）主实验：从冻结的 Qwen3-32B 教师蒸馏 Qwen3-1.7B 与 Qwen3-4B 学生，在 ALFWorld、ScienceWorld、WebShop 三个环境上评估；五个评估设置（ALFWorld Seen、ALFWorld Unseen、ScienceWorld、WebShop 的若干子设置，表注说明 Avg. 是五个评估设置的宏平均）计算宏平均成功率。所有训练 baseline 每个任务采样 4 个 rollout；State-OPD 使用对应参考重数 M = 4，β = 2，向根轨迹最高熵一半的状态各分配 6 个额外响应，同时近似匹配 baseline 的 turn-response 监督预算。batch size 16 任务，veRL 实现，32 卡（96GB/卡），训练种子 42。（2）机制验证实验（第 3.2 节）：在匹配 interaction state 下，对比保留 vs 移除先前学生推理时教师续写的成功率与 token 级目标变化，并按 Early/Middle/Late 轮次分组统计；图 1 展示 RKL(ht) 与 RKL(st) 随训练步的变化及 ΔRKL，以及不同 anchor depth 下的差异。合理推断这是用于支撑'继承推理扭曲监督'结论的诊断实验。（3）动作多样性实验（第 3.3 节）：在同一 interaction state 下，比较重采样完整 reasoning-action 响应与在固定推理轨迹下仅重采样动作后缀的动作多样性；图 3b 显示更高响应熵与更多不同动作的关联，用于支撑'当前推理塑造动作备选集'的结论。合理推断使用同一组初始根轨迹的中间状态做多次重采样并统计唯一动作数。（4）效率/消融分析：报告受监督 turn 响应数量与任务成功率的关系，说明 State-OPD 用更少的监督响应达到更高成功率；统计持久失败动作重复（persistent failed-action repetition）的减少。（5）定性轨迹分析：展示成功案例轨迹，说明模型基于 interaction 信息进行决策修正（摘要末尾的 'qualitative traces illustrate decision revision based on interaction…' 被截断）。

Q5: 发现了什么实验现象？

实验现象与发现包括：（1）继承推理扭曲教师监督：在匹配 interaction state 下，移除先前学生推理后教师续写成功率提升，教师 token 级目标改变；该续写收益随轮次后移而变大（Early→Middle→Late 递增），说明推理污染随轨迹变长累积，支持'监督退化源于继承推理而非仅上下文压缩'的解释。（2）当前推理决定动作多样性：同一 interaction state 下重采样完整响应比固定推理重采样动作后缀产生更多不同动作，说明动作多样性主要来自推理路径的变化而非动作后缀本身的随机性；响应熵越高，越容易产出多个不同动作（图 3b）。（3）训练动态差异：RKL(ht) 与 RKL(st) 在训练步上的轨迹不同，ΔRKL 随 anchor depth 变化，表示锚定在当前状态 vs 完整历史会改变教师目标的质量（从图 1 合理推断）。（4）主结果：State-OPD 在 ALFWorld、ScienceWorld、WebShop 上对 Qwen3-1.7B 与 Qwen3-4B 都取得最高宏平均成功率；Qwen3-4B 从最强基线的 26.07% 提到 31.74%，并在 ALFWorld Seen 上超过其 Qwen3-32B 教师——小模型在 seen 环境蒸馏后反超教师本身是一个反直觉结果。（5）效率现象：State-OPD 在更少的受监督 turn 响应下获得更高任务成功率，说明预算重分配（聚焦高熵状态）比均匀覆盖所有轮次更高效。（6）失败模式改善：持久失败动作重复显著减少（reduces persistent failed-action repetition），表明高熵状态重采样不只提升成功率，还改变了失败行为结构——模型不再反复执行同一失败动作。（7）定性轨迹显示决策修正基于 interaction 信息（摘要截断处）。需要说明：受可用片段限制，各 baseline 的具体数值、各环境分项成功率、Rounds 指标的具体读数未在检索证据中完整呈现，只能确认表 1 报告了 SR 与 Rounds，宏平均的核心数字已由摘要确认。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向：（1）理论深化：Proposition 1 建立在'响应只通过可执行动作影响环境、所有反馈都在观测中'的假设上；对内部推理本身能改变环境（如 memory 操作、工具调用副作用）或部分环境反馈不完全可观测的设定，需要扩展 interaction-state sufficiency 的条件。（2）状态抽象与泛化：当前 st 是原始动作-观测序列，不同任务/环境的状态空间差异大；可探索隐式状态表示或状态聚类，使高熵状态选择与监督锚定跨环境迁移。（3）熵度量的选择：论文用响应熵（response entropy）选择重采样状态，其他不确定性度量（如教师-学生分歧、价值估计、状态访问频率）可能更适配不同环境；可研究 entropy 阈值与预算分配的 adaptive 策略。（4）多教师与在线教师：当前是冻结 Qwen3-32B 教师，可探索教师随训练更新、或混合多尺度教师监督在 interaction-state grounding 下的表现。（5）学生规模与架构边界：实验覆盖 1.7B 与 4B 学生；更小（0.6B 出现在图 1 诊断中）或更大的学生、以及非 Qwen 系列模型上的表现值得验证。（6）与其他多轮 OPD 改进的组合：课程化历史、教师辅助改写、跨轮预算重分配等方法与 State-OPD 的机制正交，组合使用可能进一步缓解推理污染并提升多样性。（7）训练效率与扩展：当前每配置单种子（42），batch size 16，32 卡；可扩展到多种子、更大 batch 以确认统计显著性，并研究高熵重采样对算力的实际开销。（8）失败行为分析：论文观察到持久失败动作重复减少，可进一步分析失败轨迹的转移结构、动作循环检测与干预策略，把'少重复失败'转化为显式训练目标。（9）部署应用：ALFWorld/ScienceWorld/WebShop 是模拟文本环境，可迁移到真实 API 调用、浏览器自动化等更复杂 agentic 场景，并评估推理污染在不同环境噪声下的严重程度。

Q7: 总结一下论文的主要内容

论文研究多轮智能体场景下 On-Policy Distillation（OPD）的监督退化问题，并给出一个以 interaction state 为组织单元的新框架 State-OPD。论证主线是：现有 OPD 在 agentic 任务中沿学生完整历史（推理+动作+观测）构造教师目标，导致两类问题被混在一起——继承的推理扭曲教师监督、当前推理决定动作备选集；论文通过把 interaction state 定义为任务指令加已执行动作-观测序列（st = (g, o0, a0, o1, ..., at−1, ot)），将实现层面的事实与学生对其的解释分离，并以 Proposition 1 论证 st 对潜在环境状态的信息充分性。技术主线包含三个成分：机制诊断（在匹配状态下去除先前推理的受控实验，发现教师续写成功率提升、token 级目标改变、越靠后轮次收益越大；重采样完整响应的动作多样性高于固定推理下重采样动作后缀）、监督 grounding（教师目标锚定在 st 而非 ht）、预算重分配（在高响应熵状态额外生成完整 reasoning-action 响应，保留根轨迹全响应，M = 4、β = 2）。实验主线是：用 veRL 在 32 卡上从 Qwen3-32B 蒸馏 Qwen3-1.7B 与 Qwen3-4B，在 ALFWorld、ScienceWorld、WebShop 五个评估设置上做宏平均；State-OPD 在两个学生规模上都取得最高宏平均成功率，Qwen3-4B 从最强基线的 26.07% 提升到 31.74%，并在 ALFWorld Seen 超越教师 Qwen3-32B；进一步分析显示更少的受监督 turn 响应带来更高成功率、持久失败动作重复显著减少、定性轨迹展示基于 interaction 的决策修正。论文结论把 interaction state 确立为 agentic 知识迁移的组织单元，强调把现实交互与学生推理分离为 grounding 监督和探索备选决策提供了共同基础。整体上这是一篇机制分析驱动的蒸馏方法论文，兼具诊断性发现、理论形式化（信息充分性命题）与工程验证。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：论文与智能体方向直接相关：它处理的是 agent 场景下 on-policy 训练/蒸馏的核心难点——沿学生生成轨迹的监督退化与动作多样性不足。

## 基本信息

- 作者：Zhentao Zhang, Yanzhao Zheng, Yanlin Liu, Yuanqiang Yu, Tianze Xu, Chao Ma, Jihuai Zhu, Lelun Wu, Baohua Dong, Hangcheng Zhu, Ruohui Huang, Gang Yu
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-12
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.13238`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成精读报告时参考了论文的简介、方法、结论等部分及 PDF 语义检索命中片段；对未出现在检索证据中的具体数值与细节已做标注，并明确区分了论文明确支持的内容与合理推断的内容。
