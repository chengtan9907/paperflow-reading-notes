---
user_id: "cheng tan"
paper_id: 16076
arxiv_id: "2610.17953v1"
title: "Agent-Reflex: Teaching Language Agents to Act Reflectively"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.17953v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.17953v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-18T02:05:48"
---
# Agent-Reflex: Teaching Language Agents to Act Reflectively

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reflective agent training · tool-use agent · noise-robust interaction · reflection skill discovery

## 一句话总结

Agent-Reflex 提出一个三阶段通用训练框架（RSD 技能挖掘、ReSFT 反思监督微调、ORPO 反思策略优化），让基于 Qwen3-8B 的智能体学会在噪声工具反馈和用户干预下主动反思、技能恢复与用户导向纠正，在五个长程工具使用基准和 AgentNoiseBench 上显著超过环境扩展与反思类基线。

## 摘要

> Large language models have evolved from single-turn chatbots into interactive agents capable of long-horizon reasoning. In real deployments, MCP tool services can return noisy feedback and users may intervene to correct the agent, but existing methods provide limited supervision for reflection learning and failure recovery. To bridge this gap, we propose Agent-Reflex, a general training framework for incentivizing proactive reflection in agents. Specifically, Agent-Reflex comprises three stages. First, Environment Reflection Skill Discovery (RSD) injects grounded tool-feedback perturbations into environment scaling, samples a capability-diverse LLM pool, mines matched success-failure pairs, and summarizes reusable reflection meta-skills. Building on this skill library, we then apply Reflective Supervised Fine-Tuning (ReSFT) that converts each skill into supervision for self-reflection, skill recovery, and user-centric correction, strengthening complementary reflective behaviors. Finally, we propose On-Policy Reflective Policy Optimization (ORPO) to combine agentic reinforcement learning under noisy interactions with reflection-guided on-policy distillation to further elicit proactive reflection. Built on Qwen3-8B, Agent-Reflex reaches 58.6 overall on five long-horizon tool-use benchmarks and 29.1 on AgentNoiseBench, surpassing strong environment-scaling and reflection baselines. Experiments on six challenging agent benchmark suites demonstrate the effectiveness of Agent-Reflex. Further analyses show that Agent-Reflex improves agents' robustness to noise while maintaining stable training dynamics, supporting scalable agent training. Our code is available at https://github.com/RUC-NLPIR/Agent-Reflex.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：在真实部署中，语言智能体与 MCP 工具服务交互时会遇到两类现有训练方法很少覆盖的监督缺口。第一，工具环境返回的反馈是噪声化的，可能包含错误、不完整或误导性信息，导致智能体在失败后缺乏可用的反思信号；第二，用户在智能体执行过程中可以中途介入纠正，而现有方法很少把这种‘用户指导下的纠正’纳入反思学习。论文指出，虽然环境扩展（environment scaling）可以提升交互多样性和分布外泛化，但现有生成式环境（如 Simia、GenEnv）依赖 LLM 作为隐式文本世界模型，可能产生幻觉性反馈；可执行环境虽复现真实交互逻辑，却没有专门刻画噪声反馈发生后的反思与恢复。同时，推理时反思方法（如 Reflexion）把反思当作提示而非常规化训练目标，难以内化为模型参数中的能力。因此论文把问题形式化为：如何系统性构建带噪声反馈和用户纠正的反思技能库，并把这些技能转化为可规模化、可泛化的训练信号，使模型在推理时主动反思而不是依赖外部指导。

Q2: 有哪些相关研究？

相关工作按四条线组织。一是环境扩展（environment scaling）路线：一类把 LLM 当作隐式文本世界模型生成反馈（Simia、GenEnv），降低环境工程成本但反馈可能幻觉；另一类显式复现真实交互逻辑，合成可执行工具和环境数据库（Song et al., 2026; Wang et al., 2026b; Tu et al., 2026; Dong et al., 2026a），包括论文对比的 EnvScaler 协议；Agent-Reflex 的评估遵循 EnvScaler 的标准 MCP 格式和任务模板。二是推理时反思与纠正轨迹学习方法：Reflexion（Shinn et al., 2023）、Self-Refine（Madaan et al., 2023）、Yuan et al. 2025、Bi et al. 2026、Zhang et al. 2026、Chen et al. 2026 等，但多数方法把反思当作推理时提示或仅学习纠正轨迹，没有把噪声反馈的反思内化为训练目标。三是噪声增强训练（noise-augmented training），对比的基线包括 AgentNoiseBench（Wang et al., 2026a）等噪声注入方法。四是轨迹级强化学习算法：Shao et al. 2024（DeepSeek-R1 的 RL 路线）、Yu et al. 2025、Hu et al. 2025、Wu et al. 2026（SEED 估计器相关，ORPO 第二路直接复用其估计器），说明 ORPO 与现有 RL 估计器正交。由于 retrieved_evidence 只覆盖 introduction/results/references/conclusion 片段，Related Work 细节建议回原文核对完整引用列表。

Q3: 论文如何解决这个问题？

Agent-Reflex 是包含三个训练阶段的通用框架，逐层把‘主动反思’转化为可训练的技能、监督和优化目标。第一阶段 RSD（Reflection Skill Discovery）：在环境扩展过程注入扎根的工具反馈扰动（grounded tool-feedback perturbations），使环境在训练时就能模拟真实部署中的噪声；随后采样一个能力多样化的 LLM 池（capability-diverse LLM pool），执行扰动后的任务，挖掘匹配的 success-failure 轨迹对；再对这些轨迹对归纳出可复用的反思元技能（reflection meta-skills），构成技能库。第二阶段 ReSFT（Reflective Supervised Fine-Tuning）：把技能库中的每一条技能转化为三类互补的监督信号——自我反思（self-reflection，模型复盘自己错在哪）、技能恢复（skill recovery，从失败状态中恢复）、用户中心纠正（user-centric correction，针对用户干预的纠正行为），从而避免单一监督形态覆盖不全。第三阶段 ORPO（On-Policy Reflective Policy Optimization）：在噪声交互下做 agentic reinforcement learning，同时用反思引导的 on-policy distillation 把技能先验注入策略；一个关键设计是‘特权槽’（privileged slot），训练时把技能 prepend 到任务前使同一策略充当 teacher，对没有技能先验采样的 student rollout 重新打分，并通过 stop-gradient log-likelihood shift 和 sigmoid gating（Wu et al., 2026 的估计器）实现蒸馏；ORPO 与第一条 RL 路线正交，第二路直接复用 SEED 估计器，贡献点在于特权槽的内容而非估计器本身。评估协议沿用 EnvScaler 的 MCP 格式和任务模板，所有 trained 方法（包括基线）都从同一个 Qwen3-8B 主干出发。

Q4: 论文做了哪些实验？

实验部分分为两组基准。（1）长程工具使用：BFCL-v4、ACEBench、ToolSandbox 200 个分层抽样任务、τ²-Bench、VitaBench，覆盖通用函数调用和交互式工作流；（2）噪声交互：AgentNoiseBench（Wang et al., 2026a），在 τ²-Bench 和 VitaBench 的工具噪声与用户噪声设置上实例化。所有评估遵循 EnvScaler 的标准 MCP 格式和任务模板，每个基准用官方指标评分。基线分为三类：反思方法（Reflexion、Zhao et al. 2024、Yuan et al. 2025、Chen et al. 2026）、环境扩展方法（Simia、Xu et al. 2025、EnvScaler、Wang et al. 2026b、Xu et al. 2026、Dong et al. 2026a）、轨迹级 RL 算法（Shao et al. 2024、Yu et al. 2025、Hu et al. 2025、Wu et al. 2026）。所有 trained 方法从相同 Qwen3-8B 主干出发。主结果：Agent-Reflex 在五个长程智能体基准上取得最高总体分数，并报告了‘超越推理时指导的内化反思’等观察。消融实验：移除任一组件都会降低性能（对应 ANB-τ²、ANB-Vita 两个噪声基准上的完整模型分数对比）；训练动态图显示在相同任务池与匹配预算下，Agent-Reflex 的 RL 步进曲线和 reward 曲线相比基线更稳定。论文特别声明 RSD 构建过程从未参考 AgentNoiseBench 的 taxonomy 或 injector，两种噪声设计构造方式完全不同，Agent-Reflex 使用的噪声完全来自自身扰动机制，以排除评估泄漏嫌疑。

Q5: 发现了什么实验现象？

从已检索的证据可以归纳出以下实验现象：（1）在五个长程工具使用基准上，Agent-Reflex-8B 相比强基线取得最高 overall 分数（58.6），说明把反思内化为训练目标优于把反思当作推理时提示；（2）在 AgentNoiseBench 上达到 29.1，说明对 AgentNoiseBench 引入的工具/用户噪声干扰具有鲁棒性，且这种鲁棒性不是来自在评测基准上‘见过’同类噪声——因为 RSD 构建时从未 consult 该基准的 taxonomy 或 injector，两种噪声设计构造方式完全不同，排除了数据泄漏式性能提升；（3）消融实验显示移除 RSD、ReSFT、ORPO 任一组件都会在两个噪声基准（ANB-τ² 45.9、ANB-Vita 12.3 上，完整模型分数对应）上造成性能下降，验证三阶段设计互补而非冗余；（4）训练曲线显示 Agent-Reflex 在相同任务池和匹配预算下 RL 步进过程中 reward 与 accuracy 更稳定，未出现 reward 震荡或训练崩溃，说明反思监督起到了稳定 agentic RL 的作用；（5）从主结果观察（1）‘Internalizing Reflection beyond Inference-Time Guidance’来看，模型把反思能力内化为参数，而非依赖推理时外部提示。注意：检索到的数字零散且部分来自表格噪声，消融具体数值和每个基准的逐项得分建议回原文核对 Table 1/Table 2。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向包括：（1）把 RSD 的噪声扰动从工具反馈扩展到场、多模态工具返回、异步事件和跨进程状态变化，检验反思技能库在更丰富噪声类型下的覆盖；（2）把用户干预建模从单轮纠正扩展到多轮对话式引导、隐式偏好反馈和跨会话长期纠正，提升 user-centric correction 的生态效度；（3）将 ORPO 的特权槽蒸馏推广到更大模型（如 Qwen3-14B/32B、DeepSeek 系列）和多主干架构，检验框架的规模扩展性（论文标题和摘要都强调 scalable agent training，但实验以 8B 为主）；（4）研究技能库的组合与遗忘问题，例如技能数量增长后如何避免冲突、如何按任务动态选择技能子集（类比 MoE 式技能路由）；（5）把 Agent-Reflex 与在线环境持续学习结合，让 RSD 在部署后从真实 MCP 噪声中持续挖掘新技能，形成闭环；（6）探索反思技能的跨任务迁移边界，测量在某个工具域归纳的元技能在未见过的 MCP server 上的零样本泛化；（7）在科学计算/生物信息类工具场景（对应读者画像中的 ai-for-science 偏好）验证框架，因为这类工具链同样存在噪声输出和用户纠错需求；（8）理论层面分析 ORPO 中 stop-gradient 与 sigmoid gating 对策略梯度的偏差-方差影响，以及反思蒸馏是否等价于某种 reward shaping。

Q7: 总结一下论文的主要内容

Agent-Reflex 论文的论证主线是：真实部署中智能体面对的 MCP 工具反馈充满噪声、用户会随时介入纠正，而现有训练方法只给智能体提供有限监督，无法学会主动反思和失败恢复。作者把这一空白拆成两个具体缺口：缺乏噪声环境反馈、执行中缺乏用户引导的纠正。技术主线是三个阶段的训练框架：（1）RSD 通过在环境扩展中注入扎根工具反馈扰动，并采样能力多样化 LLM 池、挖掘匹配 success-failure 轨迹对，归纳可复用的反思元技能库，解决‘反思信号从哪里来’的问题；（2）ReSFT 把每项技能翻译成自我反思、技能恢复、用户中心纠正三种互补监督，解决‘技能如何变成训练信号’的问题；（3）ORPO 把噪声交互下的 agentic RL 与反思引导的 on-policy 蒸馏结合，通过‘特权槽’让同一策略充当 teacher 重打分 student rollout，解决‘如何让反思从被动变为主动’的问题。实验主线是六套挑战性基准（BFCL-v4、ACEBench、ToolSandbox 200、τ²-Bench、VitaBench、AgentNoiseBench），统一从 Qwen3-8B 出发，与反思方法（Reflexion 等）、环境扩展方法（Simia、EnvScaler 等）、轨迹级 RL 方法三族基线对比，取得总体最高分（58.6 overall，AgentNoiseBench 29.1）。消融显示三阶段互补，训练曲线稳定，并且通过‘构建 RSD 与 AgentNoiseBench 独立’的声明排除噪声泄漏。结论是 Agent-Reflex 改善了长程工具使用和噪声鲁棒性同时保持稳定训练动态，支持可扩展智能体训练。代码开源。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与‘agent’研究方向直接相关：论文研究的是语言智能体的反思训练方法，而非提示工程式反思

## 基本信息

- 作者：Guanting Dong, Xinyu Yang, Jiajie Jin, Yuyang Hu, Xiaoshuai Song, Yifei Chen, Xiaoxi Li, Jiejun Tan, Yutao Zhu, Tat-Seng Chua, Ji-Rong Wen, Zhicheng Dou
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-10-13
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.17953v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索的证据片段（Introduction/Results/References/Conclusion 共 9 条命中），并结合论文摘要与启发式草稿补全；部分消融数值与细节推断建议回原文核对。
