---
user_id: "cheng tan"
paper_id: 16477
arxiv_id: "2610.26811v1"
title: "State World Models: Execute the Rules, Generate the Pixels"
publish_date: "2026-10-19"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.26811v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.26811v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-20T13:05:15"
---
# State World Models: Execute the Rules, Generate the Pixels

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：world model · video generation · stateful interaction · counterfactual evaluation

## 一句话总结

Stateful Harness 将世界状态演化（规则执行）与视频渲染解耦，用一个显式维护语义状态和空间状态的控制器驱动冻结的视频生成模型，从而在隐藏状态不可见于当前帧的情况下实现有状态、受规则约束的交互；在 WorldState-Bench 的 50 对反事实孪生样本上，冻结的 MiniMax-H3 在仅规则提示时为 32% PAIRFOLLOW，经完整 harness 提升到 92%。

## 摘要

> Existing world models can generate action-conditioned video from a single image. Many focus on camera movement, allowing users to explore the generated scene. However, navigation alone does not ensure that an action follows persistent rules when its outcome depends on state that is invisible in the current frame. We present Stateful Harness, a framework that separates world evolution from video rendering to support stateful and rule-governed interaction. It maintains an evolving world state under specified rules, renders each resulting event with a frozen video model, and uses the generated clip to update the world state for the next turn. At each turn, the harness resolves the action selected by the user and compiles the resulting event into an instruction, while a preview clip from an editable 3D scene provides spatial and camera guidance. To evaluate rule compliance, we build WorldState-Bench, a benchmark of 50 counterfactual twin pairs that share an initial frame and action but differ in hidden state and required outcome. Under rule-only prompting, the best of the 11 evaluated video backends produces the required endings for both cases in 32% of pairs. For MiniMax-H3, specifying the required endings raises this rate from 32% to 64%, while the full harness reaches 92% with the same frozen model. These results support separating rule execution from video rendering for world interactions with hidden state.

Q1: 这篇论文试图解决什么问题？

论文要解决的核心问题是：视频世界模型中的交互控制只停留在导航或动作条件生成层面，生成器对“同一可见画面 + 同一动作”是否会因隐藏状态不同而产生规则要求的、且跨回合持续的结果，并没有保证。具体而言：(1) 导航控制只能改变相机视角，无法决定与场景交互的后果；(2) 动作条件视频模型能给出响应式视频，但“响应”不等于“符合世界状态与规则所要求的结果”，例如开锁动作只有在玩家持有钥匙时才应打开箱子，即使初始画面完全相同；(3) 这类区分需要语义状态（如库存、进度变量）与可寻址的空间状态（如场景布局、相机路径）跨回合持久化；(4) 已有系统虽然也引入显式状态（可编程引擎、学习的游戏变量、重建的 NPC 状态），但其评测集中于挑选的机制，未回答一个关键问题：一个冻结的、不重新训练的视频模型能否在外部规则与状态管理下达到可靠的规则遵从。论文用反事实孪生对（counterfactual twin pairs）的形式把该问题操作化：同一初始帧、同一动作、不同隐藏状态，必须产出两种不同的规定结局。

Q2: 有哪些相关研究？

相关工作分几支：(1) 视频生成基座模型：Wan2.2（Wan et al., 2025）、HunyuanVideo 1.5（Wu et al., 2025）、LongCat-Video（LongCat Team et al., 2025）、MiniMax-H3（Chen et al., 2026a）等，能生成视觉逼真、运动连贯的视频；(2) 交互式视频世界模型：LingBot-World v2、Echo-WM Flash、FantasyWorld、AlayaWorld、InSpatio-World、Astra、HY-WorldPlay 等，通过导航信号或玩家/智能体动作条件生成，支持在场景中移动或更丰富的交互（Valevski et al., 2025; Zhu et al., 2026a）；(3) 显式状态建模方向：可编程引擎、学习的游戏变量、重建的 NPC 状态（Huang et al., 2026; Lin et al., 2026; Deng et al., 2026），这些系统在任务中整合状态但评测集中于特定机制；(4) 相机与几何状态追踪/记忆：Nam et al., 2026、Yu et al., 2025a、Li et al., 2025b 等，对可导航性和连续性有价值（合理推断：作者认为这类几何追踪并不测试规则遵从）；(5) 后续相关讨论见 Ding et al., 2026、Xu et al., 2026a、Fang et al., 2026、Yang et al., 2026（从检索片段可确认这些引用出现在 Introduction，具体主张需回原文确认）。

Q3: 论文如何解决这个问题？

方案是 Stateful Harness，核心思想是“执行规则、生成像素”分离：规则执行与视频渲染解耦。harness 维护两个显式状态：(a) 语义账本（semantic ledger）S_sem，记录库存、进度等变量；(b) 空间状态 S_sp，记录场景布局与相机路径，通过一个可编辑的 3D 场景（editable clay model）提供。每个回合：(1) 解析用户选择的动作；(2) 依据确定性规则（deterministic rules）在语义账本上结算（settle）动作的后果，更新世界状态；(3) 将结算后的事件编译为指令（instruction），供视频模型执行；(4) 从可编辑 3D 场景生成预览片段，提供空间与相机引导；(5) 用冻结的视频模型（frozen video world model Vθ）渲染每个事件；(6) 渲染生成的片段反过来作为视觉观察更新世界状态，供下一回合使用。渲染失败不会逆转规则程序已经确定的事件——失败策略可以是重试请求、复用前一锚帧或终止生成（论文 Algorithm 1 描述的是成功执行路径；失败路径细节需回原文补读）。该方案的关键设计点是：所有“记账”和规则判断发生在显式状态与规则程序中，而不是期望视频生成器隐式地学会状态与规则；视频模型只负责把已确定的事件渲染成像素，因此权重可完全冻结、无需重新训练或微调。

Q4: 论文做了哪些实验？

实验分为两大块。第一块是 WorldState-Bench 上的规则遵从评测：50 对反事实孪生样本，共 100 个片段用于单独评测，50 对用于联合评测。每个后端每个动作生成一个数秒片段，原生分辨率和帧率，片段 77-128 帧、15-24 fps、分辨率 592×336 到 1280×720；harness 输出 864×480、24 fps。共评估 11 个无需训练或微调的现成后端：4 个图像到视频生成器（Wan2.2、LongCat-Video、HunyuanVideo 1.5、MiniMax-H3，接收指令文本和初始帧），7 个交互式世界模型（LingBot-World v2、Echo-WM Flash、FantasyWorld、AlayaWorld、InSpatio-World、Astra、HY-WorldPlay，走各自标准动作接口）。评测用 Qwen3.8-27B-VL 作为 judge，在 3 fps 采样帧并连同最终帧一起判断。生成和评测使用 8 张 A800 80GB GPU。评测协议包含两种指标：ENDFOLLOW（单片段结局符合）、PAIRFOLLOW（样本对中两个结局同时符合）；对比条件包括仅规则提示（Rule-only prompting）和显式指定结局的提示（explicit ending instructions）。第二块是多回合能力验证：同一冻结权重下用 harness 支撑 11 到 29 回合的 episode，覆盖三个体裁（城镇探索、约会叙事、回合制决斗），各自有独立的状态转移和规则，每个体裁相当于在同一底层视频模型上定义的一个新的可执行游戏；另外还考察了空间触发器（spatial triggers），由规则程序吸收额外记账而渲染器不承担。

Q5: 发现了什么实验现象？

关键实验现象：(1) 仅规则提示下，效果最好的后端 MiniMax-H3 达到 66% ENDFOLLOW、32% PAIRFOLLOW，Wan2.2 为 65% 和 32%；联合准确率远低于单片段准确率，说明单个片段结局正确不意味着能同时满足两个隐藏状态下的规定结局——这正是“孪生坍缩”（twin collapse）现象的量化体现；(2) 对 MiniMax-H3，在提示中显式写明所需结局，PAIRFOLLOW 从 32% 提升到 64%（约翻倍），说明提示工程能部分补救，但远未解决；(3) 使用完整 harness、权重完全冻结，PAIRFOLLOW 达到 92%，即在各条件中最高，且被标记为 star 的 harness 在 ENDFOLLOW 和 PAIRFOLLOW 两个指标上都表现突出；(4) Astra、HY-WorldPlay 和 InSpatio-World 的表现存在明显不足（从 results 片段可见这 11 个模型被逐一排开，Astra、HY-WorldPlay、InSpatio-World 的结局片段并不一定意味着严格遵从）；(5) 附带评测显示各模型在 Consistency（8 项）和 Physical（2 项）上有细粒度得分分布，例如一致性分数从 65.1 到 89.9、物理分数从 60.5 到 71.2 不等（这些是检索到的表格残片，具体对应关系需回原文确认），说明不同后端在连续性和物理合理性上有明显差异；(6) 该配方在多回合（11-29 回合）和空间触发器等扩展场景中同样成立，规则程序而不是渲染器吸收新增簿记负担。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向：(1) 把 harness 扩展到更多视频后端和更大规模基准，验证解耦架构的通用性；(2) 研究反事实孪生对之外的更复杂规则空间：嵌套状态、随机性或部分可观测状态；(3) 探索语义状态与空间状态如何自动初始化或从视觉观察中推断，而不仅仅由外部指定；(4) 研究渲染失败策略（重试、复用锚帧、终止）对多回合稳定性的影响，以及失败恢复的规则一致性；(5) 将状态簿记从确定性规则扩展到学习型规则或概率规则，同时保持冻结渲染器的可验证性；(6) 把该配方与可编程引擎、学习游戏变量、重建 NPC 状态等现有显式状态方案结合，比较它们在更广泛机制上的成本与收益；(7) 研究指令编译质量与视频模型遵从之间的瓶颈：显式结局指令只达到 64%，说明从事件到指令的映射仍有信息损失，如何压缩指令或利用多模态引导值得深入；(8) 探索 3D 可编辑场景（clay model）与视频模型的空间对齐，空间引导在何种场景布局下会失效；(9) 扩展到真实世界交互或机器人任务，其中隐藏状态和持久规则同样关键。

Q7: 总结一下论文的主要内容

论文围绕“视频世界模型的交互必须尊重隐藏状态与持久规则”这一主张展开。作者指出，视频生成已能产生连贯逼真的动作条件视频，交互式世界模型也支持导航和动作条件生成，但“响应式视频”不等于“符合规则要求的结果”：同样的开锁动作在玩家持有钥匙与否时应产生不同结局，而初始画面可以完全一致。为此论文提出 Stateful Harness，把世界演化与视频渲染彻底分离：一个显式维护的语义账本加上一个可编辑 3D 场景提供的空间状态，在确定性规则下结算每个动作，把结算后的事件编译为指令，再用冻结的视频模型渲染成片段，片段又作为视觉观察反馈更新状态，循环进入下一回合。这一设计使规则执行完全落在可验证的程序侧，视频模型只负责像素渲染。为了严格评测规则遵从，作者构建 WorldState-Bench：50 对反事实孪生样本，共享初始帧和动作但隐藏状态不同，联合评测要求模型在同一规则下为两种隐藏状态分别产出规定结局。评测了 11 个现成后端（4 个图生视频、7 个交互式世界模型），用 Qwen3.8-27B-VL 以 3fps 采样帧加末帧做判断。结果：仅规则提示下最强后端 MiniMax-H3 的 PAIRFOLLOW 只有 32%，显式指定结局可提升到 64%，完整 harness 在权重完全冻结下达到 92%；单个片段正确率高不代表孪生对同时正确，说明存在“孪生坍缩”。同一配方还支持 11-29 回合的多回合 episode 和空间触发器，覆盖城镇探索、约会叙事、回合制决斗三个体裁，每个体裁都是同一冻结模型上的新可执行游戏。结论是：显式状态与可执行规则是控制有状态渲染交互的有效路径，规则程序而非渲染器承担状态簿记。论文最后附带 AI 使用声明：生成式 AI 仅用于写作辅助和辅助工具脚本，不用于研究构思。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体方向直接相关（权重 0.10）：框架本质是把规则执行当作外部可验证的簿记程序，把视频模型当作执行渲染器的工具，这种“外部状态 + 工具模型”的分工是智能体系统设计的典型范式。

## 基本信息

- 作者：Chan Yan Ki, Yanran Zhang, Wenzhao Zheng, Xiaofeng Wang, Zheng Zhu, Lei Chen, Jie Zhou, Jiwen Lu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-10-19
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.26811v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Introduction、Results、References、Conclusion 片段，并据此修正了摘要、方法与结果数字的表述；部分表格残片和算法细节证据不足，已在相应字段内标注需回原文确认。
