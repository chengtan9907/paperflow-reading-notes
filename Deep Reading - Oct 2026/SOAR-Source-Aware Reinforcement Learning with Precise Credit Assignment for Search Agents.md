---
user_id: "cheng tan"
paper_id: 15488
arxiv_id: "2610.13157"
title: "SOAR: Source-Aware Reinforcement Learning with Precise Credit Assignment for Search Agents"
publish_date: "2026-10-12"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.13157.pdf"
pdf_url: "https://arxiv.org/pdf/2610.13157v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-12T13:15:46"
---
# SOAR: Source-Aware Reinforcement Learning with Precise Credit Assignment for Search Agents

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：search agent · reinforcement learning · evidence fabrication · credit assignment

## 一句话总结

SOAR 提出面向搜索智能体的源感知强化学习框架，通过显式分离外部检索信息与内部知识流、引入源一致奖励，解决 RL 训练中证据伪造问题，在 3B/7B 规模上以更低的 Evidence Fabrication Rate 显著提升开放域问答性能。

## 摘要

> 论文指出：尽管 "think-then-search"（先思考后搜索）范式的代表性框架能在开放域任务中显著提升搜索智能体的强化学习效果，但存在一个关键失败模式——证据伪造（Evidence Fabrication），即智能体绕过外部检索信息，仅凭不可靠的内部知识编造错误答案。作者认为根源在于推理轨迹的不透明性，导致模型过度依赖内部知识作为捷径。为此提出 SOAR（SOurce-Aware Reinforcement Learning）：一是通过 Source-Aware Workflow 把外部信息（Eext）与内部知识（Eint）拆成并行可观察流；二是用 Source-Consistent Reward 约束自我声明的证据来源与推理过程严格一致，进而在不同知识流之间做精确信用分配。实验显示，SOAR 在 3B 和 7B 基座上分别超过 baseline 7.3% 与 5.4%，同时将证据伪造率降低 74.8%。

Q1: 这篇论文试图解决什么问题？

1. 核心问题：在搜索智能体 RL 训练中，存在证据伪造（Evidence Fabrication）这一系统性失败模式。具体表现为智能体不基于检索到的外部证据（Eext）进行推理，而是绕过外部信息、依赖内部知识（Eint）编造答案，甚至偶尔在未检索到关键证据时恰好蒙对答案。2. 已有研究局限：Jia et al. (2025)、Xu et al. (2025)、Xinjie et al. (2025) 等先前工作虽然也观察到证据伪造现象，但普遍将其归因于模型容量不足或数据偏差，忽视了搜索智能体 RL 框架本身在架构层面的缺陷。作者用 Figure 1(c) 量化了此前方法的 EFR：Search-R1 高达 16.0%，IKEA 为 13.4%，AutoRefine 为 11.1%，说明这是尚未解决的开放问题。3. 根本原因分析：作者认为证据伪造源于推理轨迹的固有不可观测性/不透明性。当 RL 只看到成功的最终结果时，无法区分“成功是来自对 Eext 的正确综合”还是“来自未落地的 Eint 启发式猜测”；因此 RL 无法提供精确信号来抑制内部捷径，反而会强化不忠实的推理行为。4. 由此引出需求：需要让证据来源显式化、可观测，把单一的整体推理过程（monolithic reasoning）改造成 source-explicit 轨迹，从而支持在不同知识流之间做精确的信用分配（precise credit assignment）。5. 边界与风险（合理推断）：SOAR 强调 source 信息的显式化引入，这意味着需要额外监督信号或提示工程来提取 Eint/Eext 标签，并可能引入额外计算开销（原文提到跨 GPU 外部检索和实现分析见 Appendix G.1）。

Q2: 有哪些相关研究？

1. 静态 RAG 到自主搜索智能体的演进：论文把相关工作放置于从静态检索增强生成（RAG）到自主搜索智能体的发展背景中，引用 Shi et al. 2025（AutoRefine）、Jin et al. 2025b（Search-R1）、Yao et al. 2022（ReAct）、Zheng et al. 2025 等。这些工作让 LLM 通过 RL 学会与外部搜索引擎交互、迭代检索证据并支撑多步推理。2. 证据伪造的既有观察：Jia et al. 2025、Xu et al. 2025、Xinjie et al. 2025 等工作也识别了证据伪造现象，但作者指出其解释集中于模型容量或数据偏差，而非框架架构限制。3. Search-R1（Jin et al. 2025b）：作为主要 baseline 之一，其 EFR 为 16.0%，代表 think-then-search RL 训练的代表性框架。4. IKEA（Huang et al. 2025b）：EFR 13.4%，在训练中注入误导信息以缓解检索噪声导致的内部知识回退，属于数据层面干预的代表。5. AutoRefine（Shi et al. 2025）：EFR 11.1%，是自适应、economical 使用工具结构透明轨迹的代表（合理推断：without tools relying on external judges 暗示其不用外部判别器而保持结构透明的轨迹）。6. 相邻方向（合理推断）：beyond general agents, several recent works like IKEA... 提示论文还讨论了通用智能体之外的专门搜索智能体工作；方法区还引用 Liang et al. 2026a 等。7. 与 RAG/幻觉抑制的关系：论文沿用 Hendrycks et al. 2020、Wei et al. 2022、Huang and Chang 2023、Schick et al. 2023、Che et al. 2024 等关于 LLM 静态参数知识局限与幻觉的讨论，把证据伪造定位为幻觉调控在 agentic RL 场景下的延续。8. 风格定位（合理推断）：相对以往只改数据或只改奖励的工作，SOAR 从轨迹结构本身入手，属于架构/训练范式层面的修正，这与用户画像中系统性工作优先的偏好吻合。

Q3: 论文如何解决这个问题？

1. 总体思路：把不可观测的整体推理（monolithic reasoning）改造为来源显式（source-explicit）的轨迹，使 RL 能够区分并分别奖励对 Eext 的忠实综合与对 Eint 的恰当使用，从而精准抑制证据伪造。2. 模块一：Source-Aware Workflow——将外部信息 Eext 与内部知识 Eint 解耦成并行、可观察的信息流。结合检索证据用提示模板提取 Eint（Table 14 展示了提取 Eint 的 prompt template，强制严格格式），可推断该 workflow 会在推理过程中显式标注/分离使用搜索证据与使用内部记忆的推理片段；同时强调实现 functional independence of internal/external streams（Introduction 证据提到确保内部/外部流的功能独立性）。3. 模块二：Source-Consistent Reward——强制模型自我声明的证据来源与其实际推理过程严格对齐。设计上应包含规则或模型判断对声称用了 Eext 但实际未检索到关键证据却答对这类情况的惩罚；检索证据中 occasionally reaches the correct prediction without retrieving key evidence 正是要惩罚的对象。4. RL 训练框架：搜索智能体与搜索引擎交互，在内部知识库与外部检索证据上生成一组多样推理轨迹 {o1, o2, ..., og}，由参考模型和基于规则的奖励计算 Group Advantage 进行强化学习（limitations 证据片段描述了这一训练管线）。SOAR 应是在 GRPO 类 group-based policy optimization 基础上加入 source-consistency 信号（合理推断，需原文确认具体 RL 算法）。5. 实现细节：外部检索跨 GPU 并行以减少开销，详细分析与实现见 Appendix G.1；Eint 的提取用严格格式的 prompt（Table 14），保证提取一致性。6. 设计意图：通过显式来源标注，使正确但证据缺失的成功样本不再被 Reward 模型误判为高质量示范，从而从根源上切断对内部捷径的正强化。

Q4: 论文做了哪些实验？

1. 总体协议：在 3B 和 7B 两种基座规模上对比 SOAR 与 Search-R1、IKEA、AutoRefine 等代表性方法，主要测开放域问答准确率与 Evidence Fabrication Rate。命中证据显示 baseline 的性能与 EFR 数据分别见表（Table 5、Table 6 及正文），但论文片段未给出 EFR 的完整定义公式，需回原文确认（合理推断为答案正确但推理未引用检索证据的比例或据此设计的自动判定）。2. 主结果（抽象）：SOAR 在 3B 基座上超过 baseline 7.3%，在 7B 基座上超过 5.4%，同时证据伪造率降低 74.8%。3. 表格细节：正文 method 区给出 EFR 缩减量表：Search-R1 的 EFR 16.08%（相对基线 7.57% 上 Reduction 24.14% 的语义与表头对应关系因截图残片不完整，不建议直接解读）；SOAR 相对 Search-R1 的 reduction 为 82.30%（EFR 1.34% vs 7.57%）、71.33%（6.92% vs 24.14% 的对应关系需原文确认）与 74.81%（4.05% vs 16.08%）。初步可读：去掉 Source-Consistent Reward 后 EFR 上升到 6.75%（相对 Search-R1 降 58.02%），完整 SOAR 进一步降到 4.05%（相对 Search-R1 降 74.81%），说明 Source-Consistent Reward 贡献显著。4. QA Stability（Table 5）：在干净训练上 SOAR 单跳 0.545、多跳 0.375、平均 0.448；加入 disturb 后为 0.543（↓0.002）、0.345（↓0.030）、0.429（↓0.019）。对照 Search-R1（0.515/0.342/0.415，disturb 后 0.490/0.290/0.376）与 AutoRefine、IKEA，SOAR 在扰动下性能下降最小。5. Evidence Fabrication 鲁棒性（Table 6）：默认训练下 SOAR EFR 4.05%，扰动后 4.38%（+0.33%）；Search-R1 从 16.08% 到 18.23%（+2.15%），IKEA 从 13.19% 到 14.98%（+1.79%），AutoRefine 从 11.45% 到 12.96%（+1.51%）。SOAR 的绝对 EFR 与扰动增幅都最低。6. 鲁棒性实验协议（Systematic Robustness）：在 5% 训练样本中注入噪声，针对长尾查询（模型内部知识本已足够）把正确检索证据替换为误导信息，制造推理冲突，检验智能体是否会退回内部先验。7. 消融：w/o Source-Consistent Reward（去掉 Source-Consistent Reward）对比，验证该奖励模块对降低 EFR 的贡献。8. 其他实验（合理推断）：表 4 涉及跨 GPU 外部检索的性能与成本分析，详细见 Appendix G.1；Table 14 是 Eint 提取 prompt template（属于方法验证）；可能存在附录中的更多消融与 case study，需原文确认。

Q5: 发现了什么实验现象？

1. 核心现象：SOAR 显著压低 Evidence Fabrication Rate——完整 SOAR 在图片/表格呈现的三个设定下 EFR 分别降至 1.34%、6.92%、4.05%，相对 Search-R1 基线降低 82.30%、71.33%、74.81%；即便去掉 Source-Consistent Reward，也已相对 Search-R1 降低 63.67%、55.30%、58.02%，说明 workflow 本身（来源显式化）已能大幅缓解证据伪造，而 reward 进一步压缩剩余空间。2. 反直觉点：SOAR 在鲁棒性测试中表现出的优势不只来自检索更快/更准，而来自面对冲突证据时保持搜索主动策略（不退回内部先验）的能力；在 5% 污染下其 QA 性能下降仅 0.019（平均），而 Search-R1 下降 0.039、IKEA 下降 0.038、AutoRefine 下降 0.033。3. EFR 增长幅度对比：污染条件下 SOAR 的 EFR 仅 +0.33%，远低于 Search-R1（+2.15%）、IKEA（+1.79%）、AutoRefine（+1.51%），说明 SOAR 对局部矛盾的免疫性更强。4. 扰动对多跳任务影响更大：所有方法的单跳性能下降普遍小于多跳（SOAR 单跳 -0.002 vs 多跳 -0.030；Search-R1 单跳 -0.025 vs 多跳 -0.052），说明长链推理对检索噪声更敏感，这一趋势对所有方法成立，但 SOAR 的绝对降幅最小。5. 指标间张力（合理推断）：SOAR 的 QA 准确率（0.448 平均）在干净训练下并非最高——IKEA 的平均 0.402、AutoRefine 0.425、Search-R1 0.415，SOAR 0.448 是最高的但优势集中在多跳（0.375 vs Search-R1 0.342）；单跳上 Search-R1 0.515 与 SOAR 0.545 差距较小。这说明 SOAR 的收益主要体现在需要多步证据综合的场景。6. 负结果/失败案例证据：检索片段中 answer despite failing to retrieve the key supporting evidence 描述的正是未检索到关键证据但恰好答对的典型失败案例，SOAR 的 self-verification 能力（superior self-verification capabilities）可主动识别检索信息不足/冲突并继续搜索而非直接作答。7. 趋势观察：EFR 基数越高的方法（Search-R1 16.08% > IKEA 13.19% > AutoRefine 11.45%），在污染下的 EFR 增幅也越大（+2.15% > +1.79% > +1.51%），呈现越依赖内部捷径越经不起噪声的规律；SOAR 同时打破基数与增幅两个纪录。

Q6: 有什么可以进一步探索的点？

1. 将 SOAR 从开放域问答推广到 multi-hop 之外的知识密集型任务（如长文档问答、事实验证、代码检索、工具调用链），验证 source-explicit 轨迹在不同任务形态下的通用性。2. 把 Source-Consistent Reward 泛化为更细粒度的证据引用级奖励：不仅区分 Eext/Eint 两个大流，还可对检索片段 A vs 片段 B 做逐条信用分配，进一步提升 grounding 精度。3. 与推理时计算（inference-time compute）结合：在 RL 训练之外，把 source-consistency 作为 self-verification 的推理时信号，引导模型在检索冲突时主动追加搜索（论文已展示 SOAR 具备 superior self-verification，可进一步做推理时搜索策略）。4. 理论化证据伪造：对内部捷径正强化建立可量化的 credit-assignment 理论或 sample complexity 分析，解释为何 group-based advantage 会在轨迹不透明时偏向 Eint。5. 扩展到更大规模（>7B）与多模态检索（图像/表格证据），观察 EFR 是否随规模持续下降、reward 设计的规模效应。6. 处理长尾查询的分布偏移：论文的噪声实验只覆盖 5% 样本，可进一步研究更高污染率、连续污染、对抗性检索噪声下的行为退化边界。7. 减少工程开销：跨 GPU 检索并行是工程解决方案，未来可探索异步检索、缓存检索结果或蒸馏 source-consistency 信号到小模型，降低训练成本。8. 与数据层面干预（如 IKEA 的误导注入）结合，形成架构+数据+奖励三重防护，观察边际收益。9. 把 source-aware 思想用于 RLHF/安全对齐：让模型在拒绝回答或知识边界处显式声明证据来源，可能改善幻觉与过度自信问题。10. 可解释性研究：利用 Eext/Eint 并行流分析模型何时、为何转向内部知识，构建 faithfulness probes，为搜索智能体提供机制层面的可解释性。

Q7: 总结一下论文的主要内容

论文以 think-then-search 搜索智能体 RL 训练为背景，抓取了一个此前被归因为模型容量/数据偏差、实则为框架结构性缺陷的失败模式——Evidence Fabrication（证据伪造）。作者用 Figure 1(c) 给出三个代表性框架的 EFR 量化（Search-R1 16.0%、IKEA 13.4%、AutoRefine 11.1%），证明其普遍性。核心论点：推理轨迹的不透明性使得 RL 的 reward 无法区分忠实综合外部证据得到的成功与靠内部知识捷径侥幸得到的成功，于是 RL 非但不惩罚、反而强化了不忠实的推理。SOAR 的解法分两层：第一层用 Source-Aware Workflow 把 Eext 与 Eint 拆成并行可观察流（并用严格格式 prompt 提取 Eint、跨 GPU 并行外部检索降低开销）；第二层用 Source-Consistent Reward 强制模型自我声明的证据来源与实际推理一致，使 credit assignment 能在不同知识流间精确进行。实验在 3B/7B 基座上对比 Search-R1、IKEA、AutoRefine：主结果上 SOAR 分别提升 7.3%/5.4%、EFR 降低 74.8%；在 5% 污染训练（长尾查询正确证据被替换为误导信息）的鲁棒性测试中，SOAR 的 QA 平均分仅下降 0.019，EFR 仅上升 0.33%，各项均优于 baseline。消融表明即使去掉 Source-Consistent Reward，仅靠 source-aware workflow 也能把 EFR 相对 Search-R1 降低约 55%-64%，加上该 reward 后进一步降低到约 1.34%-6.92%（降幅 71%-82%）。整体上，SOAR 把证据来源从隐变量变成显式结构，把忠实性从隐式期望变成可优化目标，是搜索智能体 RL 训练范式层面的系统性修正。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与用户画像直接相关：该论文属于 agent 方向（权重 0.10），且聚焦搜索智能体的 RL 训练，是 agentic RL 与 RAG 的交叉点

## 基本信息

- 作者：Shengze Huang, Zihao Tang, Jinluan Yang, Xin Yu, Zhenghao Lin, Xiao Liu, Zengxuan Wen, Yeyun Gong, Haizhen Huang, Weiwei Deng, Feng Sun, Qi Zhang, Fei Wu, Kun Kuang
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-12
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.13157`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成以 arXiv:2610.13157 的 Abstract、Introduction/Method 检索片段与启发式草稿为依据；PDF 片段存在截断与版式噪声，关键表格数值已尽量交叉核对，部分细节标注为需原文确认。
