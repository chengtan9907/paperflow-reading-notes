---
user_id: "cheng tan"
paper_id: 10411
arxiv_id: "2609.02849v1"
title: "Post-Training Language Models for Gold-Medal Performance in Coding Competitions"
publish_date: "2026-09-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.02849v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.02849v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-05T01:31:47"
---
# Post-Training Language Models for Gold-Medal Performance in Coding Competitions

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：competitive programming · international olympiad in informatics (ioi) · post-training · supervised fine-tuning (sft)

## 一句话总结

该论文提出一套面向国际信息学奥林匹克（IOI）的端到端后训练流水线，涵盖大规模题目筛选、合成推理轨迹、监督微调（SFT）与强化学习（RL），并引入反馈驱动的测试时计算策略 GenCorrect，使 NVIDIA Nemotron-3 系列模型在 IOI 2026 现场赛中以 535.4 分超越最高分人类选手（498.27 分），成为首个在 IOI 题目集上得分超过人类冠军的 AI 系统。

## 摘要

> Competitive programming has become a key test of large language model reasoning, with international competitions such as IOI and ICPC representing its most challenging settings. We present an end-to-end specialization pipeline combining large-scale problem curation, synthetic reasoning traces, supervised fine-tuning (SFT), and reinforcement learning (RL). Using 22,000 curated problems, we train Nemotron-3-Nano-CC (30B-A3B) with SFT and RL and Nemotron-3-Ultra-CC (550B-A55B) with SFT alone. We further introduce GenCorrect, a feedback-driven test-time compute strategy that iteratively generates, evaluates, and refines diverse solutions. On IOI 2025, Nano-CC improves from 130 points to 291 after post-training and to 468 with GenCorrect, exceeding the gold threshold of 438.3 while Ultra-CC reaches 502. Guided by these results, we develop a competition-specific Ultra-CC system and evaluate it prospectively during IOI 2026. Under the same time, internet-access, and submission constraints as human contestants, it scores 535.4 out of 600, exceeding both the gold threshold of 361.12 and the top human score of 498.27. To our knowledge, this is the first AI system to outscore the highest-scoring human contestant on an IOI problem set.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决什么问题：
1. 大模型在标准编程基准（如 HumanEval）表现虽好，但面对 IOI/ICPC 这类高难度、约束严格的算法竞赛时仍存在显著差距；IOI 限时、限提交次数、通常无网络，需要跨多步推理、多样化解题策略和错误修正能力。
2. 已有工作（AlphaCode 等）展示了结构化后训练的价值，但对开源模型、规模化后训练（SFT+RL）的具体贡献分解尚不清晰；特别是 RL 在竞赛编程中的增益是否可复现、是否需要与专门的代码 RL 管线结合，是开放问题。
3. 测试时计算的有效利用：简单多数投票或盲目采样在竞赛场景收益有限，需要可反馈、可迭代的搜索机制，才能在数十次提交限制内达到金牌水平。
4. 如何设计一套可重复、可前瞻验证的竞赛评估协议，使其与真实 IOI 规则（时间、环境、提交限制）一致，避免依赖赛题泄漏或非正式评测造成虚高。
5. 隐含问题：评估成功的定义——是从人机协作角度还是全自动系统角度？该文选择了系统级对比，即同一约束下的 AI 与人类选手直接比较（合理推断，因 Limitations 中明确说明这是 system-level comparison）。

Q2: 有哪些相关研究？

相关研究包括：
1. 早期语言模型评测：早在 2021-2022 年，就有论文把 APPS、CodeContests 等作为编程推理评测集，但模型远达不到获奖水平。论文引用了 Li et al. (2022) 的 AlphaCode，它结合领域专属训练、大规模采样、过滤和行为聚类，是首个达到人类中等水平的系统（相关字段中明确提及）。
2. AlphaCode 2：在 AlphaCode 基础上改进采样和过滤，进一步提升了竞赛表现。
3. 来自 OpenAI 和 Google DeepMind 的工作：围绕 frontier 模型，使用额外推理 token 获得 ICPC 2025 金牌（证据片段提到 OpenAI et al., 2025；Lin & Cheng, 2025）。这些工作通常依赖闭源专有模型和大量专用工程。
4. 开源权重模型的竞赛进展：GenCluster 达到 IOI 金牌水平（相关片段提及），说明开源权重 + 高效后训练可以冲击金牌；另一个提及是 NVIDIA 的 Nemotron 3 nano 和 NeMo RL 等基础模型和库。
5. 推理时扩展方法：多数投票（self-consistency）、蒙特卡洛树搜索、程序合成检查等作为 baseline 或相关技术，与论文 GenCorrect 的迭代“生成-评估-改进”范式有关。
6. 测试时训练的早期工作（如 OpenAI o1 系统等）证明了在测试阶段增加思考时间可提高竞赛成绩；本文的 GenCorrect 属于同一研究维度，但强调对多样性解法进行反馈驱动修正。

Q3: 论文如何解决这个问题？

论文如何解决这个问题：
1. 大规模问题策展（Problem Curation）：从多个来源收集 22,000 道精选题目，保证题目覆盖算法竞赛的高难度分布（具体来源和去重策略未在检索证据中给出，合理推断是 CodeContests、IOI 历届题目、AtCoder/Codeforces 等高质量题目经过过滤和标注）。
2. 合成推理轨迹生成（Synthetic Reasoning Traces）：自动产生带有逐步推理过程的解题数据，供 SFT 使用。具体生成方式（是否由更强模型回滚生成、是否过滤失败样本）未被检索证据支持，属于推测。
3. 监督微调（SFT）：在基础模型上用上述数据进行 SFT。作者明确指出 SFT 是单一采样增益最大的环节（来自 Conclusion 片段）。
4. 强化学习（RL）：只对 30B 级别模型进行代码特定 RL（Nano-CC），在 IOI 2025 上进行 SFT 后从 130→291，加入 RL 还能带来小幅额外提升；而 550B 模型（Ultra-CC）只做 SFT，不做代码专属 RL（据 Introduction 描述）。对比组“without code-specific RL”与“with RL”被用于分析贡献。
5. GenCorrect：一种测试时计算策略——(i) 输入问题生成多种不同的初始解法；(ii) 执行/评估这些解法（通常依赖 unit tests 或编译器反馈）；(iii) 将评估结果反馈给模型，让其修正错误或提出替代方案；(iv) 迭代多轮（在 IOI 2025 上为 5 轮），在有限的提交次数内择优提交（Ultra-CC 5 轮后得分 502.0 超过无 GenCorrect 时的 304（依 Introduction 片段））。GenCorrect 与 AlphaCode 的过滤/聚类不同，更强调“feedback-driven”和迭代修正。
6. 竞赛专用系统化：在一般 pipeline 得到验证后（IOI 2025 复盘），作者针对 IOI 2026 做规则对齐：把时间限制、提交次数、是否允许网络访问等都按真人选手规则设定，构建“competition-specific Ultra-CC system”，并将 IOI 2026 作为前瞻性验证。
7. 基础设施：基于 NVIDIA 的 Nemotron-3-Ultra (550B-A55B) 和 Nemotron-3-Nano (30B-A3B)，使用 NeMo RL 训练库（references 中 NVIDIA 2025 条目）。

Q4: 论文做了哪些实验？

论文做了哪些实验：
1. IOI 2025 赛题集上的离线/回顾性评测：
 - Nano-CC 的进展：基础模型 130 分 → 后训练后 291 分（SFT+RL）；+GenCorrect 后到 468 分，超过金牌线 438.3。
 - Ultra-CC 的评估：无代码专属 RL 时为 304 分；使用 GenCorrect（5 轮）后 502 分。
 - 这可能包括单次采样（single-sample）和多次采样/GenCorrect 多个条件（合理推断，原文“single-sample gains”说明有 single-sample 指标）。
2. IOI 2026 前瞻性现场评测：
 - 在 IOI 2026 真实比赛环境中，以与人类选手相同的约束（时间、互联网访问、提交限制），专用 Ultra-CC 系统获得 535.4/600 分；金牌线 361.12，人类最高分 498.27。
 - 这是对 2025 年离线分数的强基线验证：离线 502 → 实际系统 535.4（推测是因为系统版本、GenCorrect 轮数或专用调整的差异）。
3. 消融性对比：SFT 与 RL 的贡献分解（证据在 Conclusion 片段中明确提出 SFT 增益最大，RL 额外小增益）；GenCorrect 开/关（IOI 2025 对比）。
4. 可能包含计算成本记录（训练和测试时 token 消耗），但由于相关章节缺失不可得。实验一致性使用相同问题集和提交限制进行，避免对某个赛题的刻意为调。详细表格、baseline 对比在原文中可能还有，但检索到的字段证据有限。

Q5: 发现了什么实验现象？

实验现象与关键观察：
1. SFT 的增益最大且持久：在 30B 模型上，从 130 提升到 291（+161 分）；作者明确表述 SFT 提供“largest single-sample gains”——指向单次采样的能力提升主要来自 SFT 数据质量而非 RL。
2. RL 带来“smaller additional improvements”——在 Nano-CC 上观察到的 RL 增量相对较小（例如 130→291 是 SFT 和 RL 混合后的总体增益，但 RL 是其中小部分；从无 RL 版 Ultra-CC 304 分看，SFT 本身已经把 550B 模型推到较高水平）。
 - 注意 Ultra-CC 无 RL 时单次采样即得 304，仍高于 Nano-CC 的 291（后者有 RL 但模型小），暗示模型规模仍是核心因素。
3. GenCorrect 在 Ultra 上带来巨大收益（304→502，约 +198），但 Nano 上从 291→468（+177）；两者均超过金牌线。说明测试时反馈迭代可以显著补偿模型能力不足。
4. 模型规模影响：仅用 SFT 的 550B 模型略高于用 SFT+RL 的 30B 模型但差距不悬殊（502 vs 468 都是 GenCorrect 后；但在单采样情况下 Ultra 304 vs Nano 291 也接近），表明规模在单样本上的优势可能不是绝对主导，训练数据质量同样重要。
5. 从 IOI 2025 回顾性分数到 IOI 2026 前瞻性分数的提升（Ultra-CC 502→535.4）：说明竞赛系统在真实环境下通过系统级调优（如专用 agent 逻辑、更精细的用时控制、资源分配）还可获得额外收益（合理推断），而不是纯粹模型能力。
6. 反直觉点：获得“gold medal”并不需要解决所有题目——IOI 2025 金牌线 438.3（60 分制中 600 分满分）意味着只需覆盖约 73% 的分值即可金牌；而 535.4/600 的分数已大幅超过人类冠军 498.27。这暗示 AI 在审题覆盖面、极端算法实现稳定性上达到新的高度。
7. 负结果（推测）：没有列出 RL 的大规模收益，说明直接套用通用 RL 难以超越高质量 SFT 数据；这是论文引导读者注意的一个潜在“负 result”——也解释了为何只在 30B 模型做 RL。
8. 提交效率：在 50 次或更低的提交限制下 GenCorrect 多轮迭代仍有足够鲁棒性，说明它的采样多样性不会过早坍缩到同一错误解法（从最终分可推测）。

Q6: 有什么可以进一步探索的点？

可以进一步探索的点：
1. RL 的可扩展性研究：论文表明 SFT 增益最大而 RL 增益较小，且只在 30B 模型做 RL；未来可在更大模型上研究如何设计竞赛专项 RL，例如利用多题对拍的博弈式奖励、提交成功稀疏奖励的重新加权。
2. 反馈信号增强：GenCorrect 目前使用执行/测试反馈；可延伸为使用形式化验证器、编译 warning、内存时序 profile，甚至将多模型相互评审结果作为反馈。
3. 泛化性：这套 pipeline 是否从编程竞赛扩展到数学奥赛（IMO）、物理奥赛等需要长链推理的领域？合成轨迹和问题策展范式可能是通用“金牌后训练ipeline”。
4. 测试时计算预算与提交次数的最优策略：GenCorrect 轮数、每轮生成样本数、候选集多样性度量的组合优化，可建立“分-时间-计算”三维的帕累托前沿。
5. 解决真实世界算法工程问题：竞赛技能向软件工程的迁移是否成立？代码竞赛与公开算法部署的差距仍然很大，需要新评测。
6. 数据策展可复现性：22,000 题的选择标准、去重、难度标定如何影响效果？公开题目集和许可证是否允许再分发？可作开放数据和研究伦理讨论。
7. 小模型能力蒸馏：Nano-CC 已经接近金牌水平，可以继续研究如何把小模型的成本降低从而普及；反向蒸馏 Ultra 的能力到 Nano。
8. “系统 vs 选手”比较的公平性问题：如何设计更精细的协议（人类可以使用 AI 吗？只能单机？），以及评估工具使用、C++/Python 语言偏好等影响。未来比赛规则变化时系统如何适应。
9. 长期记忆和持续学习：IOI 每年题目类型风格变化，需要更廉价的年度回流更新而不是每次重新收集。
10. 安全问题：能超过人类冠军的解题能力，如用于自动化代码审查、恶意软件分析和漏洞挖掘方面的潜在风险，需要安全评估与限制。

Q7: 总结一下论文的主要内容

论文的全景总结：
作者以 IOI/ICPC 作为 LLM 综合推理能力的极限测试场，目标不是“能通过排行榜上的公开测试”，而是在真实 IOI 规则限制下超越最顶尖人类选手。为此提出了一个端到端的后训练专业化流水线，其四大组件是：
1) 问题策展：从算法竞赛题库中筛选出 22,000 道高质量题目，以覆盖最困难的推理类型和算法主题。
2) 合成推理轨迹：为这些题生成完整 step-by-step 推理链条，使模型能从代码之外学到清晰的数学演绎和算法设计思路。
3) SFT：用这些轨迹进行监督微调。作者明确将其作为能力跃升的最关键步骤（证据：Conclusion 里“SFT provides the largest single-sample gains”）。
4) RL：在 Nano（30B-A3B）这一较小模型上开展针对代码生成/竞赛的强化学习，获得进一步但有上限的提升；在 Ultra（550B-A55B）上没有做代码专属 RL，以便清洗地展示 SFT 的规模效应。
此外还有一个特色组件 GenCorrect：一种迭代式测试时搜索。它吸取 AlphaCode 的“采样-过滤”思想，但比它多一个反馈闭环——模型先产生多种不同解法，执行后得到针对性错误信息，再带着这些信息修正、重写。这样在固定提交次数内显著提升成功率。
实验演进层次：
- 第一层（IOI 2025 回顾）：Nano-CC 130→291→468，说明后训练能翻倍提升，而 GenCorrect 又将结果再推过金牌线 438.3；Ultra-CC 304→502，证明模型规模越大越能享受 GenCorrect 的收益。
- 第二层（系统搭建）：作者告诉读者他们不是拿一个公开模型去现场，而是基于前面经验制造了一个“competition-specific”系统——可能包含更精细的题目分发策略、时限内做题选择、语言选择、提交节奏等（细节原文缺失，但 IOI 2025 到 2026 得分提升直接说明系统级改进有实际作用）。
- 第三层（IOI 2026 前瞻）：在真实赛场与人类选手相同的约束下取得 535.4 分。金牌线是 361.12，人类最高分是 498.27——AI 不但获得金牌，而且甩开金牌线约 174 分，甚至超过人类第一名 37 分。这是“首个在 IOI 题目集上得分超过人类冠军的 AI 系统”。
论文同时在文末强调这是系统级比较，不是同资源比较，承认训练和推理需要大量算力，但也强调实时测试遵循人类时间限制。工作定位在“系统级别能力展示”而非“每模型效率最优”。结合限制条款，作者保持谨慎——现场成绩只能说在相同赛场规则下达成，而不宜解释为 AI 在纯粹智力上击败人类。
论文的价值在于给出了一套可重复的技术组合（curation→traces→SFT→RL→smart test-time search），同时也给出了这种组合的上限证据。开放权重模型能达到如此高度，意味着竞赛专用智能体门槛明显降低；而这套范式如何迁移到现实工程与科学推理，是后续研究者最该追问的问题。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对 agent 研究的启示：GenCorrect 是一种轻量级“critic-execute-revise”agent loop，可应用于通用编程 agent、自动修复和可验证任务；竞赛中的选择策略与时间分配也可借鉴为多任务 agent 的调度；

## 基本信息

- 作者：Aleksander Ficek, Sean Narenthiran, Mehrzad Samadi, Somshubra Majumdar, Boris Ginsburg
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL, cs.MA, cs.SE
- 日期：2026-09-02
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.02849v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次精读参考了论文摘要、heuristic_draft 及由 PDF 语义检索提供的 49 个文本块中的证据，并按 field_evidence_map 映射到相应输出字段；对未获得全文证据支撑的方法细节和实验条件，已用“合理推断”“推测”等标注说明。
