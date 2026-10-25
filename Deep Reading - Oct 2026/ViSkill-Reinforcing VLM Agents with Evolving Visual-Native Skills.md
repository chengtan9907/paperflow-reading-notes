---
user_id: "cheng tan"
paper_id: 15141
arxiv_id: "2610.12403v1"
title: "ViSkill: Reinforcing VLM Agents with Evolving Visual-Native Skills"
publish_date: "2026-10-08"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.12403v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.12403v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-12T12:58:30"
---
# ViSkill: Reinforcing VLM Agents with Evolving Visual-Native Skills

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：visual-native skill learning · vlm agent · reinforcement learning · skill library

## 一句话总结

ViSkill 提出一种视觉原生技能学习框架，将成功轨迹蒸馏为复合视觉技能卡片并直接注入 VLM 视觉上下文，通过技能检索同时指导推理与奖励塑形、再把成功轨迹在线蒸馏回技能库，形成技能积累与策略优化的闭环反馈，在 Sokoban、FrozenLake 和 PrimitiveSkill 上达到 0.89 的整体成功率（冷启动初始化后为 0.91），超越所有对比的闭源与开源基线。

## 摘要

> Skill-augmented agents improve sample efficiency by distilling successful trajectories into reusable strategies. Yet most existing approaches remain text-centric, linearizing spatial layouts and action-state correspondences into language that loses critical geometric structure. Recent efforts have begun incorporating visual evidence, but construct and update skills separately from policy optimization, leaving their mutual improvement underexplored. We propose ViSkill, a visual-native skill learning framework that encodes successful interactions as composite visual skill cards directly accessible to VLM agents. Retrieved skills guide both inference and reward shaping, while successful trajectories are distilled back into the library, forming a closed feedback loop in which skill accumulation and policy improvement reinforce each other. An optional cold-start mechanism further accelerates early-stage learning. Evaluated on Sokoban, FrozenLake, and PrimitiveSkill, ViSkill achieves an overall success rate of 0.89, rising to 0.91 with cold-start initialization, outperforming all evaluated proprietary and open-source baselines while converging faster than standard PPO. Our code is available at https://github.com/ZJU-REAL/ViSkill.

Q1: 这篇论文试图解决什么问题？

论文针对的核心问题是：VLM 智能体在复杂多轮交互环境中做强化学习时，成功策略只被隐式吸收进策略参数，导致智能体需要在多个 episode 中反复重新发现类似的空间解决方案，样本效率低下。此前的技能增强智能体虽能把成功轨迹蒸馏为可复用策略，但存在两个结构性缺陷：（1）表征层面：主流方法把空间布局、动作-状态对应关系线性化为自然语言，丢失几何结构，造成有损抽象（如图 1 所示，文本技能把空间关系压成线性描述，损失了精确的空间锚定信息）；（2）优化层面：技能库的构建与更新和策略优化彼此分离，技能改进与策略提升无法相互促进，形成不了正反馈。论文因此要回答：如何设计一种 VLM 原生可访问的视觉技能表征，并把技能学习与策略优化耦合进同一个闭环，使得技能积累和策略改进共同演进？此外还包括工程性问题：如何判断一个轨迹是否值得蒸馏为新技能（新颖性阈值）、如何判断检索到的技能是否值得采纳（检索阈值）、以及如何缓解早期技能库为空时的冷启动问题。

Q2: 有哪些相关研究？

相关工作按几条脉络组织：（1）RL 训练 VLM 智能体：以 PPO 为代表的强化学习（Schulman et al., 2017; Guo et al., 2025; Ahmadian et al., 2024）已成为训练 VLM 智能体与复杂多轮环境交互的主流范式（Zhai et al., 2024; Wang et al., 2026a）。（2）技能增强智能体：把交互轨迹蒸馏为自然语言指令或结构化工作流，组织为持久库用于检索复用（Shinn et al., 2023; Zhao et al., 2024; Wang et al., 2023）；近期工作进一步把这类库纳入强化学习（Xia et al., 2026）。（3）视觉证据的引入：最近有工作开始把视觉证据引入技能表征，但技能构建/更新与策略优化分离，二者相互改进未被探索，这正是 ViSkill 要填补的空白。（4）实验基线与环境：评估采用 VAGEN（Wang et al., 2026a）框架的三个视觉交互环境（Sokoban、FrozenLake、PrimitiveSkill），基线包括闭源模型 GPT-5、o3、o4-mini、GPT-4o、Gemini 2.5 Pro、Gemini 2.0、Claude 4.5 Sonnet、Claude 3.7 Sonnet，开源模型 Qwen2.5-VL 系列、VLM-R1-3B，以及技能方法 VAGEN 和 Atlas-VA。论文与这些工作的区别在于：视觉原生表征 + 技能检索与 RL 的闭环耦合。

Q3: 论文如何解决这个问题？

ViSkill 由三部分组成。整体流程：对每个任务 x～D，分为技能检索、技能引导交互、在线技能蒸馏三个阶段；完整轨迹记为 τ=(x, z, (o_t, a_t)_{t=1..T})，其中 z 是检索到的视觉技能。1）视觉原生技能库：技能库 B={z_k}，每个技能 z_k 由一条成功轨迹通过结构化渲染构建——保留原始视觉观测，叠加轨迹标注以澄清空间和流程进展，组织为统一视觉序列；策略从成功轨迹蒸馏出的策略描述 d_k 作为文本渲染进同一图像，捕捉技能的高层意图。检索机制：采用几何描述符（附录 A 给出细节）度量新轨迹与现有技能的相似度；技能 z_k* 只有在相关度 q_k* ≥ δ_r 时才被检索，否则策略无技能引导直接行动；检索到的复合技能卡片 C_k* 直接追加进 VLM 的视觉上下文。2）技能引导的奖励塑形：轨迹终止后环境给出二元成功指示 y(τ)，该指示同时用于策略优化的结果奖励、更新所检索技能的效用、以及决定轨迹是否够格蒸馏为新技能。技能对齐的失败轨迹会获得一定部分信用奖励，但该部分信用被约束为始终小于完成奖励，从而在给技能对齐的失败轨迹提供有界部分学分的同时不扭曲全局奖励排序（合理推断：这是为了在不鼓励'假装接近成功'的前提下提供中间学习信号）。3）在线蒸馏与闭环：成功轨迹蒸馏回技能库，技能效用和使用计数在终止后被更新；技能积累与策略优化互相促进。4）冷启动机制：可选机制，通过预置初始技能加速早期学习阶段。5）策略优化：在给定轨迹奖励后，用标准 RL 目标优化策略（摘要称收敛快于标准 PPO；具体损失函数细节论文片段未完整给出，需回原文确认）。

Q4: 论文做了哪些实验？

论文在三个视觉交互环境上评估，环境来自 VAGEN（Wang et al., 2026a），覆盖 2D 空间推理与 3D 具身操作：Sokoban（把所有箱子推到目标位置）、FrozenLake（导航到目标并避开洞）、PrimitiveSkill（包含 Place、Stack、Drawer、Align、Swap 等多个子任务）。对比基线包括：闭源模型 GPT-5、o3、o4-mini、GPT-4o、Gemini 2.5 Pro、Gemini 2.0、Claude 4.5 Sonnet、Claude 3.7 Sonnet；开源模型 Qwen2.5-VL-72B/7B/3B、VLM-R1-3B；技能方法 VAGEN（0.80）与 Atlas-VA（0.87）。主结果表（Table 1）报告各环境成功率。ViSkill 整体成功率 0.89，ViSkill + Cold-Start 为 0.91。消融实验：技能库消融（有无技能库）、联合策略优化消融（带联合策略优化 vs 不带）；分辨率/视觉细节缩放实验（对技能卡片图像的高度和宽度应用缩放因子，生成更低分辨率表示以消耗更少 token，考察成功率与 token 效率的权衡）；新颖性阈值 δ_d 消融（图 4(b) 和图 5，低阈值 0.85 过于宽松会把相似轨迹大量收入库中，高阈值则过严）。具体各环境数值：ViSkill 在 Sokoban 0.82、FrozenLake 0.84、PrimitiveSkill 各子任务全部 1.00；ViSkill + Cold-Start 在 Sokoban 0.88、FrozenLake 0.85、PrimitiveSkill 全 1.00。收敛速度对比标准 PPO（摘要声明更快，具体曲线需回原文）。训练基础设施细节（如采样数、PPO 超参、渲染分辨率等）论文片段未提供。

Q5: 发现了什么实验现象？

主实验现象：（1）ViSkill 在全部三个环境上一致超越所有闭源与开源基线，包括最强的 Atlas-VA（0.87→0.89）和 VAGEN（0.80→0.89），说明视觉原生技能闭环在跨环境泛化上优于既有技能方法；（2）冷启动初始化带来进一步增益（0.89→0.91），尤其 Sokoban 从 0.82 提到 0.88，说明早期技能库稀疏时冷启动机制能有效加速学习；（3）在 PrimitiveSkill 上 ViSkill 达到各项 1.00 的完美平均成功率，追平 Atlas-VA、比 VAGEN 高 12 个百分点（合理推断：'高 12 个百分点'对应 VAGEN 的 0.88→1.00），而像 GPT-5、o3 这类强闭源模型在 Drawer 等子任务上为 0.00，显示技能库对具身操作子任务的稳定化作用；（4）消融显示：给基础模型加上任一技能库都能带来一致增益，但'技能库 + 联合策略优化'同时具备时增益最大（表/图碎片显示 0.2 vs 0.14 的成功率对比，具体配置对应需回原文确认），证实检索引导与 RL 优化对正反馈环路各自有贡献；（5）分辨率/视觉细节缩放实验：成功率随缩放因子增大而上升，说明视觉技能卡片的分辨率（即视觉保真度/token 消耗）与成功率存在 trade-off，低分辨率省 token 但损失信息；（6）新颖性阈值 δ_d 的敏感性：低阈值（0.85）过于宽松，会把相似轨迹大量吸入技能库（推测导致库冗余、检索噪声上升）；高阈值则过严（推测导致技能库增长过慢），存在最优区间；（7）负结果/失败模式：强闭源模型（GPT-4o、Gemini 2.0、Claude 3.7 Sonnet 等）在若干子任务上成功率极低甚至 0.00（如 Drawer 为 0.00、Swap 为 0.00），而任务本身是可解的，说明纯文本推理在这些空间任务上系统性失效；（8）小模型差距：Qwen2.5-VL-7B/3B、VLM-R1-3B 几乎全部失败（整体 0.09-0.14），暗示技能卡片方法的收益可能依赖足够强的视觉底座。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向：（1）更复杂与开放环境的验证：论文自陈目前只在 VAGEN 框架支持的环境上评估，视觉技能卡片在更复杂或开放式设置中是否持续有效需要验证，例如长 horizon 任务、部分可观测环境、动态环境；（2）技能引导奖励的失效边界：当前奖励塑形假设有效的部分进展会到达检索技能所表示的状态；当合法解需要穿越不相交状态区域时引导可能失效，需要设计不依赖状态可达性的中间奖励，或学习状态-技能价值函数来放松该假设；（3）跨任务/跨环境技能迁移：技能卡片能否在任务分布漂移时继续有效、如何对技能库做去重与压缩、技能的新颖性度量是否可以学习而非手工几何描述符；（4）token 效率优化：分辨率缩放实验已显示视觉细节与 token 消耗的权衡，可探索自适应分辨率、技能卡片稀疏化或视觉 token 压缩；（5）冷启动的扩展：当前冷启动是可选机制，可研究自动生成初始技能而非人工预置，或让冷启动技能随学习动态更新；（6）技能库的规模扩展与检索效率：随着技能数量增长，检索延迟与噪声如何控制，几何描述符能否换成学习式检索器；（7）与其他 RL 范式的结合：与 GRPO 等无 critic 方法、离线 RL、多智能体设置结合，或把技能卡片用作 critic 输入而非仅 policy 输入；（8）安全性：论文结尾提及技能库能约束低质量行为但不等同于开放场景安全验证，可探索安全约束技能卡片、红队测试视觉技能库的对抗鲁棒性。

Q7: 总结一下论文的主要内容

论文的论证主线：RL 训练 VLM 智能体已成为主流，但成功策略只隐式存在于策略参数中，智能体需要跨 episode 重新发现类似空间解，样本效率低；成功轨迹本身已编码可复用的程序性知识（绕障碍导航、多步物体操作协调），因此应显式保留并复用成功经验。但现有技能增强方法以文本为中心，把空间布局和动作-状态对应线性化为语言，是有损抽象；引入视觉证据的近期工作又把技能构建/更新与策略优化分离，未利用二者相互促进的潜力。ViSkill 的技术主线：把每条成功轨迹通过结构化渲染构建为复合视觉技能卡片（原始视觉观测 + 轨迹标注 + 策略描述的文本渲染在同一图像中，保留空间与程序结构），存入视觉原生技能库；推理时用几何描述符计算相似度，只有相关度超过阈值 δ_r 才检索技能卡片，直接追加进 VLM 视觉上下文；交互终止后二元成功指示同时驱动结果奖励、技能效用更新和蒸馏资格判定；技能对齐的失败轨迹获得有界部分信用（始终小于完成奖励），不扭曲全局奖励排序；成功轨迹在线蒸馏回库，形成技能积累与策略优化互相强化的闭环；可选冷启动机制加速早期学习；在给定轨迹奖励后用标准 RL 目标优化策略（收敛快于标准 PPO）。实验主线：在 Sokoban、FrozenLake、PrimitiveSkill（VAGEN 框架）上对比 8 个闭源模型、5 个开源模型/方法以及 VAGEN、Atlas-VA 两个技能方法；ViSkill 整体成功率 0.89、冷启动 0.91，全面最优；消融证实视觉表征、策略引导（检索）、联合优化对正反馈环路各有贡献；分辨率缩放与新颖性阈值实验揭示视觉保真度- token 权衡与库准入控制的敏感性。结论：通过保留空间几何结构并让技能积累与策略优化共同演进，ViSkill 弥合了文本中心技能方法的表征鸿沟，在三个基准上一致超越闭源与开源基线。局限：仅限 VAGEN 框架环境、技能引导奖励对不相交状态空间的解可能提供有限引导、技能库对低质量行为的约束不能替代开放场景安全验证。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与'agent'方向直接相关：论文核心是让 VLM 智能体通过视觉原生技能库 + 强化学习闭环提升多轮交互环境中的空间决策能力

## 基本信息

- 作者：Hongxing Li, Dingming Li, Yixin Li, Yong Du, Wenqi Zhang, Weiming Lu, Jun Xiao, Yueting Zhuang, Yongliang Shen
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-08
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.12403v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索证据（Introduction、Method、Results、Conclusion 共 9 个命中片段）并结合论文元数据与启发式草稿；表 1 数值、部分消融配置与超参细节在检索片段中不完整，已按证据强度标注合理推断或推测，并明确需回原文核对之处。
