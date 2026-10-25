---
user_id: "cheng tan"
paper_id: 14463
arxiv_id: "2609.37950v1"
title: "Video-RSI: Recursive Self-Improvement of Video Understanding Agents via Harness Evolution"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Sept 2026/2609.37950v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.37950v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-12T13:04:09"
---
# Video-RSI: Recursive Self-Improvement of Video Understanding Agents via Harness Evolution

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：video understanding agent · harness evolution · recursive self-improvement · active video investigation

## 一句话总结

Video-RSI 提出一种视频理解智能体的递归自改进框架：让同一个冻结权重的语言模型既当 Solver 又当 Editor，通过重访原始训练视频、主动检验失败解释，并在同时考虑答案准确率与视觉帧成本的选择规则下，将诊断结果转化为可复用的 harness 代码修订，从而在不修改模型权重的前提下提升视频理解精度并降低帧开销。

## 摘要

> Video understanding agents acquire evidence through an executable harness that controls what they observe and how they use those observations. However, execution traces contain only the evidence acquired by the current harness, leaving competing explanations for failure unresolved and limiting the basis for self-improvement. We introduce Video-RSI, a framework for recursive self-improvement in which a video understanding agent uses its own language model to revise its harness. Through active video investigation, the model revisits the original training videos to test competing failure explanations with additional observations, grounding proposed changes in evidence beyond the existing trace. Cost-aware harness evolution turns these diagnoses into reusable revisions and determines which revisions to retain by considering both answer accuracy and visual cost. Across our evaluation settings on video understanding benchmarks, the evolved agent improves accuracy while processing fewer frames and achieves competitive accuracy-efficiency trade-offs against existing video understanding agents. These results demonstrate the potential for video understanding agents to improve their own evidence acquisition and use through harness evolution. Code is available at https://github.com/bingjunluo/Video-RSI .

Q1: 这篇论文试图解决什么问题？

1. 视频理解智能体面临的核心问题是：在长视频中，相关事件可能很短暂、在时间上相距很远、或周围环绕着视觉上相似的干扰，智能体必须先决定观察什么，才能决定如何回答。
2. 现有智能体通过迭代检索、时间导航和工具引导的观察来应对长视频理解，但这些行为受限于一个可执行的 harness，它控制证据采集、处理观察并支撑后续推理；harness 同时决定了模型看到什么和如何使用证据，并将答案准确率与视觉观察成本挂钩，因此 harness 是提升视频理解的一个机会点。
3. 现有执行轨迹只包含当前 harness 获取的证据，当智能体答错时，轨迹本身无法区分相互竞争的失败解释（例如：是模型漏看了某个事件，还是看到了但没有正确表征和使用证据）。这种证据不足限制了自我改进的基础。
4. 已有自动化智能体设计工作用反馈改进智能体程序，也有工作将代码级进化扩展到视频理解，Self-Harness 展示了冻结模型可以在终端环境中修订自己的操作 harness；但如何让视频理解智能体在 open-vocabulary、长视频、视觉成本显著的场景下可靠地自我改进 harness，仍缺乏系统性研究。
5. 更具体地，论文要解决的是 Editor 在离线 code-level search 设置中如何获取修改证据的问题：仅依赖执行轨迹不足以判断失败原因，需要主动调查原始视频来检验失败假设，然后再编辑代码；同时，选择修订时必须在答案质量与视觉使用之间做显式权衡，而不是只追求准确率。
6. 隐含难点还包括：调查后的诊断可能指向“事件未被覆盖”或“事件已被观察但表征/使用不当”，这两种情况需要不同的 harness 修订；修订过程的视觉成本本身可能抵消帧数节省，需要 cost-aware 的选择规则。合理推断：作者认为 harness 自我改进是“不改变模型权重即可提升视频理解”的一条实际可行路线。

Q2: 有哪些相关研究？

1. 视频理解智能体（Video Understanding Agents）：通过迭代检索、时间导航和工具引导观察来解决长视频中的证据获取问题，相关代表工作包括 Wang et al. (2024)、Zhang et al. (2025b)、Lin et al. (2026) 等；VideoSeek（Lin et al., 2026）是论文复现并作为 baseline 的智能体方法之一。
2. 长视频理解基准：Video-MME（Fu et al., 2025）的长视频 split、LongVideoBench（Wu et al., 2024）、EgoSchema（Mangalam et al., 2023）官方公开子集、MLVU test（Zhou et al., 2025），以及用于进化的 LVBench（Wang et al., 2025a）；这些基准覆盖长视频、字幕、时序关系和多模态理解等不同难度。
3. 自动化智能体设计（Automated Agent Design）：Hu et al. (2025)、Lee et al. (2026) 等工作使用反馈信号改进智能体程序，属于 code-level agent program 进化的路线；Xu & Chen (2026) 的 VideoHarness-RSI 将代码级进化扩展到视频理解，是论文最直接的对标方法之一。
4. Harness 自我改进：Zhang et al. (2026a) 的 Self-Harness 证明冻结模型可以在终端环境中修订自己的操作 harness；Video-RSI 将该思想迁移到视频理解，并强调了主动视频调查为修改提供超出轨迹的证据。
5. 失败解释与证据获取：论文强调执行轨迹只包含当前 harness 收集的证据，无法解决相互竞争的失败解释；这与可解释性、trace-based debugging 和主动查询（active querying）思路相关——模型需要额外观察来验证失败假设。
6. 多模态大模型（MLLM）baseline：论文对比了通用的 MLLM 基线，代表当前视频问答的强基线；同时对比视频智能体模型和 harness 自改进方法，以区分“更强的模型/智能体”与“更好的证据获取层”带来的增益。
7. 成本感知的智能体设计：论文的选择规则显式考虑视觉成本，这与 agent efficiency、frame budget、token budget 等研究方向一致；推测与 cost-sensitive tool use、resource-constrained agent 设计有关。
8. 值得注意：论文中引用的部分文献（Zhang et al. 2025b, Lin et al. 2026, Hu et al. 2025, Lee et al. 2026, Xu & Chen 2026, Zhang et al. 2026a）带有未来年份或近期编号，属于 arXiv 预印本生态中的快速迭代路线；这些工作的具体细节需要回原文参考文献核对。

Q3: 论文如何解决这个问题？

1. 总体框架：Video-RSI 采用“同一个冻结语言模型同时担任 Solver 与 Editor”的递归自改进框架。Solver 负责在给定 harness 下回答视频问题；Editor 负责分析失败、诊断原因并提出 harness 代码修订；模型权重全程冻结。
2. Harness 定义：可执行的 harness 控制证据采集（看哪些帧、用什么工具）、观察处理（如何整理视觉、字幕、OCR 信息）和推理支持（记忆、检索、图查询等）。初始 harness HS0 支持全局与局部视频检查、transcript、OCR 和 observation memory。
3. 进化协议：在每一轮尝试 t，Solver 在训练问题上执行当前 harness Ht 并产生执行轨迹；Editor 检查轨迹、发现失败，并重访原始视频来检验失败解释（active investigation），然后提出候选修订 harness eHt。
4. 主动调查（Active Video Investigation）：调查的核心是区分两类失败解释——(a) 事件未被覆盖（temporal coverage 问题，即模型根本没看到相关事件），此时诊断支持修订时间覆盖；(b) 事件已被观察但表征或使用不当（representation/usage 问题），此时诊断转而检查证据如何被表示和使用。调查以得到一个有证据支撑的修订假设为终点，或返回证据缺口以指导进一步检查。
5. 候选选择：独立私有选择集上进行候选评估，选择规则同时考虑答案准确率和视觉成本（Eq. (3)，α=0.1, β=0.2, ϵ=1/|Dg|），允许 cost-reduction 分支至多减少一个正确答案。Development examples 和 per-question outcomes 保持私有，Editor 只接收 aggregate 反馈。
6. 与 trajectory-only 变体的对照：active-investigation 变体和 trajectory-only 变体共享训练样本、标注访问、初始化、模型和每轮资源限制，唯一区别是 pre-edit 阶段能否访问新的视频观察，从而隔离主动调查的贡献。
7. 成本感知的保留机制：cost-aware harness evolution 将诊断转化为可复用的修订，并决定保留哪些修订——不仅要准确率更高，还要考虑视觉帧成本，使“更丰富的观察工具”与“更少的帧数”可以共存。
8. 评估协议：用 216 个 LVBench 问题做调查与修订，72 个视频不重叠的问题作为固定 development set 用于候选选择；跑 20 轮修订尝试，最终被接受的 harness 冻结用于主评估，中间被接受的版本也用于分析进化动态。

Q4: 论文做了哪些实验？

1. 基准与数据划分：在 Video-MME 长视频 split（带字幕）、LongVideoBench（带字幕）、EgoSchema 官方公开子集和 MLVU test 上评估；进化阶段使用 216 个 LVBench 问题，另用 72 个与训练视频不重叠的问题作为固定 development set 进行候选选择。
2. 模型与进化协议：Solver 和 Editor 均使用 DeepSeek-V4-Pro，视觉观察使用 Qwen3.6-Plus；所有模型权重冻结；初始 harness HS0 支持全局/局部视频检查、transcript、OCR 和观察记忆；共进行 20 轮修订尝试，选择规则参数 α=0.1、β=0.2、ϵ=1/|Dg|，cost-reduction 分支允许至多减少一个正确答案；最终接受的 harness 冻结用于主评估，中间接受的版本用于分析进化动态。
3. 对比基线：包括 MLLM（通用多模态大模型）、视频智能体模型（video agentic models）和 harness 自改进方法；论文用与 Video-RSI 相同的 DeepSeek-V4-Pro 复现了 VideoSeek（Lin et al., 2026）和 VideoHarness-RSI（Xu & Chen, 2026），其余基线结果引用原文。
4. 消融与变体实验：active-investigation 与 trajectory-only 变体共享训练样本、标注访问、初始化、模型和每轮资源限制，仅 pre-edit 阶段是否有新的视频观察可访问不同，用于隔离主动调查的贡献；文中还提到对 cost-aware selection 的消融。
5. 进化动态分析：评估中间被接受的 harness 版本，分析各轮修订带来的准确率与帧数变化；例如 MLVU 上最后两个被接受的修订贡献了大部分净准确率提升，说明最终 harness 是累积式精炼而非仅靠早期效率提升。
6. 保留代码分析：对被接受的 harness 代码进行分析，识别出结构化感知（structured perception）、证据复用（evidence reuse）和选择性观察（selective observation）三类变化模式。
7. 效率-准确率权衡对比：报告各评估设置下准确率与视觉使用量的权衡，与 VideoHarness-RSI 相比在每一个基准上以更少帧数取得更高准确率，最大准确率增益出现在 Video-MME；MLVU 上增益达 8.2 个百分点且推理时帧使用量相近。
8. 主结果格式：由于部分结果表格未在提供的片段中完整呈现，具体数值（各基准准确率、帧数、所有基线的完整表格）需要回论文正文核对。

Q5: 发现了什么实验现象？

1. 核心现象：进化后的 harness 在四个基准上都比初始 harness HS0 提高了准确率，同时使用的帧数减少了 10.8%–44.1%，说明“更丰富的观察工具”与“更低的视觉成本”可以共存，而不是简单的准确率-成本折中。
2. 与 VideoHarness-RSI 的系统性对比：Video-RSI 在每一个基准上都以更少帧数获得更高准确率；准确率增益在 Video-MME 上最大。这支持“主动调查提供的离线证据”在 harness 进化中是有价值的。
3. MLVU 的特殊模式：MLVU 上增益达 8.2 个百分点，但推理时帧使用量与对比方法相近；其他三个基准上更高准确率伴随更少帧数。说明不同基准上准确率提升的来源可能不同（有的靠更好的证据选择，有的靠更好的证据使用）。
4. 进化动态：MLVU 上净准确率增益的大部分来自最后两个被接受的修订，说明进化不是一步到位的，而是累积式精炼；早期修订可能主要是效率改进，后期修订贡献准确率。
5. 消融对比：active-investigation 与 trajectory-only 的对照中，两者共享除 pre-edit 视频观察访问外的一切条件，结果支持主动调查的贡献（具体数值未在片段中给出，需核对）；cost-aware selection 的消融也被提及作为贡献证据，但具体数值需要回原文确认。
6. 保留代码的模式：分析显示进化后的 harness 出现了结构化感知（对视觉、transcript、OCR 累积观察建立关系和结构化表示）、证据复用（eligible relational/temporal 问题返回证据缺口以指导进一步检查）、选择性观察（图查询本身不处理新帧）等模式；这解释了为什么更丰富工具可以与更少帧数共存。
7. 反直觉点：通常增加工具/观察能力会增加成本，但这里 richer observation tools 反而伴随更低帧数，说明 harness 进化的价值在于让智能体更聪明地选择观察，而不是观察更多。
8. 注意：提供的证据片段中未包含失败案例的详细分析、各基线的逐项数值表格和方差/显著性信息，这些需要回论文正文核对；谨慎对待任何未在片段中出现的具体数字。

Q6: 有什么可以进一步探索的点？

1. 扩展到更多视频理解任务与基准：当前评估集中在 Video-MME、LongVideoBench、EgoSchema、MLVU，可以探索更长视频（如 hour-long、day-long 场景）、流式视频、开放域视频问答和视频 grounding 任务，检验 harness 进化的泛化边界。
2. 跨领域迁移与复用：被接受的 harness 修订是在 LVBench 训练问题上进化出来的，是否可以迁移到未见过的视频域、语言和任务类型？是否可以积累一个“harness 进化库”，让新任务从已有修订开始而不是从头进化？
3. 进化效率与计算成本：20 轮修订、每轮在训练集上执行 Solver 并重访视频调查，离线计算成本可能很高；可以探索更高效的调查策略（如主动学习选择值得调查的问题）、更少的修订轮次、以及如何用预算约束控制进化成本。
4. 调查策略的深化：当前调查区分“事件未覆盖”与“事件已观察但表征/使用不当”；可以进一步自动化地生成并验证更细粒度的失败假设（如时序关系错误、空间位置错误、字幕与视觉冲突），甚至用形式化方法验证 harness 行为。
5. 联合优化 Solver 与 Harness：当前模型权重冻结，只进化 harness；可以探索在允许微调或 LoRA 的场景下，harness 进化与模型权重更新的协同作用，以及 harness 进化能否为训练数据选择提供信号。
6. 成本模型的改进：当前选择规则用 α、β、ϵ 等参数平衡准确率与视觉成本；可以学习更精细的成本模型（如按工具类型、帧分辨率、token 数区分成本），或让选择规则自适应不同基准/任务。
7. 可解释性与安全：进化后的 harness 代码可能出现难以解释的复杂行为；可以研究 harness 行为的可解释性、可验证性，以及在错误方向进化时的安全机制（如回滚、约束、人类审核接口）。
8. 与其他自我改进范式的结合：与 Self-Harness（终端环境）、自动 agent 设计（Hu et al. 2025, Lee et al. 2026）和代码级进化（VideoHarness-RSI）结合，探索 memory、反思、多智能体协作等机制的互补性。
9. 多模态与长上下文扩展：当前使用 DeepSeek-V4-Pro 作 Solver/Editor、Qwen3.6-Plus 作视觉观察；可以探索更强视觉模型、多视觉模型集成，以及将 harness 进化应用于音频、点云、具身感知等其他模态。
10. 现实部署问题：视频理解智能体的实际应用中，视觉成本、时延、隐私（重访训练视频可能需要数据访问权限）等问题值得研究；例如在无法访问训练视频的部署场景下，如何用代理数据集或合成视频进行 active investigation。
11. 论文自身提到的边界：结论中只总结了“harness evolution as a practical route”，未讨论失败案例和失败模式；合理推断作者认为框架仍有改进空间，但具体 open problems 需要结合正文 discussion 部分确认（该部分在本次材料中缺失）。

Q7: 总结一下论文的主要内容

Video-RSI 研究的问题是：视频理解智能体如何在不修改模型权重的前提下，改进自己获取证据的方式。论文的核心观察是，视频理解智能体的行为由可执行的 harness 支配——harness 控制智能体观察什么（哪些帧、哪些工具）、如何处理观察（transcript、OCR、记忆）以及如何使用证据推理；因此 harness 的质量直接决定答案准确率和视觉成本。作者指出，仅依赖执行轨迹进行自我改进存在根本缺陷：轨迹只包含当前 harness 所获取的证据，无法区分相互竞争的失败解释——比如“模型根本没有看到关键事件”和“看到了但表征/使用不当”，这两种失败需要完全不同的修复。

针对这个问题，Video-RSI 提出一个递归自改进框架：同一个冻结权重的语言模型同时担任 Solver（回答视频问题）和 Editor（诊断失败、修订 harness）。每轮进化中，Solver 在训练问题（216 个 LVBench 问题）上执行当前 harness 并产生轨迹；Editor 检查轨迹、发现失败后，主动重访原始训练视频来检验失败解释——如果调查发现事件未被覆盖，则修订时间覆盖；如果事件已被观察但使用不当，则检查证据的表征和使用。这种 active video investigation 为代码修改提供了超出轨迹的证据基础。随后 Editor 提出候选修订，并在一个视频不重叠的私有 development set（72 个问题）上进行选择；cost-aware 选择规则（Eq. 3，α=0.1, β=0.2, ϵ=1/|Dg|）同时考虑答案准确率和视觉成本，允许 cost-reduction 分支至多减少一个正确答案。整个进化过程进行 20 轮修订尝试，最终被接受的 harness 冻结用于主评估。

实验在四个长视频理解基准（Video-MME、LongVideoBench、EgoSchema、MLVU）上进行，对比 MLLM、视频智能体模型和 harness 自改进方法；作者用相同模型复现了 VideoSeek 和 VideoHarness-RSI。主要结果显示：进化后的 harness 在四个基准上准确率提升的同时帧数减少 10.8%–44.1%；与 VideoHarness-RSI 相比，在每个基准上以更少帧数获得更高准确率，MLVU 上增益达 8.2 个百分点且帧使用相近，最大准确率增益出现在 Video-MME。消融实验隔离了 active investigation 的贡献（与 trajectory-only 变体共享除 pre-edit 视频观察访问外的一切条件）和 cost-aware selection 的贡献。进化动态分析显示 MLVU 上大部分净增益来自最后两个被接受的修订，说明累积式精炼的重要性。对保留代码的分析识别出结构化感知、证据复用和选择性观察三类变化模式，解释了更丰富工具与更低帧数共存的原因。

结论上，论文主张 harness evolution 是提升冻结模型视频智能体证据获取与使用能力的实际可行路线。需要指出的是，本次材料的正文不完整（method 和 discussion 部分为空，部分结果数值未出现在检索片段中），因此部分实验细节（如各基准的具体准确率数值、所有基线的完整表格、失败案例分析、与 MLLM 基线的逐项对比）需要回原文核对。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与你画像中的 agent 方向（权重 0.10）直接相关：论文研究的是视频理解智能体通过 harness 进化实现自改进，属于 test-time agent self-improvement 路线。

## 基本信息

- 作者：Bingjun Luo, Jialin Guo, Siqi Li
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-09-29
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.37950v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（introduction/results/conclusion 共 7 条命中片段）并结合启发式草稿进行补全；由于原文 method 与 discussion 部分缺失，部分实验细节标注为需回原文核对。
