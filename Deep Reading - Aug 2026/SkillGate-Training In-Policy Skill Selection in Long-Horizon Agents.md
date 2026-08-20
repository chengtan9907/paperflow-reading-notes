---
user_id: "cheng tan"
paper_id: 8657
arxiv_id: "2608.18852"
title: "SkillGate: Training In-Policy Skill Selection in Long-Horizon Agents"
publish_date: "2026-08-20"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.18852.pdf"
pdf_url: "https://arxiv.org/pdf/2608.18852"
abs_url: "https://arxiv.org/abs/2608.18852"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-20T11:29:45"
---
# SkillGate: Training In-Policy Skill Selection in Long-Horizon Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：skill selection · credit assignment · long-horizon agent · reinforcement learning

## 一句话总结

SkillGate 指出长程智能体技能选择的默认训练方式——outcome-rewarded RL——因“selector credit starvation”（选择器信用匮乏）而结构性失败，并提出将 token support 划分为两条不相交信用通道（结果信用只达执行 token，action-local advantage 只达技能命名 token）的训练方法，在五个 agentic benchmark 和 16 个候选技能的设置下将 9B 策略的 trial 成功率从 40.8% 提升到 53.2%。

## 摘要

> Agent frameworks increasingly package procedural knowledge as skills: instruction files an agent reads on demand, while public libraries now hold thousands of them. Which skill to read has thus become a decision the policy itself makes in the middle of an episode, yet no existing signal trains it. We show that the default remedy, outcome-rewarded RL over the candidate slate, cannot teach it, for a structural reason we identify and name selector credit starvation: under a broadcast, sequence-level advantage, the few tokens that name the chosen skill carry a vanishing share of the loss, and the credit they inherit is increasingly wrong-signed as trajectories lengthen. A correct choice is punished whenever the execution after it fails, even though the choice itself is among the most valuable decisions in the trajectory. Auditing a completed run's own training artifacts confirms all three properties, each worsening monotonically with horizon. SkillGate removes the failure by construction: it partitions the token support into two disjoint credit channels, outcome credit reaching only execution tokens, and a separate action-local advantage reaching exactly the skill-naming tokens, positive only when a trajectory's single read is the correct one. On five agentic benchmarks under a 16-candidate slate, SkillGate lifts a 9B policy from 40.8% to 53.2% trial success, well ahead of the identical budget spent on outcome reward alone, while cutting exposure to misleading candidates by two thirds and reading fewer skills.

Q1: 这篇论文试图解决什么问题？

1. 核心问题：长程智能体中，技能选择（mid-episode 决定读取哪条技能）是一个策略自身做出的决策，但没有训练信号专门优化该决策。现有 agent 框架把技能封装为指令文件，公共库含数千条技能，于是“读哪个技能”变成 episode 中途的关键决策，而默认的 RL 训练信号（任务结果）无法正确地把信用分配给技能命名 token。
2. 结构性失败机制（selector credit starvation）：在 outcome-rewarded RL 中，advantage 通常是序列级（sequence-level）或广播式（broadcast）的，即同一 advantage 广播给轨迹内所有 token；技能命名 token 只是轨迹中极少一部分，在损失函数中权重极小；同时，由于 credit 按整条轨迹的结果计算，命名 token 继承的符号往往不正确——如果选择正确但后续执行失败，选择 token 也被惩罚；如果选择错误但执行侥幸成功，选择 token 也被奖励。
3. 与 horizon 的关系：轨迹越长，选择 token 占全部 token 的比例越小（稀缺性加剧），错误符号的 credit 占比越高，因而三个问题都随 horizon 单调恶化。
4. 一个明显的替代方案（在候选表上做 outcome-rewarded RL）被论文证明无法解决该问题，因此需要新的训练机制。
5. 论文还指出，单纯提升技能覆盖率（coverage）不够，因为此类系统已经失败在“选择”（selection）环节，而不是“覆盖”（coverage）环节。

Q2: 有哪些相关研究？

1. Agent 技能库与技能使用：现代 agent 框架把技能包装为指令文件，按需读取；公共技能库已有数千条技能，技能选择因此成为 episode 中途的关键决策。
2. Outcome-rewarded RL 用于技能选择：已有工作（Shao et al., 2024; Schulman et al., 2017; DeepSeek-AI, 2025）直接把候选技能呈现给策略，用任务结果做 RL 奖励，论文表明这类方法在长程任务中失败。
3. 联合优化技能构建与使用：SkillRL (Xia et al., 2026)、Skill1 (Shi et al., 2026)、SkillRise (Yao et al., 2026) 属于在任务奖励下联合优化技能构建与使用的路线；论文指出这些结果表明覆盖率不是关键瓶颈，选择才是。
4. Token-level credit assignment：论文在 3.1 节介绍了 token-level credit assignment 的相关工作；SkillGate 把轨迹的训练 token 沿着 read-action spans 分成 selection tokens（写技能名的 token）和其余执行 token，并分别给 credit。
5. RL 优化器：PPO（Schulman et al., 2017）等策略梯度方法；DeepSeek-AI (2025) 等大规模 RL 训练。
6. 其他相关工作（从 References 片段可见）：包括关于 agentic harness 中技能使用的综述/论文、关于 action bottleneck 的 agentic 工作（Langzhou He et al.，Resolving action bottleneck: Agentic...）等；由于检索证据只有 References 残片，具体细节需回原文确认。

Q3: 论文如何解决这个问题？

SkillGate 的核心思想是：在同一个策略梯度更新中，把 token support 分成两条不相交的信用通道。具体包括：(i) task channel（任务通道）：结果信用只到达执行 token——即真正执行任务、产生中间动作和最终结果的那些 token；这条通道保留 outcome-rewarded RL 的执行学习能力。(ii) selection channel（选择通道）：一条独立的 action-local advantage 精确到达技能命名 token(s)，且只有在该轨迹的单次读取是正确的时候才为正；也就是说，选择正确时给命名 token 正信号，选择错误时给负信号，但信号不再被后续执行结果污染。SkillGate 训练单一策略同时学好两件事：选择正确的技能，以及用读到的内容执行任务。两条 credit channel 不触碰同一个 token——执行 token 接收结果 credit，选择 token 接收 action-local advantage。这样从构造上消除 selector credit starvation：技能命名 token 不再因后续执行失败而被错误惩罚；同时仍保留 outcome credit 来训练执行。论文还给出了选择 token 的严格定义：$\bigcup_{a\in A(\tau)}I(a)$，即轨迹中所有写技能名的 token 集合（read-action spans 内的命名 token）。

Q4: 论文做了哪些实验？

1. 训练设置：单策略参数规模 9B；五个 agentic benchmark；技能候选表大小 16（16-candidate slate）。
2. 对比基线：共享 SFT 初始化；受控的 outcome-only RL 运行（把同样预算花在 outcome reward 上）；监督式（supervised）方法；基于偏好（preference-based）的方法；Skill1 替代方案。
3. 主结果（Table 1）：SkillGate 在 9B 规模下任务性能最强，trial 成功率从 40.8% 提升到 53.2%，明显超过 outcome-only RL。
4. Oracle-only 干预（Table 3）：只把正确技能给冻结的 SFT executor，任务成功率提升约 11 个点，证明正确技能访问对任务成功有实质性影响。
5. 附加指标：暴露于误导性候选的比例（exposure to misleading candidates）降低三分之二；读取技能数量更少。
6. 审计实验：对一次完整运行的训练制品做审计，确认 selector credit starvation 的三个性质（稀缺性、符号错误、价值）都随 horizon 单调恶化。

Q5: 发现了什么实验现象？

1. 默认的 outcome-rewarded RL 在长程技能选择任务中失败，且失败原因可被结构化诊断：命名 token 承担损失份额极小、继承的信用符号错误、正确选择却常被惩罚。
2. 三个诊断属性都随 horizon 单调恶化：轨迹越长，稀缺性越严重、credit 符号越可能错误、正确选择的价值越大（因此损失也越大）。
3. Oracle-only 实验：如果直接把正确技能提供给冻结的 SFT executor，任务成功率提升约 11 个点，说明技能访问正确性对任务成功有显著影响，也说明执行器本身够用，瓶颈在选择。
4. SkillGate 不仅提升平均成功率，还减少了对误导性候选的暴露（降低三分之二），并读了更少的技能；这说明训练信号正确后，策略既能选择更好，也能更早/更少读取。
5. 在 16-candidate slate 上，9B 策略成功率从 40.8% 到 53.2%（+12.4 个点），明显超过同样预算的 outcome-only RL；说明 credit 分配方式比预算/容量更关键。
6. 推测：这种 credit 通道分离的收益可能随 horizon 增长而增大（因为 selector credit starvation 随 horizon 恶化），但检索证据未直接给出 scaling trend 曲线，需回原文确认。

Q6: 有什么可以进一步探索的点？

1. 把 SkillGate 扩展到更大的策略规模（如 30B/70B 甚至更大）和更长 horizon，验证 credit 分离收益是否随规模/horizon 持续放大。
2. 在更大候选表（如数百/数千条技能）上测试，因为公共技能库已达数千条，selection 困难更大。
3. 与联合优化技能构建与使用的路线（SkillRL、Skill1、SkillRise）结合：SkillGate 解决“选择”，那些方法解决“构建+使用”，二者可能互补。
4. 探索 action-local advantage 的更好估计器：目前只在单次读取时才为正，如果轨迹中有多次读取，如何分配 credit 值得研究。
5. 更细粒度：把 SkillGate 的选择通道扩展到技能内步骤选择、工具选择、API 选择等其他 mid-episode 决策。
6. 理论上刻画 selector credit starvation 的边界条件：在什么 advantage 估计器（如 GAE、temporal-difference 风格）下问题会减轻或消失。
7. 在真实/更大规模 agentic harness 和在线部署环境中验证，尤其是技能库动态变化、技能版本更新、开放世界任务。
8. 训练制品审计方法本身可推广：用稀缺性、符号错误、价值三个诊断指标来审计其他 mid-episode 决策（如记忆读取、上下文检索）的 credit 分配情况。

Q7: 总结一下论文的主要内容

本论文研究长程智能体（long-horizon agent）中“技能选择”（skill selection）这一决策的训练问题。在现代 agent 框架中，技能通常被封装为指令文件，策略在 episode 中途按需读取；公共技能库已含数千条技能。因此“读哪个技能”成为策略在 episode 中途做出的决策，但现有训练信号并未专门训练该决策。论文指出默认修复手段——直接在候选技能表上做 outcome-rewarded RL——存在结构性失败，并命名该失败为“selector credit starvation”（选择器信用匮乏）：在广播式、序列级 advantage 下，命名所选技能的少数 token 只承担损失的极小份额，而且随着轨迹变长，它们继承的信用越来越可能带有错误符号。正确选择在后续执行失败时会被惩罚，尽管该选择本身就是轨迹中最有价值的决策之一。作者对一次完整运行的自有训练制品（training artifacts）进行审计，确认了三个属性都随 horizon 单调恶化：1) 稀缺性（scarcity）：技能命名 token 只占监督信号极小比例；2) 符号错误（wrong-signed credit）：轨迹越长，正确选择越常被惩罚而非奖励；3) 价值（value）：在匹配的 prompt 组内，正确读取仍值 +11.2 个点的任务成功率。论文提出 SkillGate，通过构造将 token support 划分为两条不相交的信用通道：结果信用（outcome credit）只到达执行 token，而独立的 action-local advantage 精确到达技能命名 token，且仅当一条轨迹中的单次读取是正确时为正。在五个 agentic benchmark、16 个候选技能的表上，SkillGate 将 9B 策略的 trial 成功率从 40.8% 提升到 53.2%，明显超过把同样预算花在纯 outcome reward 上的基线，同时将暴露于误导性候选的比例降低了三分之二，并读了更少的技能。论文还讨论了 token-level credit assignment 的相关工作、SkillRL/Skill1/SkillRise 等联合优化技能构建与使用的路线，说明覆盖率（coverage）不是问题，选择（selection）才是瓶颈。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 agent 方向直接相关：论文研究长程智能体的技能选择训练问题，属于 agent 决策与 RL 训练的交界处。

## 基本信息

- 作者：Qingyao Li, Wenxiang Jiao, Shuai Shao, Kangning Zhang, Yuan Lu, Yi Guo, Weiwen Liu, Weinan Zhang, Yong Yu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-20
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.18852`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 provided heuristic_draft 与 retrieved_evidence 中命中的摘要、Introduction、Method、Main Results、Conclusion 和 References 片段；由于未提供论文全文 PDF，部分实验细节和结论表述是基于片段的中文归纳，标注了需要回原文确认的地方。
