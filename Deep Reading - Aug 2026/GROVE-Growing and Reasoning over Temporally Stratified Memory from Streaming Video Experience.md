---
user_id: "cheng tan"
paper_id: 6205
arxiv_id: "2608.02392v1"
title: "GROVE: Growing and Reasoning over Temporally Stratified Memory from Streaming Video Experience"
publish_date: "2026-08-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.02392v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.02392v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-05T01:52:51"
---
# GROVE: Growing and Reasoning over Temporally Stratified Memory from Streaming Video Experience

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：video memory · streaming video understanding · wearable assistant · temporal hierarchy

## 一句话总结

GROVE 提出了一种无需训练的流式视频记忆框架，通过将连续视频流因果地组织为感知证据、时刻、情节和跨天模式四个时间分层，并用尺度原生检索技能统一服务反应式问答与主动式辅助。

## 摘要

> A wearable assistant should both answer questions about its visual history and recognize when that history is useful to the present situation. Existing video-memory systems primarily support question-conditioned recall, whereas proactive assistants typically use separate memory and control mechanisms. We introduce GROVE, a training-free framework that supports both behaviors with one memory grown causally from a continuous video stream. GROVE retains fine-grained perceptual evidence and incrementally consolidates it into time-stamped moments, coherent episodes, and recurring cross-day patterns. Each stratum is paired with a scale-native retrieval skill for locating an observation, replaying an activity, or traversing long-range regularities. Reactive QA and proactive assistance share this memory and access interface, differing in whether retrieval is initiated by a user query or the current situation. Across multiple benchmarks including the challenging MM-lifelong and EgoServe, GROVE achieves the best results among the compared methods. Controlled ablations show that the temporal strata and their access skills are complementary, with patterns providing the largest benefit when evidence spans multiple days. Code will be available at https://github.com/SitongGong/GROVE.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：可穿戴助手在连续观察用户数小时、数天甚至数周的第一视角视频后，如何构建一个既能被用户显式查询（reactive QA）又能被当前情境主动触发（proactive assistance）的统一记忆系统。

1. 现有视频记忆系统的结构性缺口：
 - 现有系统（如 Xie et al. 2026、Long et al. 2025、Li, Numan, and Steed 2026、Chen et al. 2026a、Li et al. 2026、Liang et al. 2026 等）虽然已能构建在线事件记忆、层次化组织经验、用迭代或智能体式检索定位稀疏证据，但绝大多数仍遵循“查询优先”（query-first）契约：必须有一个显式请求来决定检索什么。
 - 主动助手（如 Zhang et al. 2025b、Gong et al. 2026）虽然能用当前情境决定是否干预，但其记忆和控制机制主要为服务触发设计，而非作为开放式回忆的统一接口。
 - 因此缺失的能力不是另一个记忆层级或更强的搜索过程，而是一个单一的流式记忆，既可由用户查询访问，也可由当前情境访问。

2. 为什么这个问题重要：
 - 反应式问答和主动式辅助依赖同一个底层能力：把开放式的视频流转化为随时间保持忠实（faithful）、可搜索（searchable）和可行动（actionable）的记忆。
 - 若两个任务使用分离的记忆系统，会导致重复存储、一致性问题（两个记忆对同一事件的描述可能冲突）以及维护成本翻倍。

3. 问题定义的特征：
 - 流式（streaming）：视频随时间持续到达，无法一次性离线处理。
 - 因果性（causal）：记忆构建不能依赖未来帧，系统在任意时刻只能使用当前及过去的信息。
 - 长期性（long-term）：跨度可达数天至数周，存在跨天重复模式。
 - 双访问模式（dual-access）：同一记忆需要同时支持用户发起的检索和情境发起的检索。
 - 无需训练（training-free）：不针对记忆组织使用训练信号，依赖冻结的感知模型（合理推断，从 limitation 中“frozen perception model”可以确认感知模型是冻结的）。

Q2: 有哪些相关研究？

从摘要、引言和结论中可以定位到以下几类相关研究（检索证据不足，无法给出完整文献综述细节，但可以梳理研究脉络）：

1. 视频记忆系统（Video-memory systems）：
 - 代表性工作包括 Xie et al. 2026、Long et al. 2025、Li, Numan, and Steed 2026、Chen et al. 2026a、Li et al. 2026、Liang et al. 2026。
 - 这些工作已解决长视频和流式视频中的多个难点：在线事件记忆构建、经验层次化组织、迭代式/智能体式检索定位稀疏证据。
 - 共同局限：绝大多数保留“查询优先”契约，检索必须由显式请求触发，不支持情境驱动的主动检索。
 - 论文评价为“这些进展改变了过去的存储和搜索效率，但契约未变”。

2. 主动助手（Proactive assistants）：
 - 代表性工作包括 Zhang et al. 2025b、Gong et al. 2026。
 - 特点：用当前情境决定是否干预（如用户即将重复犯错时主动提醒）。
 - 共同局限：记忆和控制机制主要为服务触发设计，不是作为开放式回忆的通用接口；记忆与控制在架构上分离。

3. 基准（Benchmarks）：
 - MM-Lifelong（Chen et al. 2026c）：多模态终身学习/记忆基准，含多个 split（包括主文省略的 Train@Month）。
 - OVO-Bench（Niu et al. 2025）：一对多视频问答基准。
 - StreamingBench（Lin et al. 2026）：流式视频理解基准。
 - ESTP-Bench（Zhang et al.，引用编号被截断）：估计与时间定位相关基准。
 - EgoServe：第一视角长期视频服务基准，与 GROVE 的场景高度契合（合理推断：该基准包含主动服务/辅助任务的评测）。

4. 论文的研究范式定位：
 - 现有范式 A：记忆系统 + 查询接口（由查询触发检索）。
 - 现有范式 B：主动助手 + 服务触发控制（由情境触发服务）。
 - GROVE 的范式 C：统一记忆 + 双访问接口（同一记忆同时被查询和情境访问）。
 - 论文的论点是：范式 A 和 B 并不需要两套系统，因为“什么时候回答”和“什么时候行动”两种策略读取的是同一个构建好的过去。

Q3: 论文如何解决这个问题？

GROVE 的解决方案围绕“时间尺度”（temporal scale）这一组织原则展开：

1. 四层时间分层记忆结构（causal 构建）：
 - 第一层：感知证据（perceptual trace）——保留细粒度的逐帧感知信息，是后续所有层的基础，也是当前窗口查询的依赖层。
 - 第二层：带时间戳的时刻（time-stamped moments）——由感知证据增量整合而来，记录离散的时间点事件。
 - 第三层：连贯情节（coherent episodes）——把相关时刻整合为有始有终的活动片段。
 - 第四层：跨天重复模式（recurring cross-day patterns）——从多个情节中归纳出用户行为中反复出现的规律。
 - 构建过程是因果的（causal）：逐层向上，从输入帧到感知证据、到时刻、到情节、到模式。

2. 尺度原生检索技能（scale-native retrieval skill）：
 - 每一层记忆配有一个专门的检索技能：
 - 定位一次观察（locating an observation）→ 对应感知证据/时刻层。
 - 回放一段活动（replaying an activity）→ 对应情节层。
 - 遍历长期规律（traversing long-range regularities）→ 对应模式层。
 - 设计思想：不同时间尺度的查询需要不同的检索操作，单一检索过程无法高效处理所有尺度的问题。

3. 统一访问接口（shared memory and access interface）：
 - 反应式问答和主动式辅助共享同一记忆和技能库。
 - 区别仅在检索发起者：用户查询发起（reactive）还是当前情境发起（proactive）。
 - 论文用“两个策略”（two policies）来比喻：用户查询策略和情境触发策略读取同一个构建好的过去，通过同一个尺度原生接口进行决策。

4. 无需训练（training-free）：
 - 框架不依赖针对记忆组织的训练信号。
 - 感知模型是冻结的（frozen perception model），这既是优势（无需训练数据）也是局限来源（感知错误被继承）。

5. 更新机制（从 limitations 反推）：
 - 高层记忆只在情节关闭（episode closes）时更新。
 - 因此当前窗口（尚未关闭的情节）内的查询只能依赖低层感知证据。
 - 摄入阶段（ingestion）是唯一获取证据的时机，错过即不可恢复。

Q4: 论文做了哪些实验？

由于检索到的证据只包含部分实验信息，以下对实验设计的描述部分基于论文可得信息的合理推断，并用标注区分：

1. 评测基准（5个）：
 - MM-Lifelong（Chen et al. 2026c）：多模态终身记忆基准。证据明确：主文报告了部分 splits，附录 B.1 的 Table 13 报告了全部四个 splits，包括主文因篇幅省略的 Train@Month split。GROVE 在每个 split 上取得最佳分数，对 ReMA 的优势在新增 split 上保持。
 - OVO-Bench（Niu et al. 2025）：证据确认被评测，具体任务形式和指标未知。
 - StreamingBench（Lin et al. 2026）：证据确认被评测，具体任务形式和指标未知。
 - ESTP-Bench（Zhang et al.）：证据确认被评测，引用编号被截断，具体任务形式和指标未知。
 - EgoServe：证据确认被评测，被与 MM-Lifelong 并列为“具有挑战性”的基准，具体任务形式和指标未知。

2. 对比方法（baselines）：
 - 证据明确提及 ReMA 作为 MM-Lifelong 上的对比方法，GROVE 全面优于 ReMA。
 - 其他对比方法未出现在检索证据中（信息缺口：需要原文确认完整 baseline 列表）。

3. 控制消融（controlled ablations）：
 - 检验各时间层（时刻/情节/模式）及对应技能是否互补。
 - 证据明确：跨多天证据场景中，patterns 层带来的收益最大。
 - 这验证了核心假设：时间分层结构与尺度原生检索技能的组合优于单一结构。

4. 合理推断的实验设置：
 - 实验应同时覆盖反应式 QA 任务和主动式辅助任务，以验证统一记忆接口的可行性。
 - MM-Lifelong 和 EgoServe 可能分别侧重反应式和主动式（推测）。
 - 需要原文确认：具体指标（如 QA 准确率、时序定位精度等）、输入视频长度、感知模型的选型、prompt 设计等细节。

Q5: 发现了什么实验现象？

检索证据支持的实验观察有限，以下区分“证据明确支持的观察”与“合理推断”进行描述：

1. 证据明确支持的观察：
 - MM-Lifelong 主文报告的 splits 上，GROVE 全面取得最佳成绩；附录新增的 Train@Month split 上，GROVE 仍然保持最优，且对 ReMA 的领先幅度在新增 split 上不缩水。这意味着 GROVE 的优势不是只针对特定时间跨度，而是跨不同训练/测试时间区间都稳健。
 - 在“挑战性”基准 MM-Lifelong 和 EgoServe 上，GROVE 均胜过所有对比方法。
 - 消融实验显示：时间分层与对应的访问技能是互补的（complementary）——去掉任一层的技能组合都会带来性能下降（合理推断自“互补”的表述），说明层与技能之间不是冗余关系。
 - 最值得注意的消融趋势：patterns 层（跨天重复模式）在证据跨越多个天时提供最大收益。这是一个符合直觉但需要实验验证的设计选择——越是长期的问题，越需要高层的规律性归纳。

2. 合理推断的观察（根据 limitation 内容反推实验现象）：
 - 当前窗口内的查询（尚未关闭的情节）性能受限于底层感知证据的质量，高层记忆无法参与，说明“情节关闭时更新”的机制在实时性上有代价。
 - 摄入阶段感知模型漏掉的证据无法在后续阶段恢复，这会导致特定错误类型：感知遗漏型错误（而非检索失败型错误）。

3. 未观察到的内容（信息缺口）：
 - 检索证据未包含任何具体数值（准确率、F1、时序定位误差等），因此无法报告具体指标。
 - 未包含跨基准的性能对比矩阵细节。
 - 未包含失败案例的定性分析。
 - 若需要精确数值，必须回原文核对表格。

Q6: 有什么可以进一步探索的点？

基于论文的设计选择、局限性和当前证据，可以梳理出以下可进一步探索的方向（标注推测与合理推断）：

1. 感知层升级（证据支持）：
 - GROVE 继承冻结感知模型的错误，且摄入阶段漏掉的证据不可恢复。未来可用更强的感知模型、多视角融合或主动感知（如检测到低置信度时提醒用户确认）来减少摄入阶段的信息丢失。

2. 更新机制改进（证据支持）：
 - 当前高层记忆在情节关闭时才更新。可探索滑动窗口式或在线增量式的情节/模式更新，使当前窗口内的查询也能利用高层结构化信息。

3. 模式层的主动利用（证据支持的合理推断）：
 - 消融显示 patterns 对跨天场景收益最大，说明主动辅助（如预防重复犯错）高度依赖模式层。未来可专门研究：如何从 patterns 中派生可行动的提醒策略（何时该干预、如何表达提醒），而不是仅仅检索。

4. 可训练版本的探索（推测）：
 - GROVE 是 training-free 的，这避免了训练数据需求，但也意味着层与层之间的整合规则是启发式的。未来可探索用强化学习或偏好优化学习“何时该检索哪一层”，以替代手工设计的尺度原生技能。

5. 多用户与隐私（推测）：
 - 可穿戴助手记录用户数天到数周的第一视角视频，天然涉及隐私。跨天模式（patterns）的归纳意味着系统在持续学习用户的习惯——未来需要研究记忆的遗忘机制、用户可控的记忆编辑、以及模式归纳的隐私边界。

6. 记忆压缩与资源效率（推测）：
 - 数天的视频流意味着感知证据层的数据量巨大。未来可探索分层压缩策略：低层保留短期、高层长期驻留，或在模式稳定的情况下压缩对应情节的存储。

7. 多模态扩展（推测）：
 - 当前证据显示系统以视频为核心。未来可将音频（对话、环境声）、IMU 传感器、位置信息等纳入记忆分层，补充视频难以捕捉的信息。

8. 评测体系的扩展（合理推断）：
 - 现有基准（MM-Lifelong、OVO-Bench、StreamingBench、ESTP-Bench、EgoServe）已覆盖反应式和主动式任务，但可探索更细粒度的评测：主动提醒的时机质量、用户接受率、跨周记忆的保持率等。

Q7: 总结一下论文的主要内容

GROVE 提出了一种无需训练的流式视频记忆框架，通过将连续视频流因果地组织为感知证据、时刻、情节和跨天模式四个时间分层，并用尺度原生检索技能统一服务反应式问答与主动式辅助。

可穿戴助手应从用户视角持续观察世界数小时、数天甚至数周，并在此基础上提供两种互补的帮助形式：反应式问答（如“我昨天把钥匙放哪了？”）和主动式辅助（如用户即将重复犯错时主动提醒）。两种行为依赖同一个底层能力：把开放式的视频流变成随时间保持忠实、可搜索、可行动的记忆。

现有视频记忆系统（Xie et al. 2026、Long et al. 2025、Li, Numan, and Steed 2026、Chen et al. 2026a、Li et al. 2026、Liang et al. 2026 等）已能构建在线事件记忆、层次化组织经验、用迭代/智能体式检索定位稀疏证据，但绝大多数保留“查询优先”契约——必须由显式请求决定检索什么。主动助手（Zhang et al. 2025b、Gong et al. 2026）则用当前情境决定是否干预，但记忆和控制机制主要为服务触发设计，与开放式回忆接口分离。

因此研究空白是：缺乏一个单一记忆流，既能被用户查询访问，也能被当前情境访问。GROVE 以时间尺度为组织原则，提出训练免费框架，用同一记忆同时服务反应式问答与主动式辅助。

GROVE 的核心方法分为两个部分：时间分层记忆结构与尺度原生检索接口。

1. 时间分层记忆：从流式视频中因果地逐层构建四层结构：
 - 感知证据层：保留细粒度逐帧感知证据，是基础层。
 - 时刻层：增量整合为带时间戳的时刻。
 - 情节层：进一步整合为连贯活动片段。
 - 模式层：归纳跨天重复的行为模式。
 - 更新是因果的（只依赖当前和过去的信息），且高层记忆在情节关闭时才更新。

2. 尺度原生检索技能：每一层配一种专精技能——定位观察、回放活动、遍历长期规律。不同时间尺度的问题使用对应层的技能，而不是用单一检索过程处理所有尺度。

3. 统一访问：反应式 QA 和主动式辅助共享记忆和技能库。两种策略（查询发起 vs 情境发起）通过同一个尺度原生接口读取同一个过去，仅在检索发起者上不同。

4. 无需训练：整个框架不依赖针对记忆组织的训练信号，使用冻结的感知模型（从 limitation 确认）。

5. 已确认的局限（方法固有特性）：摄入阶段丢失的证据不可恢复；当前窗口查询只依赖感知证据层。

关键结果如下（证据只确认定性的结果结构，定量数值需查原文）：

1. 在五个基准上取得最佳成绩：MM-Lifelong、OVO-Bench、StreamingBench、ESTP-Bench、EgoServe。其中 MM-Lifelong 和 EgoServe 被视为挑战性基准。

2. MM-Lifelong 全部四个 splits 上均取得最佳：附录 Table 13 补齐了主文省略的 Train@Month split；GROVE 在每个 split 上都最佳，且对 ReMA 的领先在新增 split 上保持。

3. 消融验证核心设计：时间分层与访问技能互补；跨多天证据场景中，patterns 层收益最大——这是对“时间尺度分层胜过单一结构”主张的最直接证据。

4. 实验层面的隐含成果：反应式与主动式行为在同一记忆系统上同时成立（这是 GROVE 作为统一框架的可行性证据，合理推断）。定量指标（准确率、F1、定位误差等）需要回原文核对表格确认。

主要贡献包括：提出 GROVE 框架：一个无需训练的流式视频记忆框架，以时间尺度为组织原则，从连续视频流因果构建记忆。；设计四层时间分层记忆结构：感知证据、带时间戳的时刻、连贯情节、跨天重复模式，并给每层配备尺度原生检索技能。；统一反应式问答与主动式辅助：两种行为共享同一记忆与访问接口，区别仅在检索发起者是用户查询还是当前情境。；通过五个基准上的最优结果证明有效性，包括具有挑战性的 MM-Lifelong 和 EgoServe。。

需要注意的边界包括：GROVE 继承冻结感知模型的错误，摄入阶段漏掉的证据无法通过后续整合恢复。；高层记忆只在情节关闭时更新，因此当前窗口内的查询只能依赖感知证据层，难以利用高层结构化信息。；训练免费的架构意味着层间整合规则依赖启发式设计而非学习得到（合理推断，原文未直接表述），可能限制其在感知模式复杂时的泛化能力。。

与用户画像的关系：论文与你画像中的“agent”方向直接重合：GROVE 本质上是为可穿戴智能体设计的记忆基座，反应式问答与主动式辅助都是智能体行为的组成部分。；论文属于系统性工作而非增量式：它提出了统一记忆范式并设计了多个基准的评测协议，覆盖任务设定、实验安排和框架构建，符合你对系统性工作的偏好。；时间分层记忆（低层感知/高层模式）的思路可推广到其他长期记忆场景，包括智能体对话历史管理、机器人经验积累等，如果你关注跨场景的 agent 记忆设计，这篇文章是相关的参照物。。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文与你画像中的“agent”方向直接重合：GROVE 本质上是为可穿戴智能体设计的记忆基座，反应式问答与主动式辅助都是智能体行为的组成部分。

## 基本信息

- 作者：Sitong Gong, Caixin Kang, Tianyu Yan, Guo Chen, Bo Zheng, Kaipeng Zhang, Yunzhi Zhuge, Xiang Ruan, Huchuan Lu, Yifei Huang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-08-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.02392v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据（摘要、引言、结论、局限性、附录 B.1 的部分片段），方法细节和实验数值存在信息缺口，已按字段指出需回原文确认的内容。
