---
user_id: "cheng tan"
paper_id: 6956
arxiv_id: "2608.05891v1"
title: "AppDeltaWorld: Transition-Grounded Delta Code World Model for Mobile GUI Agents"
publish_date: "2026-08-06"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.05891v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.05891v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-08T14:01:05"
---
# AppDeltaWorld: Transition-Grounded Delta Code World Model for Mobile GUI Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：world model · mobile gui agent · delta code generation · html retrieval

## 一句话总结

AppDeltaWorld 提出一种以转移为约束的增量代码世界模型，将移动 GUI 下一状态预测构建为可达的代码更新（层级 HTML 检索 + 可执行 HTML 生成 + 视觉资产合成），在保真度上优于图像/代码基线，并能为 GUI Agent 提供闭环监督数据与测试时强化学习，从而缓解真实轨迹缺失问题。

## 摘要

> Mobile GUI agents can operate apps through pixel perception and touch actions, making them a promising interface for collecting and improving long-horizon mobile interaction policies. However, real trajectories are difficult to obtain for sensitive apps and privacy-critical operations. At the same time, existing simulated environments are costly to scale up, and GUI world models still suffer from unstable generation, limited modality coverage, and inconsistent action-transition logic. To address these limitations, we propose AppDeltaWorld, a transition-grounded delta code world model that predicts the next GUI as a reachable code update rather than as an unconstrained image or text description. AppDeltaWorld retrieves app-specific Level-1 HTML references under an action-transition constraint, generates Level-2 executable HTML conditioned on the current screen, action, predicted next-screen text, and retrieved structure, and inserts generated visual assets into image slots before browser rendering. As a world model, AppDeltaWorld achieves the highest fidelity on CMGUIBench-500 under Code2World evaluation, with clear gains in structural layout and UI element reconstruction over image-only and code-only baselines. As a training environment, AppDeltaWorld supports filtered closed-loop SFT data construction that, when combined with public supervision, enables AppDeltaAgent to achieve state-of-the-art performance on AndroidLens and consistent gains on MobileGym and MobileWorld. Moreover, world-model-based test-time reinforcement learning enables policy adaptation and shows further improvements without additional interaction with real apps.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决移动 GUI Agent 数据获取与仿真环境可扩展性之间的根本矛盾，以及现有 GUI 世界模型在实用化中的三重缺陷。具体问题拆解如下：

1. 真实轨迹获取受限：敏感应用（如银行、健康、支付）和隐私关键操作往往缺乏真实交互轨迹，导致长程移动交互策略难以监督。API/CLI 型 Agent 只能通过应用暴露的接口行动，受平台授权和权限边界约束，无法像 GUI Agent 一样普适操作异构应用。
2. 模拟环境扩展代价高：已有工作通过构建可执行模拟环境（Tang et al. 2026; Dong et al. 2026）提供合成监督，但扩展到广泛应用覆盖需要大量人工努力实现应用特定功能，且随着移动界面演化维护昂贵。
3. GUI 世界模型不满足三个关键需求：
 - 稳定保真度（Stable Fidelity）：以图像或文本形式直接生成下一屏容易产生不稳定生成、结构畸变和视觉细节丢失（如图1上方所示）。
 - 模态覆盖（Modality Coverage）：仅图像或仅代码建模都会丢失另一侧信息；真实 GUI 同时包含视觉外观和可交互结构代码，需要覆盖两者。
 - 动作转移逻辑一致性（Action-Transition Logic Consistency）：预测的状态转移必须遵循真实应用中的交互逻辑，即世界模型面对无效动作时应表达拒绝或低置信度，而不是无约束地产生幻觉状态。

因此，论文将问题设定为：如何在无真实轨迹且无昂贵环境工程的前提下，训练一个既具备高视觉/结构保真、又能保证动作-转移逻辑一致性的 GUI 世界模型，并利用该世界模型为 GUI Agent 生成可用的策略监督信号。

需要注意的是，检索证据中关于“valid or low-confidence transitions rather than unconstrained hallucinated states”的片段明确呼应了第三点；对世界模型的评估不仅关心视觉真实感，还关心转移的合法性判定。

Q2: 有哪些相关研究？

与本文直接相关的研究脉络有三支：

1. 移动 GUI Agent 与交互范式：多模态基础模型的进展催生了一批基于感知和行动的移动 GUI Agent（Lu et al. 2026; Yang et al. 2026; Qin et al. 2025）。与 API/CLI 代理（Xiao et al. 2026）不同，GUI Agent 直接模拟人类触摸交互，适用性广，但面临数据瓶颈。代表性数据集和基准有 AndroidLens（Cao et al. 2026a，长时延嵌套子目标评估）、MobileGym、MobileWorld 等，这些被本文用作策略评估环境。
2. 模拟环境与可执行仿真：Tang et al. 2026、Dong et al. 2026 等构建可执行模拟环境，提供合成交互数据，但人工维护成本高，难以快速覆盖新应用或界面更新。本文认为模拟环境“expansive to scale up”。
3. GUI 世界模型：世界模型通过观测和动作预测未来状态，近期视觉语言模型被用于 GUI 自动化（Gu et al. 2024; Chae et al. 2024; Guan et al. 2026; Jiang et al. 2026; Sun et al. 2024）。现有 GUI 世界模型或直接生成图像（如 Mobile-Dreamer，Cao et al. 2026b，生成式草图世界模型），或生成 UI 代码（如 Code2World、gWorld），使用符号中间表示渲染截图。这些方法的问题包括：图像生成不稳定、代码生成缺乏视觉细节、缺乏动作转移逻辑约束。此外，还有工作研究如何用移动世界模型指导 GUI Agent（Xu et al. 2026a）以及多轮强化学习（如 Advancing gui agent with multi-turn reinforcement learning, Wang et al. 2024 的 Mobile-agent-v2 等）。

本文的定位是：将“delta code”思想引入世界模型——预测的是相对当前可执行代码的增量更新，而不是从零生成整个屏幕或自由文本。这样既保留了代码的可执行性和结构信息，又通过“转移约束”保证动作逻辑一致性。

Q3: 论文如何解决这个问题？

AppDeltaWorld 的核心方法可以概括为“转移锚定的增量代码世界模型”，将下一屏 GUI 表示为从当前状态可达的代码更新。其技术路线包含四个关键组件：

1. 动作约束的层级 HTML 检索（action-constrained hierarchical HTML retrieval）：
 - 维护应用特定的 Level-1 HTML 参考库，作为描述控件布局和语义结构的模板或骨架。
 - 检索过程受“动作-转移”约束，即检索结果必须与当前动作和当前屏幕状态相容；对无效动作，检索应返回低置信度或拒绝，从而避免无约束幻觉。
 - 层级设计：Level-1 为结构参考，Level-2 为可执行的具体实现，类似先检索模板再实例化。

2. 可执行下一屏生成（executable next-screen generation）：
 - 以当前屏幕（图像或状态表示）、动作、预测的下一屏文本、以及检索到的结构为条件，生成 Level-2 可执行 HTML。
 - 生成的 HTML 是可直接交由浏览器渲染的可执行代码，而非自由文本描述，因此保证结构完整性和模态覆盖（代码与视觉共存）。

3. 视觉资产生成与插入（visual-asset synthesis and insertion）：
 - 由于 HTML 本身不能表示所有视觉细节（如图标、图片、纹理），模型额外生成视觉资产（图片、图标等），并将其插入到 HTML 中的图像槽位，再交给浏览器渲染。
 - 这一步弥补了“代码-only”方法在视觉外观上的不足，同时避免“图像-only”方法的结构不可控。

4. 世界模型在环的 rollout 构建（world-model-in-the-loop rollout construction）：
 - 利用世界模型模拟多步交互轨迹，生成闭环数据。
 - 对生成轨迹进行过滤（filtered closed-loop SFT），只保留高置信度、动作转移合法的样本，避免噪声传播。
 - 将这些过滤后的数据与公开真实监督数据结合，用于训练下游 Agent（AppDeltaAgent）。

此外，论文还提出基于世界模型的测试时强化学习（test-time RL）：在测试阶段，Agent 与世界模型交互获得多种未来轨迹的回报信号，更新自身策略，从而无需与真实应用交互即可适应新场景。

总体而言，AppDeltaWorld 的创新在于把“生成下一屏”从“无约束生成”转化为“受动作转移约束的代码增量更新”，并利用层级检索降低了生成难度、提高了可解释性。

Q4: 论文做了哪些实验？

论文设计了双轨实验：一是评估 AppDeltaWorld 作为世界模型本身的生成保真度；二是评估其作为训练环境对下游 Agent 能力的提升。

1. 世界模型评估（world modeling evaluation）：
 - 基准：CMGUIBench-500，即从原始 CMGUI benchmark（Xie et al. 2026）中随机取 500 个样本，以控制评估规模。
 - 评估协议：采用 Code2World 评估方式，即生成代码并用浏览器渲染成截图，再与真实截图比较。
 - 对比基线：图像-only 生成方法、代码-only 生成方法（如 gWorld 等），以及现有世界模型。
 - 评估指标（合理推断）：结构布局（如控件位置、尺寸、布局树相似度）、UI 元素重建（如控件类别、文本、组件识别）以及视觉外观（如图像相似度）等。论文强调“clear gains in structural layout and UI element reconstruction”。

2. 动作策略评估（action-policy evaluation）：
 - 数据集：AndroidLens、MobileGym、MobileWorld 三个移动 GUI 基准。
 - 训练方式：使用 AppDeltaWorld 生成过滤后的闭环 SFT 数据，与公开监督数据混合，训练 AppDeltaAgent。
 - 测试方式：静态动作预测（static action prediction）以及可能的端到端任务完成率。
 - 结果：在 AndroidLens 上达到 SOTA，在 MobileGym 和 MobileWorld 上获得一致提升。

3. 测试时强化学习（test-time RL）：
 - 世界模型作为模拟器提供交互轨迹，Agent 在测试时通过 RL 更新策略。
 - 结果显示无需与真实应用交互即可进一步改善策略，验证了世界模型在闭环优化中的价值。

注意：检索证据仅明确提到静态动作预测设置，未给出具体数值；CMGUIBench-500 的“500 random samples”是明确信息。实验细节（如超参数、模型架构、训练预算）在证据中缺失，需回原文确认。

Q5: 发现了什么实验现象？

基于摘要与证据，论文报告了以下实验现象：

1. 保真度优势：AppDeltaWorld 在 CMGUIBench-500 上取得最高整体保真度，尤其在结构布局和 UI 元素重建方面显著优于图像-only 和代码-only 基线。这表明“delta code + 层级检索 + 视觉资产插入”的设计确实缓解了单一模态的缺陷。
2. 模态互补效果：图像-only 方法可能在视觉外观上具有一定优势但结构不稳；代码-only 方法结构稳定但视觉细节不足；AppDeltaWorld 通过“代码为主、视觉资产补充”的方式同时改善了两方面。
3. 转移一致性改进：动作约束的检索使世界模型能够表达对无效动作的拒绝或低置信度，减少无约束幻觉状态的出现。这是与纯生成式世界模型（如 Mobile-Dreamer 的草图生成）的关键差异。
4. 数据有效性：过滤后的闭环 SFT 数据与公开监督结合时，能够提升下游 Agent 在多个基准上的性能，尤其在 AndroidLens 上达到 SOTA，说明合成数据具有真实可用的训练信号。
5. 测试时 RL 的收益：在测试阶段利用世界模型进行策略适应，可以带来额外提升，且无需真实交互。这暗示世界模型不仅可用于离线数据生成，还可用于在线策略优化。

注意：这些现象中，部分数字（如准确率、提升幅度）没有出现在检索证据中，因此无法给出具体数值。另外，未报告的负结果或失败案例在证据中不存在，需要谨慎对待。合理推断是论文中可能讨论了某些基线（如图像生成）的失败模式，但具体细节需查原文。

Q6: 有什么可以进一步探索的点？

结合方法本身与现有空白，可以延伸出以下探索方向：

1. 应用覆盖扩展：当前 Level-1 HTML 参考库需要针对每个应用或应用类别构建。未来可研究自动挖掘应用 UI 结构、跨应用共享参考模板，减少人工维护成本。
2. 动态界面与版本漂移：移动应用界面频繁更新，Level-1 参考可能过期。如何让世界模型自动感知版本变化并更新检索库，是一个值得关注的在线适应问题。
3. 长程一致性：本文主要关注单步转移。将增量代码世界模型扩展到多步 rollouts 时，误差累积和转移合法性累积判定需要进一步研究；可结合模型置信度实现自适应 rollout 截断。
4. 动作空间扩展：当前模型可能覆盖常见触摸动作（点击、滑动、输入）。对于复杂手势（长按、拖拽、多指操作）和系统级操作（返回、权限弹窗），需要扩展转移约束的表达。
5. 真实隐私约束下的数据利用：世界模型生成可合成敏感应用行为，但如何保证生成内容不泄露隐私、不学习不当模式，需要引入安全约束和审计机制。
6. 与更强视觉-语言模型的结合：利用多模态基础模型增强视觉资产生成质量和文本理解，让 Level-2 HTML 生成更精准。
7. 世界模型度量体系：现有 Code2World 评估侧重渲染后截图指标，可加入动作合法性误判率、结构树精确匹配、以及端到端任务成功率等更全面的协议。
8. 跨 Agent 迁移：将 world-model-in-the-loop 数据用于不同架构的 Agent，分析合成数据的通用性和覆盖度。
9. 与真实环境混合训练：探讨如何动态平衡合成世界模型与真实交互数据的比例，使策略在真实部署中更鲁棒。
10. 理论分析：为什么 delta code 比绝对生成更利于转移一致性？可尝试从组合泛化和误差传播角度给出理论解释。

Q7: 总结一下论文的主要内容

AppDeltaWorld 是一篇面向移动 GUI Agent 的世界模型论文，核心主张是：将下一 GUI 状态建模为“可达的代码更新”，而不是无约束的图像或文本生成，可以在保真度、模态覆盖和动作转移一致性上同时取得改善。

论文从两个领域的痛点出发：一方面，真实移动交互轨迹在敏感应用和隐私关键操作中难以获取；另一方面，模拟环境搭建和维护成本高，现有 GUI 世界模型又存在生成不稳定、模态单一、转移逻辑不一致等问题。为此，作者提出一个四阶段流程：首先在动作转移约束下检索应用特定的 Level-1 HTML 参考；然后基于当前屏幕、动作、预测文本和检索结构生成 Level-2 可执行 HTML；再合成视觉资产并插入图像槽；最后通过浏览器渲染得到下一屏。整个过程是转移锚定的，即模型需要判断动作是否有效，对无效或低置信度转移予以拒绝，而非幻觉式生成。

作为世界模型，AppDeltaWorld 在 CMGUIBench-500 上以 Code2World 协议评估取得最高保真度，关键提升表现在结构布局和 UI 元素重建，验证了代码生成与视觉插入互补的有效性。作为训练环境，AppDeltaWorld 生成过滤后的闭环 SFT 数据，与公开监督结合训练 AppDeltaAgent，在 AndroidLens 上达到 SOTA，并在 MobileGym 和 MobileWorld 上一致提升。此外，论文展示了测试时强化学习能力：仅通过与世界模型交互即可进一步改善策略，无需接触真实应用。

论文的论证主线是“数据稀缺 -> 现有解决方案不足 -> 以转移约束的增量代码世界模型提供可扩展且高保真的替代 -> 通过世界模型与下游 Agent 的双重评估证明其价值”。技术主线是“层级 HTML 检索 + 条件可执行 HTML 生成 + 视觉资产插入 + 世界模型在环数据构建”。实验主线则分世界模型保真度和 Agent 能力两条线，并通过测试时 RL 进一步验证世界模型的可优化闭环性质。

整体而言，AppDeltaWorld 为移动 GUI 领域提供了一个具有实用潜力的合成数据引擎，既缓解了真实轨迹隐私问题，又避免了对高成本模拟环境的依赖，同时通过代码层面的增量建模提高了预测的可控性和结构可解释性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent 方向（权重 0.10）高度相关：论文直接研究移动 GUI Agent 的数据获取与策略优化。

## 基本信息

- 作者：Weikai Xu, Yunren Feng, Haoxiang Lei, Kun Huang, Yuxuan Liu, Kang Zhao, Xiaolin Hu, Shuo Shang, Bo An
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-08-06
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.05891v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据片段（Introduction、Abstract、Conclusion、GUI world models 与 settings 部分），并结合启发式草稿进行扩展；具体数值、未出现在证据中的细节均以合理推断或推测标注。
