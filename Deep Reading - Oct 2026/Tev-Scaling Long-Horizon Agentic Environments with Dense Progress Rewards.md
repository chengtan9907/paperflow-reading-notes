---
user_id: "cheng tan"
paper_id: 16110
arxiv_id: "2610.18868v1"
title: "Tev: Scaling Long-Horizon Agentic Environments with Dense Progress Rewards"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.18868v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.18868v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-18T02:08:38"
---
# Tev: Scaling Long-Horizon Agentic Environments with Dense Progress Rewards

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：long-horizon agents · dense progress reward · latent world synthesis · reward shaping

## 一句话总结

本文提出 latent-world-first 的数据合成路线与 progress-potential reward shaping (PPRS) 稠密奖励训练配方，构建含 1,170 个可验证环境的 TEV-GYM 长时程智能体训练数据集，训练出的 TEV-27B 在 ALE-CM 上 Pass@5 达到 0.28，匹配 GPT-6 Astra 与 Claude Opus 5，并在 Terminal-Bench 3.0/4.0 子集上超越多个闭源前沿模型。

## 摘要

> 长时程智能体（可自主工作数小时到数天）对自动化发现和递归自我改进（RSI）至关重要，但开源模型仍受困于训练数据缺失和训练配方不成熟两大瓶颈：长而复杂的任务难以人工整理与验证，且奖励稀疏。论文从两方面入手。第一，构建 TEV-GYM，这是首个带可验证稠密渐进奖励的长时程智能体训练数据集，涵盖 7 个领域共 1,170 个已验证任务环境。其核心是 latent-world-first 思路：先构建一个隐藏的、完整且正确的中文任务域规范（含工作流与每步后的期望状态），再从这个规范渐进渲染出面向智能体的任务环境，从而实现对复杂度的可控调节和自然的进度度量。第二，将该度量转化为训练配方：通过 progress-potential reward shaping (PPRS) 把进度转化为稠密的逐步奖励，提升训练效率并使原本不可训练的任务变得可训练。按此配方训练得到 TEV-27B，在 Agents' Last Exam 的 computing 与 math sciences 领域（ALE-CM）上取得显著提升，Pass@5 匹配 GPT-6 Astra 和 Claude Opus 5；尽管只用 ALE 相关数据训练，仍能泛化到 Terminal-Bench (TB) 3.0 和 4.0 子集，超越 GLM 5.2 与 Gemini 3.7 Flash，并在 TB 2.1 这类中等时程任务上达到 Claude Opus 4.8 水平。

Q1: 这篇论文试图解决什么问题？

论文要解决的核心问题是：长时程智能体（long-horizon agents）训练数据的缺失与训练配方的空白。具体拆解为以下层面：
1. 数据层面的结构性缺口：现有开源智能体训练语料（如 SWE-Smith、TMAX、Endless Terminals、CLI-Gym、SETA、Terminal-Universe）规模虽达数千到数万环境，但每个任务只相当于一个 pull-request 规模的修复或一次短终端会话，属于短/中时程；而真正长时程任务只存在于人工编写、仅含几十到几百条实例、每条需专家数周投入的评测基准（如 Agents' Last Exam、Terminal-Bench）中。这些基准只提供终端 pass/fail 或测试通过比例的稀疏信号，最多通过手写 checkpoint 给部分分数，且只作评测集用。因此不存在任何长时程任务的训练语料。
2. 验证与奖励稀疏问题：长任务难以自动验证正确性，即使验证成功，稀疏奖励也导致 RL 训练效率极低。论文给出定量说明：若策略以概率 p 解出任务，则 G 个 rollout 为一组的训练中浪费的组比例为 (1−p)^G——p=0.05、G=8 时浪费 66%，p=0.01 时浪费 92%。对未训练的开源模型，长时程任务的成功率极低，大部分训练组完全无信号，这是长时程 RL 不可训练的根源。
3. 复杂度可控性缺失：现有长任务基准的难度不受控制、彼此结构异构，无法系统生成从易到难的训练谱系；缺少自然的进度度量（progress metric）来支撑稠密奖励。
4. 泛化问题：即便在某个基准上训练，模型能否迁移到不同时程长度、不同领域的任务仍属未知；论文还关心训练是否破坏模型的通用编码能力。
本质上，论文试图回答：能否以可扩展、可验证、可控制难度的方式，工业化地生产长时程智能体训练数据，并把进度信号变成稠密奖励，使开源模型在长时程任务上逼近前沿闭源模型？

Q2: 有哪些相关研究？

论文涉及的相邻工作可按数据、训练配方、评测与基础模型几个维度梳理：
1. 短/中时程智能体训练语料：SWE-Smith (Yang et al., 2025a)、TMAX (Ivison et al., 2026)、Endless Terminals (Gandhi et al., 2026)、CLI-Gym (Lin et al., 2026)、SETA (Shen et al., 2026)、Terminal-Universe (Wu et al., 2026) 等。这些数据集含数千至数万环境，但任务是 PR 级修复或短终端会话，单元测试只给出终端 pass/fail 或测试通过比例，无法支撑长时程训练。
2. 人工长时程评测基准：Agents' Last Exam（Sun et al., 2026）、Terminal-Bench 系列（Merrill et al., 2026; Marten et al., 2026），每条实例需专家数周努力，仅含几十到几百条，只提供手写 checkpoint 的部分分数，且仅为评测而存在。文章对 ALE 和 TB 做了子集划分并验证子集代表性（TB 3.0 P@1 的 Pearson 相关系数 r=0.96，TB 4.0 P@1 的 r=0.97，合理推断来自附录 C.2；证据为语义检索结果）。
3. 长时程能力的闭源前沿模型：文中对标 OpenAI（GPT-6 Astra、GPT-5.6 系列）、Anthropic（Claude Opus 5、Claude Opus 4.8、Claude Sonnet 5、Claude Fable 5）、Moonshot AI（GLM 5.2）、Google（Gemini 3.7/3.6 Flash）、xAI（Grok 4.6/4.5）等闭源模型在 ALE-CM 和 TB 上的表现，显示开源明显落后。
4. RL 算法：训练采用 Qwen3.5-9B 与 Qwen3.8-27B，算法为 DPPO（Qi et al., 2026）——分布式 PPO 变体，论文贡献不在 RL 算法本身，而在奖励塑形方式。
5. 验证型奖励与 latent-world 思想：论文的 latent-world-first 可视为“逆向任务合成”（从正确终态反推任务）与“可控损坏生成”（corruption plan）的组合，与逆向 RL、课程学习、过程奖励模型等有方法血缘，但论文用隐藏规范代替人工标注来实现稠密、可验证的进度信号，这点区别于需要人工过程标注或学出来的奖励模型路线。合理推断：与 LLM-as-judge 类过程奖励相比，TEV-GYM 的进度奖励是精确可验证的，无评估器偏差。
6. 递归自我改进与自动化发现（RSI，AI-for-Science）：论文引言将其列为长时程智能体的核心应用场景，与 open problems in math/science、automated ML research、coding/security systems 相关。

Q3: 论文如何解决这个问题？

论文的技术路线分为数据合成与训练配方两大部分。
（一）数据：latent-world-first 合成管线
1. 先构建 latent world：一个对智能体完全隐藏的、完整且正确的任务域规范，描述领域语义、完整工作流、以及每一步执行完后的期望状态。这个规范是“真值”源头。
2. 从规范正向渲染（render）出面向智能体的任务环境：包括 prompt.md、input/ 目录中的损坏文件（corrupted files）、bash 工具暴露出的环境等。渲染使同一 latent world 能派生复杂度和状态各不相同的多个任务环境。
3. 通过 corruption plan 引入可逆缺陷：在正确环境上施加受控的损坏，形成待修复/待完成的任务初始状态。损坏需可逆，保证任务有解。
4. 精确验证与稠密进度：由于 latent world 已知，每一步的期望状态可计算，因此可对智能体的每一步进度做精确度量，无需人工 step labels 或 LM judge。
（二）数据规模与覆盖：TEV-GYM 含 1,170 个已验证任务环境，覆盖 7 个领域：软件（Software）、机器学习（ML）、运维（Operations）、安全（Security）等（表格可见 All 列拆分），具体还包括数据对账（data reconciliation）、检查点整合（checkpoint consolidation）、系统管理（systems administration）、合成二进制与网络数据包分析（类 CTF）等。每个环境在其公共契约（public contract）中声明工具链。生成过程中有严格的质检门槛：每个生成器需通过 Appendix A.6 的 qualification gates，作者逐一审阅每个规范。
（三）训练配方：PPRS（progress-potential reward shaping）
1. 将进度度量转化为稠密逐步奖励。核心公式为论文 Eq. (4)，给定 reward 定义（语义检索命中但公式细节未完整检索到，具体形式需回原文核对，推测为基于进度变化量的势函数塑形）。
2. PPRS 的关键机制：奖励高潜力状态时不做简单的重复奖励，而是通过 progress potential 计算避免对同一高潜力状态反复给奖而不传播信用。证据片段显示，若无此机制（“errors repeatedly rewards high-potential states without propagating credit”），会导致更多 rollout 在耗尽上下文窗口前无法提交、产生更长轨迹——即训练崩溃模式。该优势在 27B 规模上比稀疏奖励进一步扩大。
3. 训练协议：9B 模型用 TEV-GYM 中 unit difficulty tier 的 264 任务子集（该难度层级更适合 9B 学习），27B 模型用全部语料。DPPO 训练，G=24 个 rollout / 每步 8 个任务，每 rollout 响应预算 65,536 tokens（每 turn 16,384），最多 64 次工具调用，sandbox 只暴露单一 bash 工具。27B 模型在 4 节点 B200 GPU 或 8 节点 A100 GPU 上训练约 4 天。
4. 配方向更大模型的迁移：同一配方应用于 GLM-5.3-Flash 得到 TEV-320B，在 TB 3.0/4.0 子集上评估，验证配方可移植性。
（四）评估设计：ALE-CM（Agents' Last Exam 的 computing+math 拆分）作为 in-domain 评测；TB 3.0、TB 4.0 作为更长时程泛化评测；TB 2.1 作为中等时程评测；另在标准中时程编码基准上检查通用编码能力是否保持（对应 §4.2 第三个问题）。为控制评测成本（每条长时程任务最长 8 小时），对 TB 3.0/4.0 取了“CM”子集（合理推断：computing/math 子集），并验证子集与全集的代表性（r=0.96/0.97）。还对评测集做词级泄漏检查：TEV-GYM 无任何环境与 ALE-CM、TB 3.0、TB 4.0、TB 2.1 的任何任务共享 13-gram 或 8-gram（中位数指标，证据片段在此截断），说明训练环境不是评测任务的改写拷贝。

Q4: 论文做了哪些实验？

论文主实验围绕三个问题展开：TEV-GYM 上的 RL 是否提升开源模型在目标长时程任务上的表现；增益是否泛化到更长时程任务；模型是否保持通用编码能力（§4.2）。§4.3 分别消融 latent-world-first 数据方案与稠密奖励方案。具体实验如下：
1. 主训练与评测：在 Qwen3.5-9B 和 Qwen3.8-27B 上用 DPPO + Eq.(4) 奖励训练，9B 用 264 任务的 unit 难度子集，27B 用全部 1,170 环境。评测集为 ALE-CM（in-domain）、TB 3.0/4.0 子集（更长时程泛化）、TB 2.1（中时程）。
2. 闭源对标：对 ALE-CM 及 TB 各领域列出 Claude Fable 5、Claude Opus 5、GPT-5.6 (sol/terra/luna)、Grok 4.6、Claude Sonnet 5、Claude Opus 4.8、Grok 4.5、GLM 5.2、Gemini 3.7/3.6 Flash 等模型的 P@1 与 P@5。表按 Software/ML/Operations/Security/All 分列。注意检索到的结果表格显示部分闭源模型（如 Gemini 3.6 Flash）在多个领域 P@1=0.00，说明长时程任务对闭源模型也极难。
3. 更大规模配方验证：同一配方应用于 GLM-5.3-Flash 得到 TEV-320B，在 TB 3.0/4.0 子集评估。
4. 消融（§4.3）：(a) latent-world-first 数据侧的消融——需要回原文确认具体是否对比随机/非 latent 合成、不同损坏策略或不同复杂度层级；(b) 稠密奖励方案消融——从检索证据看，对比了 PPRS 与朴素稠密奖励（重复奖励高潜力状态、不传播信用）以及稀疏奖励，观测到朴素稠密奖励导致更长轨迹和上下文耗尽、稀疏奖励在 27B 上差距进一步拉大。
5. 子集代表性验证：对比榜单模型在完整 ALE、TB 3.0、TB 4.0 与对应子集上的表现，报告 Pearson r=0.96（TB 3.0 P@1）与 r=0.97（TB 4.0 P@1）（合理推断：附录 C.2 详述）。
6. 词级泄漏检查：TEV-GYM 与四个评测集之间的 13-gram/8-gram 重叠检验。
7. 成本与基础设施：27B 训练 4 天（4×B200 或 8×A100），单条长时程评测最多 8 小时。附录 C 与 C.2（Table 12/13）补充细节。
训练设置补充：G=24 rollouts、每步 8 任务、65,536 tokens/rollout（16,384/turn）、最多 64 次工具调用、单一 bash 工具 sandbox。

Q5: 发现了什么实验现象？

实验现象与关键观察如下：
1. 训练增益显著：TEV-9B 在 ALE-CM 上 Pass@5 相对 Qwen3.5-9B 提升 83%；TEV-27B 相对 Qwen3.8-27B 提升 65%，达 0.28，匹配 GPT-6 Astra 与 Claude Opus 5（检索证据，具体分数需对照原文表确认，0.28 为 Pass@5 值）。注意 P@1 与 P@5 之间仍有差距，说明多次采样仍能获得显著收益，模型单次尝试的稳健性弱于前沿模型。
2. 跨基准泛化：仅用 ALE 相关数据训练，TEV-27B 在 TB 3.0 和 4.0 子集上超越 GLM 5.2 和 Gemini 3.7 Flash；在 TB 2.1 中时程任务上达 Claude Opus 4.8 水平。这是训练数据与评测基准不同构、不同时程的强泛化证据。
3. 基础模型规模效应：从 9B 到 27B，TEV-GYM 的收益放大；且配方迁移到 320B 规模（GLM-5.3-Flash → TEV-320B）依然有效，说明 TEV-GYM 数据质量高但挑战大，需要足够大的基础模型才能充分受益（检索到的 method 片段明确说“更强的基础模型才能充分受益于高质量但有挑战性的 TEV-GYM”）。
4. 稠密奖励机制的反直觉发现：朴素稠密奖励（反复奖励高潜力状态但不传播信用）反而有害——导致更多 rollout 在提交前耗尽上下文窗口、轨迹变长，即“奖励黑客”式的停滞/无限探索行为；PPRS 通过 progress-potential 修正后行为正常化。该现象说明稠密奖励不是越多越好，奖励塑形的信用传播设计决定训练稳定性。稀疏奖励在 9B 上已有劣势，在 27B 上劣势进一步扩大——模型越强、稀疏成功信号越稀少，反而更难训练。
5. 闭源模型在长时程任务上也远未饱和：多个前沿模型在某些领域 P@1 为 0.00（如 Gemini 3.6 Flash 在 Software/ML/Operations 的多个 P@1 为 0），长时程仍是全体模型共同的短板。GLM 5.2 在 Operations/Security 上标注星号且数值极低。
6. 评测子集代表性良好：榜单模型在子集与全集的 P@1 高度相关（r=0.96/0.97），支持用子集做低成本评测。
7. 负结果与边界：论文承认最难的基准上，TEV 模型与领先闭源前沿体仍有差距（结论明示），即长时程任务的天花板尚未被开源模型触及。
8. 训练-评测时间不对称：单条长时程评测最多 8 小时，训练 4 天，评测成本是实验设计的核心约束之一；这也是他们做子集和泄漏检查的原因。
9. 需要留意：检索证据中出现了“Claude 参与判断 TB 3.0 定性数据分析”的说明（limitations 片段），说明部分定性分析环节使用了闭源模型辅助，这在解读“作者主观评估”的环节需注意。

Q6: 有什么可以进一步探索的点？

论文自己指出的方向与可进一步探索的空间：
1. 简化环境生成流程：当前支持新工作流需要人工撰写并验证额外的环境组件（每个生成器需过 qualification gates、规范需作者逐条审阅），未来工作可自动化/简化此过程，降低扩展到新领域的边际成本。
2. 扩展领域覆盖：当前聚焦合成终端环境（synthetic terminal environments），7 个领域的覆盖面仍有限；可扩展到更多专业工作流（数据对账、检查点整合、系统管理、CTF 式二进制/网络分析之外的新领域）。
3. 与更强开源模型结合：把 TEV-GYM 与更强开源基础模型（如更大规模的 Qwen、GLM 后续版本）组合，使更多专业工作流可用于可复现的 RL，且无需人工 step labels 或 LM judge。
4. 训练配方的机制深化：PPRS 的 progress-potential 计算细节、与 DPPO 的结合方式、对更长上下文/更多工具调用的扩展，都值得进一步研究；朴素稠密奖励导致上下文耗尽的机制可作为独立研究问题。
5. 从终端环境走向多模态/图形界面/物理世界环境：latent-world-first 的抽象是否适用于 GUI agent、机器人操作等非纯文本环境，是一个开放问题（合理推断：论文只验证了 bash/终端界面）。
6. 评测集扩展与成本控制：长时程评测每条最多 8 小时，子集方法（Pearson r=0.96/0.97）可在未来基准设计中被采纳；但子集划分的领域均衡性、规模与代表性上限仍需研究。
7. 课程/难度层级的系统化：9B 用 unit 难度子集、27B 用全语料，说明难度与模型规模的匹配是训练效率关键；如何在训练过程中动态调整难度（自课程）是自然延伸。
8. 安全性维度：长时程任务含 Security/CTF 类环境，训练出的更强智能体若具备数小时自主能力，其安全对齐、沙箱隔离、越狱鲁棒性需要专门研究——虽然论文未展开，但这与 RSI 方向直接相关。
9. RSI 与 AI-for-Science 的连接：论文引言把自动发现与递归自我改进作为动机，但实验尚未覆盖真正的科学发现任务（如开放数学/科学问题求解的端到端闭环），这是后续可在 ALE 之外验证的方向。

Q7: 总结一下论文的主要内容

论文试图同时回答长时程智能体的两个瓶颈——训练数据缺失和训练配方缺失——并给出了端到端方案：从隐藏真值反推任务合成的数据管线（latent-world-first）+ 把进度转化为势函数稠密奖励的训练配方（PPRS）。
论证主线：引言从 RSI/自动发现引出长时程智能体的战略价值，指出开源数据只有短/中时程规模（PR 修复、短终端会话）、评测基准只有少量人工实例且只能给稀疏信号，因此“不存在长时程训练语料”是核心空白。随后用 (1−p)^G 的量化论证说明稀疏奖励下长时程 RL 的训练浪费：p=0.05 与 G=8 时 66% 的组无效，p=0.01 时 92% 无效——这是“长时程任务不可训练”的机理层解释。技术主线：先构建 latent world（完整且正确的隐藏规范，含领域、工作流与每步期望状态），再施加 corruption plan 生成可逆缺陷，渲染出面向智能体的环境（prompt.md、损坏的 input/、bash 工具）。由于真值已知，环境可精确验证、复杂度可控、进度可度量。TEV-GYM 由此产出 1,170 个已验证环境、7 个领域（Software、ML、Operations、Security 等，含数据对账、检查点整合、系统管理、合成二进制/网络包 CTF 等任务类型），每个环境声明工具链契约，生成器有严格质检门槛。训练主线：把进度度量经 PPRS 变成稠密逐步奖励（Eq. 4），避免朴素稠密奖励的“反复奖励高潜力状态却不传播信用”导致的上下文耗尽/长轨迹崩溃，也避免稀疏奖励的低效；9B 用 unit 难度 264 任务子集、27B 用全语料，DPPO 训练（G=24、65,536 tokens/rollout、最多 64 工具调用、单 bash sandbox），27B 约 4 天训练完成。实验主线：主实验回答三个问题——(1) in-domain 提升：TEV-9B 在 ALE-CM Pass@5 提升 83%，TEV-27B 提升 65% 到 0.28，匹配 GPT-6 Astra 与 Claude Opus 5；(2) 泛化：仅用 ALE 相关数据训练的 TEV-27B 在 TB 3.0/4.0 子集超越 GLM 5.2 与 Gemini 3.7 Flash，在 TB 2.1 上达 Claude Opus 4.8 水平；配方迁移到 GLM-5.3-Flash 得 TEV-320B 依然有效；(3) 通用能力保持：标准中时程基准上不退化（具体数值未在检索证据中出现，需回原文核对 §4.2）。消融实验分离验证 latent-world-first 数据与 PPRS 稠密奖励各自的贡献。评测可信度设计：TB 3.0/4.0 子集经 Pearson 相关性（r=0.96/0.97）验证代表性；词级 13-gram/8-gram 无重叠证明训练环境非评测任务的改写。结论与局限：方案聚焦合成终端环境；最难基准上仍与前沿闭源有差距；支持新工作流需要人工撰写验证环境组件；未来工作是简化生成流程、扩展领域、与更强开源模型结合，实现无需人工 step labels 或 LM judge 的可复现长时程 RL。
整体判断：这篇论文的价值在于把“长时程智能体训练”从不可规模化的手工作坊式（人工基准 + 稀疏奖励）转化为可规模化的工业化管线（隐藏真值 + 可控损坏 + 稠密验证奖励），并且用跨基准泛化和跨模型规模迁移证明了配方的稳健性。局限在于环境形态单一（文本终端/bash）、领域广度有限、以及最硬基准上仍无法追平闭源前沿；消融细节、Eq.(4) 的具体表达式、TB 2.1/中时程基准的完整数值和标准编码基准结果在检索证据中缺失，需要翻原文 §4.2/§4.3 与附录确认。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中 agent 方向（权重 0.10）的直接相关点：论文是长时程智能体训练数据与奖励设计的系统性工作，可直接用于设计自主智能体的训练管道与奖励函数；PPRS 的信用传播问题对所有 agent RL 训练有普适警示。

## 基本信息

- 作者：Wenbo Hu, Shraman Pramanick, Krishna Kumar Singh, Nanyun Peng, Kai-Wei Chang, Yong Jae Lee
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.CV, cs.LG
- 日期：2026-10-13
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.18868v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（含摘要、Introduction、Method、Results、Conclusion 的检索命中片段），结合 heuristic_draft 与论文元数据完成；对检索证据未覆盖的公式细节、部分实验数值和附录内容已标注为需回原文核对，未编造具体数字。
