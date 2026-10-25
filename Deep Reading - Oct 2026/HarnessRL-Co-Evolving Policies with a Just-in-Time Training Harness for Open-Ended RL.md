---
user_id: "cheng tan"
paper_id: 16612
arxiv_id: "2610.26426v1"
title: "HarnessRL: Co-Evolving Policies with a Just-in-Time Training Harness for Open-Ended RL"
institution: "阿里巴巴集团（合理推断：论文致谢标注“This work was supported by Alibaba Research Intern Program”；arXiv 元数据未提供机构字段，具体部门/实验室未知）。"
publish_date: "2026-10-19"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.26426v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.26426v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-20T13:09:55"
---
# HarnessRL: Co-Evolving Policies with a Just-in-Time Training Harness for Open-Ended RL

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：open-ended rl · rubric-based reinforcement learning · co-evolving training harness · reward failure attribution

## 一句话总结

HarnessRL 提出一种与策略共同演化的训练期脚手架（training-time harness），通过任务采样、rollout 引导与 rubric 标准三个接口的联合更新，解决开放域生成中基于 rubric 的奖励失效问题，并在自建的 Rubric-Mix-10k 五领域套件上以约一半的训练步数超过最强 baseline 2.8-3.1 分。

## 摘要

> 基于 rubric 的强化学习用自然语言标准作为开放域生成任务的奖励，但训练过程中这些奖励会失效。论文识别出两种失败模式：非判别性奖励（评分坍缩到全高或全低，源于探索不足或任务难度与策略能力不匹配）和虚假奖励（高分并不对应更好回答）。二者的共同根源是任务采样、探索和评估配置在策略演化过程中保持不变。为此作者提出 HarnessRL：一个训练期 harness，通过任务采样、rollout 引导和 rubric 标准三个接口与策略共同演化；它维护训练期记忆（任务内历史与跨任务证据），由 harness evolver 将每次奖励失败归因到能够修复它的接口，再用 proposer 与 just-in-time critic 循环保证候选更新先在当前冻结策略上测量效果、满足条件才被接受，被拒候选返回修订。论文还构建了 Rubric-Mix-10k，一个覆盖医疗、科学、写作、角色扮演、指令跟随五个领域的训练与评测套件。在 Rubric-Mix-10k 上，HarnessRL 在 4B 与 9B 两种模型规模下比最强 baseline 高 2.8-3.1 分，且到达最佳 checkpoint 的训练步数几乎减半。

Q1: 这篇论文试图解决什么问题？

1. 核心问题：基于 rubric 的强化学习（rubric-based RL）用自然语言标准作为开放域生成（open-ended generation）的奖励信号，但这类奖励在训练推进中会逐渐失效，使策略优化失去有效信号。论文将失效归纳为两种可操作的失败模式：
2. 失败模式一：非判别性奖励（non-discriminative rewards）。奖励分数坍缩为一致偏低或一致偏高：偏低源于探索不足，偏高源于任务难度与策略当前能力不匹配（任务过易，导致几乎任何回答都能得分）。此时奖励无法区分回答质量，梯度信号失去信息量。
3. 失败模式二：虚假奖励（spurious rewards）。分数与真实质量排序脱钩，高分回答并不比低分回答更好。这可能源于策略对固定 rubric 的 reward hacking，或评估标准本身已过时。
4. 根本原因：两种失败指向同一个结构性缺陷——训练配置中的任务采样分布、探索/rollout 机制和评估（rubric 标准）在策略演化过程中固定不变，形成“静态配置 vs 动态策略”的错配。
5. 现有方法的缺口：已有工作分别演化 rubric（EvoRubric、AMARIS 等）或任务分布（SEC、CurES 等课程学习），但通常只针对单一组件，缺少将奖励失败系统归因到具体组件的机制；rubric 更新往往直接作用于正在训练的策略，而不是先在冻结策略上验证候选更新是否有效。HarnessRL 的研究问题因此是：一个覆盖三接口、带失败归因与即时验证的训练期整体框架，能否带来更优的奖励质量、更稳的训练动态和更高的样本效率。

Q2: 有哪些相关研究？

1. RLVR 与 LLM 后训练：可验证奖励 RL（Lightman et al., 2024; Lambert et al., 2025; Guo et al., 2025）已成为 LLM 后训练的标准配方（Ouyang et al., 2022; Bai et al., 2022; Grattafiori et al., 2024; Yang et al., 2025; DeepSeek-AI, 2026），在数学推理（Shao et al., 2024; OpenAI et al., 2024; Wang et al., 2026d）与代码生成等任务上有显著提升；但开放域生成缺少标准答案，需要 rubric 类自然语言奖励。
2. 基于 rubric 的 RL：Gunjal et al., 2026; Huang et al., 2025; Viswanathan et al., 2025; Liu et al., 2026; Li et al., 2026b; Chen et al., 2026b 等用自然语言标准作为开放域奖励；固定 rubric 下的 reward hacking（Mahmoud et al., 2026; Wang et al., 2026e）是推动 rubric 演化的直接动机。
3. 演化路线一：从策略回答重写 rubric。EvoRubric（Guan et al., 2026）剪枝并参照策略回答重新生成开放域标准；AMARIS（Wu et al., 2026a）通过记忆增强循环修订 rubric；Wang et al., 2026a;b 与 Chen et al., 2026c 同属该方向。
4. 演化路线二：与 judge/策略共同演化或课程化排序。Rubric-ARM（Xu et al., 2026）在 rubric 生成器与 judge 之间交替 RL；OpenRS（Jia et al., 2026）为每个回答对构造 rubric；Li et al., 2026a; Jia et al., 2026; Krishnan et al., 2026 等把 rubric 组织成课程。
5. 任务分布自适应：基于优先级、难度或预期学习增益的任务采样（Schaul et al., 2016; Gao et al., 2026; Sundaram et al., 2026; Yu et al., 2026c; Zeng et al., 2026）；SEC（Chen et al., 2026d）让课程随策略演化。检索到的参考文献片段还给出两个背景入口：CurES（基于梯度分析的推理 LLM 课程学习）与 SOTOPIA（语言智能体社会智能交互评测），分别对应课程学习与智能体评测线。
6. 与 HarnessRL 的差异：HarnessRL 把任务采样、rollout 引导、rubric 标准三接口放进同一个训练期 harness，用“失败归因 + 冻结策略上的 JIT 验证”统一处理，而不是只演化单一组件。

Q3: 论文如何解决这个问题？

1. 总体框架：HarnessRL 在策略训练循环之外维护一个训练期 harness，由三个接口组成——任务采样（task sampling）、rollout 引导（rollout guidance）、rubric 标准（rubric criteria）。每轮先从任务分布采样一批任务（检索证据显示每步 512 个任务），策略生成 rollout，固定 judge 按当前 rubric 打分；harness evolver 根据打分结果生成三接口的候选更新，交给 just-in-time critic 验证后落地。
2. 训练期记忆：harness 维护训练期记忆，提供两类证据：within-task history（同一任务内的历史分数与归因记录）和 cross-task evidence（跨任务证据），作为归因与更新的依据。
3. 归因机制：harness evolver 对每个奖励失败做归因，判断该失败应由哪个接口修复——任务采样（任务太难/太易或与能力不匹配）、rollout 引导（探索不足，需要脚手架或特权信息）、还是 rubric 标准（标准非判别性、饱和或被 hack）。检索证据显示 evolver 对每次采样独立分析，并追踪每个 rollout 的回报分数（R scores）作为归因输入。
4. Proposer 与 JIT critic 循环：proposer 为被归因的接口生成候选更新；critic 要求候选更新先作用于当前冻结策略（frozen policy）、测量实际效果后才接受，被拒绝的候选返回 proposer 修订。critic 的接受条件至少包含“(i) 采样类更新……”（证据片段截断，完整条件需回原文确认）。该设计避免未经验证的 harness 改动干扰正在优化中的策略。
5. 三接口更新逻辑（以任务采样为例）：采样接口维护任务权重 pt；当某任务因难度/能力不匹配被归因时，降低其任务权重，并把权重重新分配到其他任务，缩放因子由当前分组平均奖励决定；更新保持任务的期望贡献不变（检索证据：权重降低造成的贡献缺口被重分配给其他任务）。rubric 接口的更新涉及标准权重调整甚至新增（案例中把 coupling-chemistry 标准降到 +1、新增 filter-grade 标准 +2）。
6. 闭环：更新后的 harness 进入下一轮——新任务采样 + 引导 rollout + 新 rubric 打分，实现 harness 与策略的共同演化；论文强调这是“训练配置随策略演化”而非单一组件修补。

Q4: 论文做了哪些实验？

1. 数据集 Rubric-Mix-10k：从五个公开 rubric 标注源构造五领域套件——医疗（HealthBench、RaR 医疗切片、RubricHub 医疗切片）、科学（ResearchQA、RaR 与 RubricHub 科学切片）、写作（LongWriter-Zero）、角色扮演与指令跟随（RubricHub 的 chat 和 instruction 切片）。过滤不合格任务与过长 prompt 后按固定配额抽取：8,600 个训练任务（带加权标准种子 rubric）、每领域 250 个 held-out 任务（与训练集不重叠）、每领域 1 个 OOD benchmark。训练集共 110,606 条标准，平均每任务 12.9 条（中位数 10）；prompt 平均 118 Qwen tokens（中位数 54），标准平均 35.2 tokens；负权重标准占 10.7%、出现在 48.6% 的任务中；3,008 个训练任务（35.0%）附带参考答案（HealthBench 为医生撰写理想回复、RaR 为参考答案、RubricHub 为六样本中最高分样本），平均 420 tokens，领域均值从医疗 322 到角色扮演 1,205；ResearchQA 与 LongWriter-Zero 不提供参考答案；写作任务指定目标长度区间。
2. 模型与 baseline：策略为 Qwen3.5 系列 4B 与 9B 两种规模（4B 型号由摘要“4B and 9B model scales”与结果中 Qwen3.5-9B 合理推断为 Qwen3.5-4B）。Baseline 包括 GRPO、AMARIS++（AMARIS 的增强版）、SkillRL、RuscaRL 等（完整清单需回原文确认）。
3. 评测协议：分别报告 in-domain（held-out）与 OOD 分数；设置 matched-compute 对比以排除每步 harness 开销的影响；围绕四个研究问题（RQ1-RQ4）组织实验：整体性能、训练效率、组件（critic/记忆）贡献、harness 在单个任务上的行为（case study）。
4. 训练动态与组件分析：统计失败类别占比（failure-class proportions）、奖励与 reward oracle 的一致性（reward-oracle agreement）、rubric 反转更新（reversed rubric updates）比例；对 critic 与记忆做消融，观察去掉 critic 后反转率随时间的变化。

Q5: 发现了什么实验现象？

1. 主结果：在 Rubric-Mix-10k 上，HarnessRL 在 4B 与 9B 两种规模均超过最强 baseline——in-domain 提升 2.8-3.1 分；OOD 最佳 checkpoint 领先 AMARIS++ 约 +2.8 pp。
2. 效率与早熟峰值：HarnessRL 到达最佳 OOD checkpoint 的训练步数约为 GRPO 的一半：9B 规模下 HarnessRL 在 step 250 达到峰值、AMARIS++ 在 325、GRPO 在 475；4B 规模下峰值步数顺序一致，各 baseline 峰值落在 350/400/425 附近（片段截断，具体 baseline 映射需回原文）。论文将更早的峰值归因于 harness 修正了（片段截断，推测为修正了奖励退化或信号失效）。在集群时间上，HarnessRL 到达最佳 OOD checkpoint 所需 cluster hours 少于任何 baseline。
3. 计算匹配下的结论不变：每步 harness 开销并未削弱 OOD 领先——matched-compute 下 OOD 领先 AMARIS++ 为 +2.9 pp，最佳 checkpoint 下为 +2.8 pp；某规模下 OOD 指标在 step 250 达到 49.8（具体指标名因截断需回原文确认）。
4. 训练动态：与 GRPO 相比，HarnessRL 的失败类别占比更低、奖励与 reward oracle 的一致性更高；rubric 反转更新保持罕见。组件消融：去掉 critic 后，反转率虽仍低于“无 critic+无记忆”配置的 22.3%，但从第一个窗口的 6.8% 逐渐升到最后一个窗口的 14.1%，说明 JIT critic 的即时验证是抑制 rubric 漂移的关键（该段对比的精确配置需回原文确认）。
5. 单任务行为（RQ4 案例）：在一道化学任务上，harness 将两个 coupling-chemistry 标准权重都降到 +1，并新增一个 filter-grade 标准（权重 +2）；到 step 440，归因器判定 hydroxyl-count 标准已饱和、其余标准一致同意，采样更新随之把任务权重降到 γ2。这展示了“标准饱和→归因→采样降权→新增/调整标准”的完整闭环。
6. 负结果与张力：片段证据未见明显负结果；值得注意的张力是 harness 每步引入额外推理/验证开销（per-step overhead），但 matched-compute 实验表明它没有损害 OOD 领先优势。

Q6: 有什么可以进一步探索的点？

1. 接口扩展：将 reward model/judge 本身、探索策略（温度调度、自一致性采样）、甚至优化超参纳入 harness 演化，形成更通用的“训练配置自演化”框架。
2. 归因深化：当前 evolver 按 rollout 独立归因；可探索跨 rollout 聚合归因、基于反事实干预的归因（“若更换采样/标准，失败是否消失”），以及带置信度的归因路由。
3. JIT critic 的理论化：critic 在冻结策略上验证候选更新的接受条件、验证采样量与验证预算分配目前缺乏理论刻画；可研究验证误差对最终性能的影响及最优更新频率。
4. 数据与领域扩展：Rubric-Mix-10k 覆盖五个英文文本开放域；可扩展到代码生成、多模态生成、工具使用与智能体任务；跨数据集迁移和 rubric 自动构建也是方向。
5. 规模与长程训练：更大模型（30B+/MoE）与更长训练窗口下 harness 的稳定性、记忆压缩与遗忘机制需要验证。
6. 与可验证奖励的混合：将 HarnessRL 的演化思路与 RLVR 结合，处理“部分可验证 + 部分主观”的复合任务；演化出的标准也可作为数据筛选与蒸馏信号。
7. 应用侧（贴合 AI for Science 与智能体画像）：把医疗/科学领域的标准演化迁移到研究假设生成、实验规划、多步科学推理等更长链条任务，并与智能体的反思-验证循环结合。

Q7: 总结一下论文的主要内容

1. 出发点：基于 rubric 的 RL 是开放域生成后训练的常用方案，用自然语言标准作为奖励，但训练中奖励会失效。论文把失效拆成两类可操作的失败模式——非判别性奖励（分数坍缩）与虚假奖励（分数与质量脱钩）——并论证其共同根源是训练配置（任务采样、探索、评估）在策略演化中固定不变，形成“静态配置 vs 动态策略”的错配。
2. 方法主线：作者提出 HarnessRL，一个训练期 harness，与策略同步演化三个接口——任务采样、rollout 引导、rubric 标准。harness 维护训练期记忆（任务内历史与跨任务证据）；harness evolver 对每个奖励失败归因，选择“哪个接口的更新能修复这个失败”；proposer 生成候选更新，just-in-time critic 要求候选先作用于当前冻结策略、测得效果后才接受，被拒候选返回修订。这一“先验证后采纳”的机制是核心设计：harness 更新不会盲目作用于优化中的策略。
3. 数据与评测主线：论文构造 Rubric-Mix-10k——从 HealthBench、RaR、RubricHub、ResearchQA、LongWriter-Zero 抽取 8,600 训练任务、每域 250 held-out 任务与 1 个 OOD benchmark，覆盖医疗、科学、写作、角色扮演、指令跟随五域；共 110,606 条加权标准，35% 任务带参考答案，负权重标准占 10.7%。评测在 Qwen3.5-4B/9B 上进行，对比 GRPO、AMARIS++、SkillRL、RuscaRL 等 baseline，围绕 RQ1-RQ4（性能、效率、组件贡献、单任务行为）展开。
4. 结果主线：in-domain 超最强 baseline 2.8-3.1 分，OOD 最佳 checkpoint 领先 AMARIS++ +2.8 pp（matched-compute 下 +2.9 pp）；训练效率上，9B 下 HarnessRL 250 步到峰、GRPO 需 475 步，近乎减半，cluster hours 最少。训练动态上，失败类别占比更低、oracle 一致性更高、rubric 反转更新罕见；消融显示去掉 critic 后反转率从第一个窗口 6.8% 升至最后一个窗口 14.1%，证明 JIT 验证的不可替代性。RQ4 化学任务案例展示了“标准饱和→归因→降权/新增标准”的闭环。
5. 边界与意义：作者把 harness 定位为“训练配置随策略演化”的通用机制，而非单一组件修补；其研究意义在于给出开放域主观奖励场景下奖励失效的系统性诊断（归因）+ 安全更新（JIT 验证）框架。局限包括对 judge/oracle 质量的依赖、每步推理开销、领域与模态覆盖有限；部分实现细节与完整 baseline 对比需回原文确认。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中“生成”方向直接相关（权重 0.10）：论文研究开放域生成的 RL 后训练，核心是主观奖励信号的设计与维护。

## 基本信息

- 作者：Xiaoqiang Wang, Ting-En Lin, Yuchuan Wu, Pinyi Zhang, Caixin Kang, Ling Li, Bingxue Qiu, Jieping Ye, Yongbin Li, Bang Liu
- 机构：阿里巴巴集团（合理推断：论文致谢标注“This work was supported by Alibaba Research Intern Program”；arXiv 元数据未提供机构字段，具体部门/实验室未知）。
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-10-19
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.26426v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 6 条证据片段（background/method/results/conclusion）与论文元数据，并按 field_evidence_map 分配到对应字段；因证据多处截断，涉及具体数值、接受条件、baseline 映射处已在文中标注“需回原文确认”，核心方法描述结合了摘要、引言与结论信息归纳。
