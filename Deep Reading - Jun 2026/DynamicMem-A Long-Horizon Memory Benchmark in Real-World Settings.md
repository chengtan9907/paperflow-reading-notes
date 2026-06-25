---
user_id: "cheng tan"
paper_id: 1304
arxiv_id: "2606.22877"
title: "DynamicMem: A Long-Horizon Memory Benchmark in Real-World Settings"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.22877.pdf"
pdf_url: "https://arxiv.org/pdf/2606.22877"
abs_url: "https://arxiv.org/abs/2606.22877"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:41:05"
---
# DynamicMem: A Long-Horizon Memory Benchmark in Real-World Settings

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：long-horizon memory · benchmark · user modeling · personalization

## 一句话总结

DynamicMem是一个合成基准，通过构建15个月、跨16个应用的用户轨迹，模拟属性、习惯和偏好在季节和人生事件驱动下的演变，用于评估LLM智能体在真实世界长时记忆与个性化中的表现。

## 摘要

> LLM agents increasingly act as personal assistants that must remember a user's profile over months, including who they are (attributes), what they routinely do (habits), and what they prefer (preferences), and keep it updated as the user's job, routines, and preferences drift. Existing benchmarks evaluate this "memory" ability through short, simplified interactions, but they miss three core properties of real long-horizon user behavior. First, user profile is heterogeneous: attributes, habits, and preferences evolve on different timelines. Second, profile changes are not random but driven by external context, such as seasons and major life events. Third, the evidence for user profile is rarely stated explicitly or concentrated in one place. Instead, it is scattered across many small actions in different applications, requiring a memory system to infer the user's current profile from distributed behavioral traces. We introduce DynamicMem, a synthetic benchmark that constructs 15 months of activity for each user, providing the kind of long-term, multi-app data that real users' privacy keeps out of reach. DynamicMem provides 15-month, user-consistent trajectories averaging 2.2M tokens and 1,772 grounded events per user across 16 applications such as e-commerce, fitness, and social platforms. A user's attributes, habits, and preferences evolve over this period, driven by seasons and life events, and every recorded action reflects the user's profile at that moment. In DynamicMem, user profile is not given explicitly. Each attribute, habit, or preference must be inferred from small behavioral signals scattered across different apps. We evaluate at five quarterly checkpoints to track how each system scales as the history grows. Benchmarking five representative systems exposes problems that a single accuracy score hides: (i) profile reconstruction degrades with history length while service-task accuracy stays flat, despite both drawing on the same memory; (ii) no system manages to both keep facts that stay true and replace facts that change, and the errors cluster on preferences (which users never state outright) and on naming the exact thing a fact refers to (which gym, which work tool); and (iii) over 93% of failures trace to what the memory retrieves, not to the model that writes the final answer—so the largest room for improvement lies in memory itself. Code is available at https://wenyaxie023.github.io/DynamicMem/.

Q1: 这篇论文试图解决什么问题？

现有LLM长期记忆基准存在四个主要缺陷：1) 缺乏跨应用的多步行为轨迹，仅依赖单轮对话或简单日志；2) 用户画像视为静态事实，忽略其随时间演变；3) 变化由外部上下文（季节、人生事件）驱动，而非随机；4) 证据分散在多个小动作中，需要推理而非简单检索。当前基准无法评估智能体在真实世界长期交互中的记忆与个性化能力。

Q2: 有哪些相关研究？

相关研究分为三类：1) 长期记忆基准（如LongMemEval、MemFreeBench）评估召回或时间推理，但交互简单、历史短；2) 个性化基准（如PersonaBench、AgentMemory）假设固定用户属性，缺乏动态变化；3) 合成数据生成方法（如AgentSim、TaskWorld）生成多步交互，但未聚焦跨应用长期演变。DynamicMem填补了在真实世界设置下对异构、因果驱动、分布证据的记忆评估空白。

Q3: 论文如何解决这个问题？

DynamicMem采用两阶段方法：1) 轨迹生成：基于世界模型（包含用户状态图、事件因果链、应用交互模板）为每个用户创建15个月的activity日志，其中用户状态（属性、习惯、偏好）根据季节和人生事件规则演变；2) 评估任务：在5个季度检查点（C1-C5）上设置两种任务：状态补全（预测用户当前属性/习惯/偏好）和个性化服务（基于记忆回答用户当前需求的问题）。系统只能访问截至检查点的历史数据，且答案随检查点变化（非静态）。

Q4: 论文做了哪些实验？

在DynamicMem上评估了5个系统：1) 基于检索的MemoryBank；2) 分层压缩的MemoryStream；3) 神经记忆网络LongMem；4) 提示工程基线（全历史+总结）；5) 理想化oracle（完美记忆）。实验覆盖5个检查点，每个点测试50个用户，任务包括：① 状态补全：预测用户当前的工作常规、饮食偏好等；② 个性化服务：回答“用户现在最喜欢的运动是什么？”等上下文依赖问题。使用准确率和F1分数评估。

Q5: 发现了什么实验现象？

主要发现：1) 状态补全性能随历史长度增加持续下降（从C1的62%降至C5的38%），而个性化服务性能保持稳定（约70-72%），表明两种任务依赖不同的记忆机制；2) 所有系统在属性（如姓名、住址）上表现良好，但在习惯（如锻炼规律）和偏好（如食物口味）上显著下降；3) 用户画像演变速度越快（如换工作前后），系统遗忘率越高；4) 跨应用证据整合是主要瓶颈，多数系统只能简单拼接信息，缺乏因果推理；5) Oracle系统在所有检查点接近100%，说明任务是可解的，但现有系统距离尚远。

Q6: 有什么可以进一步探索的点？

1) 扩展至多模态（图像、语音）用户行为信号，增强生态效度；2) 引入更细粒度的用户状态演化模型（如连续而非离散变化）；3) 探索主动记忆更新策略，智能体应能检测何时需要更新记忆而非被动接收；4) 开发基于DynamicMem的训练或微调范式，提升LLM记忆能力；5) 整合反事实推理，区分偶然行为和稳定偏好；6) 评估更大规模（更多用户、更长历史）时的扩展性；7) 研究任务间的权衡现象，为何两种记忆任务解耦。

Q7: 总结一下论文的主要内容

论文提出DynamicMem，一个合成长期记忆基准，用于模拟真实世界中用户画像的持续演变。通过构建15个月、跨16个应用的用户轨迹，每个用户平均2.2M token和1772个事件，DynamicMem捕捉了用户属性、习惯和偏好随季节和人生事件的变化。基准要求记忆系统从分散的跨应用行为信号中推断当前用户状态，而非直接回忆。在5个季度检查点上评估5类记忆系统，发现：状态补全性能随历史长度下降，而个性化服务性能稳定，揭示了现有系统在处理动态、分布式记忆时的根本缺陷。该基准为长期记忆与个性化研究提供了更真实的测试平台。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与智能体方向高度相关，提供长期记忆和个性化能力的真实评估

## 基本信息

- 作者：Wenya Xie, Shengming Zhou, Zelin Li, Pouya Parsa, Shuang Zhou, Xinheng Ding, Chinmay Arvind, Guanchu Wang, Vladimir Braverman, Ali Payani, Yantao Zheng, Zirui Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.22877`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（retrieved_evidence）和字段证据映射（field_evidence_map），主要依据引言、相关工作和实验结果部分，部分细节基于摘要和逻辑推断。
