---
user_id: "cheng tan"
paper_id: 515
arxiv_id: "2606.16826v1"
title: "ATOM-Bench: A Real-World Benchmark for Atomic Skills and Compositional Generalization in Manipulation Policies"
institution: "None"
publish_date: "2026-06-15"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.16826v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.16826v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-18T00:09:53"
---
# ATOM-Bench: A Real-World Benchmark for Atomic Skills and Compositional Generalization in Manipulation Policies

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：robot manipulation · benchmark · compositional generalization · atomic skills

## 一句话总结

ATOM-Bench是一个用于评估操作策略中原子技能和组合泛化的真实世界基准，包含30个原子任务和24个组合任务，并引入原子分数和组合失败共享指标来诊断失败模式。

## 摘要

> Generalist manipulation policies are increasingly presented as foundation models for robotic control, but their real-world generalization remains difficult to diagnose. A policy may succeed on demonstrated tasks while still failing to execute fine-grained atomic skills or recombine learned skills in new task structures. We introduce \textbf{ATOM-Bench}, a real-world benchmark for evaluating both atomic skills and compositional generalization in manipulation policies. ATOM-Bench factorizes tabletop manipulation into motor atoms and instruction atoms, and contains 30 atomic tasks and 24 held-out compositional tasks across paired single-arm and dual-arm robot tracks. We collect 3,000 human demonstrations for atomic fine-tuning and release both the demonstration data and evaluation rollout data to support reproducible real-world evaluation. Policies are fine-tuned on atomic tasks and evaluated on both atomic skill acquisition and held-out compositional tasks. We further introduce Atomic Score (AS) and Compositional Failure Share (CFS) to distinguish failures caused by weak atomic skills from failures caused by limited compositional reuse. Through 2,700 physical rollouts on five representative manipulation policies, we find that current policies can acquire simple instruction-grounding skills, but still struggle with fine-grained motor atoms, counting, and logical filtering. More importantly, strong atomic performance does not reliably transfer to held-out compositional tasks. ATOM-Bench provides a diagnostic testbed for studying whether failures arise from weak motor execution, poor instruction grounding, or limited compositional reuse.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决通用操作策略的真实世界泛化难以诊断的问题。现有模拟基准容易被高容量策略吸收，物理挑战不足；真实世界评估昂贵且难以扩展。缺乏一个能够区分原子技能获取失败和组合泛化失败的诊断性基准。

Q2: 有哪些相关研究？

相关研究包括模拟基准（如RLBench、MetaWorld）和真实世界基准（如RoboArena、RoboChallenge、GM-100、SceneReplica、FurnitureBench）。这些基准要么物理保真度不够，要么任务分解不细，要么未明确分离原子技能和组合泛化。此外，近期VLA模型（如Pi0.5、LingBot-VLA、GROOT N1.6、SmolVLA、Motus）在语言条件机器人任务上表现强劲，但泛化来源不明。ATOM-Bench填补了诊断原子与组合能力的空白。

Q3: 论文如何解决这个问题？

论文提出ATOM-Bench基准，包括：（1）任务分解：将桌面操作分解为12个动作原子（如拾取、放置、推、拉）和24个指令原子（如颜色、大小、类别选择），形成30个原子任务和24个保留组合任务。（2）两条轨道：单臂和双臂机器人平台。（3）数据收集：3000个人类演示用于原子任务微调。（4）评估协议：策略在原子任务上联合微调，评估原子技能和保留组合任务；另设单任务微调基线。（5）诊断指标：原子分数（AS）衡量原子技能成功率，组合失败共享（CFS）度量由原子薄弱导致的组合失败比例。

Q4: 论文做了哪些实验？

实验评估了五种通用操作策略：Pi0.5、LingBot-VLA、GROOT N1.6、SmolVLA和Motus。使用两种微调协议：原子迁移微调（联合微调所有原子任务）和单任务微调（每个原子任务独立训练）。每个任务在10个固定物理随机种子下评估，共进行2700次物理部署。还进行了扰动鲁棒性测试。

Q5: 发现了什么实验现象？

当前策略能有效获取简单指令接地技能（如按颜色拾取），但在细粒度动作原子（如精确推动、旋转）和需要从集合中选择的指令原子（如计数、排除）上表现差。原子性能强并不意味着组合泛化强：许多策略在原子任务上成功率高，但在组合任务上失败率高，表明组合复用受限。原子分数与组合成功率相关性弱，组合失败共享指标显示大多数失败源于原子技能薄弱而非组合结构错误。

Q6: 有什么可以进一步探索的点？

未来工作包括：（1）扩展任务覆盖以包括更多操作因子（如铰接物体、移动操作）。（2）开发自动化场景重置、成功检测和注释工具以降低评估成本。（3）探索更有效的原子-组合迁移学习策略。（4）在更多策略（如端到端VLA和分层方法）上验证。（5）研究原子技能组合的结构性约束（如顺序、并行）对泛化的影响。

Q7: 总结一下论文的主要内容

论文提出ATOM-Bench，一个真实世界基准，旨在系统评估操作策略的原子技能获取和组合泛化能力。基准分解桌面操作为动作原子和指令原子，包含30个原子任务和24个保留组合任务，覆盖单臂和双臂场景。通过3000个人类演示和2700次物理部署，评估了五种代表性策略。实验发现当前策略在简单指令接地技能上表现良好，但在精细动作原子和需要对象选择/逻辑推理的指令上失败。更重要的是，强原子技能无法可靠迁移到组合任务，表明组合泛化是瓶颈。引入的Atomic Score和Compositional Failure Share指标有效诊断失败模式。该基准为理解操作策略泛化来源和指导改进提供了诊断工具。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：ATOM-Bench为操作策略的泛化诊断提供了系统方法论，可迁移到其他机器人评估设计。

## 基本信息

- 作者：Zenan Wu, Bingqing Wei, Lu Liu, Zheqi He, Xi Wang, Jiakang Liu, Zehui Li, Guocai Yao, Jing-Shu Zheng, Xi Yang, Yongtao Wang
- 机构：None
- 来源：arxiv
- 主题/分类：cs.RO, cs.AI
- 日期：2026-06-15
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.16826v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的证据片段，并优先采用field_evidence_map对应的证据，结合heuristic_draft进行补全和润色。
