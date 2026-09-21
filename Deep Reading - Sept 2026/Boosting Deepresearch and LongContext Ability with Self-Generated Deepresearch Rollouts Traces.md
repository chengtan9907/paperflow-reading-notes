---
user_id: "cheng tan"
paper_id: 12266
arxiv_id: "2609.20844"
title: "Boosting Deepresearch and LongContext Ability with Self-Generated Deepresearch Rollouts Traces"
publish_date: "2026-09-21"
pdf_url: "https://arxiv.org/pdf/2609.20844"
abs_url: "https://arxiv.org/abs/2609.20844"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-22T00:07:13"
---
# Boosting Deepresearch and LongContext Ability with Self-Generated Deepresearch Rollouts Traces

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：deepresearch agent · agentic reinforcement learning · long-context training · synthetic data

## 一句话总结

该论文发现 DR-RL 之后剩余错误中 61.6% 仍源于长上下文理解不足，于是把 Deepresearch rollout 轨迹中的摘要片段替换为对应 URL 全文，零标注成本地合成 LongQA 训练数据，并用「短 DR-RL 采集 → LongQA-RL 强化长上下文 → 完整 DR-RL」的三阶段 DLD-RL 训练流程，在三个 Deepresearch 基准上超过标准 DR-RL 7.3%、在三个长上下文基准上提升 13.5%。

## 摘要

> Deepresearch (DR) agents interact with real-world web environments through multi-turn search and visit, causing their contexts to grow rapidly over time. We observe that, even after DR Agentic Reinforcement Learning (DR-RL), 61.6% of the model's remaining prediction errors can still be attributed to insufficient long-context understanding, including longcontext hallucination and failures in cross-document evidence integration. It motivates us to further break the bottleneck of DR-RL by strengthening the model's long-context ability. However, effective LongContext training requires more than simply increasing context length. To bridge the data gap, we propose `DR Rollouts to LongContext-QA (DR-to-Long)'. The method repurposes DR-RL trajectories, which naturally contain search histories, visited webpages, evidence snippets, and final-answer supervision. It then replaces the compact snippets and webpage summaries in each trajectory with the full contents of their corresponding URLs, producing substantially longer multi-document contexts while preserving the original evidence relationships. Building on DR-to-Long, we introduce DLD (DR -> LongQA -> DR)-RL. DLD-RL first performs a short DR-RL stage to collect rollout trajectories, which are then converted into LongQA instances at zero annotation cost. The model is subsequently optimized with LongQA-RL to strengthen LongContext ability, followed by full DR-RL to continue improving its DR capability. Experiments show that DLD-RL outperforms standard DR-RL by 7.3% on three Deepresearch benchmarks and improves performance by 13.5% on three long-context benchmarks.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的问题可以拆成三层，层层递进：

1）表层问题（现象）：Deepresearch 智能体需要在真实网页环境中进行多轮 search 与 visit，其上下文会随交互轮数迅速膨胀。这类任务天然是「多文档 + 长上下文 + 需要跨来源证据整合」的超长上下文场景，而现有 DR agent 的能力瓶颈并不只在“能不能搜到”。

2）中层问题（诊断）：作者给出了一个量化的错误归因结论——即使经过 DR Agentic Reinforcement Learning（DR-RL），模型剩余预测错误中仍有 61.6% 可归因于长上下文理解不足，具体形态包括 long-context hallucination（在长上下文中生成与证据不符的内容）与 failures in cross-document evidence integration（跨文档证据整合失败）。也就是说，DR-RL 之后的主要失效模式从“检索/规划不足”转移到了“读得不够准”。

3）底层问题（数据与训练范式）：要把长上下文能力补上，仅靠「增大上下文长度」是不够的（论文明确表述：effective LongContext training requires more than simply increasing context length）。真正缺的是数据：需要有真实证据关系、有监督信号、且与 DR 推理时上下文分布一致的长上下文多文档 QA 数据。人工标注这类数据成本极高；普通合成长上下文数据（如把无关文档拼接）又缺少 DR 场景里「哪段证据支撑哪一步推理」的对应结构。

因此论文把「提升 DR-RL 上限」重构成「用零标注成本构造长上下文训练数据，并在训练流程中显式安排 DR 与 LongQA 的交替优化」。

隐含假设与需要核对的地方（标注为推断/待原文确认）：
• 错误归因流程本身可信——61.6% 这一比例依赖于作者如何定义并判定“长上下文相关错误”，其分类器或人判标准需要看原文，且该比例很可能只在特定 base model、特定 RL 检查点与特定 benchmark 上测得，跨模型泛化性未知（待原文确认）。
• 长上下文能力提升可以正迁移回 DR 端到端表现——这是论文的核心因果主张，但摘要只给了最终指标，未给中介分析（待原文确认）。
• 用轨迹扩充得到的长多文档上下文，能代表真实 DR 推理时的上下文分布——替换为 URL 全文会引入大量原始网页噪声与无关内容，这是否更接近真实推理分布（而非人为加噪）需要实验证据支持（合理推断，需原文核对）。
• 三阶段训练的总计算量大于标准 DR-RL，因此收益中可能混入“训练更多”的效应，需要等算力/等步数对照来分离（合理推断，需原文核对）。

Q2: 有哪些相关研究？

由于本次仅有摘要与元数据、未获得 PDF 正文与参考文献，以下按研究线路梳理与其最可能相邻的工作族，并明确标注哪些属于推断：

1）Deepresearch / 搜索型智能体：一类让 LLM 在多轮中调用搜索引擎与网页浏览工具、最终给出带引用答案的 agent 范式，通常配有端到端评测基准（用于衡量多跳检索、信息整合与答案正确性）。本文把这类 agent 的上下文膨胀问题作为出发点，属于该线路的“能力瓶颈诊断”分支。具体引用了哪些 DR benchmark 与 agent 框架，摘要未说明（待原文确认）。

2）Agentic Reinforcement Learning（DR-RL）：把搜索/浏览作为环境，用最终答案正确性等结果信号做 RL，让模型学会规划查询、决定何时停止检索。本文的直接对照基线就是 standard DR-RL；其贡献被定位为“在 DR-RL 之上进一步打破瓶颈”，而不是替换 DR-RL。

3）长上下文 LLM 与长上下文训练：包括上下文扩展（位置编码外推/插值）、长上下文 SFT、长上下文 RL，以及长上下文评测（needle-in-haystack、多文档 QA、长文推理类基准）。本文与这一线路的差异点在于数据来源：不是爬取/合成长文档，而是从 agent 自身 rollout 中“膨胀”出长上下文实例，从而保留证据关系。

4）自生成数据与轨迹复用（self-generated data / rollout reuse）：把 RL 过程中产生的轨迹二次利用为监督或偏好数据，是当前降低标注成本的常见做法；本文把这一思路用于“跨任务形态转换”（DR 轨迹 → LongQA 样本），与常见的“同任务拒绝采样/自蒸馏”不同，属于跨能力迁移式的数据再利用（这一点是本文较具辨识度的设计）。

5）多阶段 / 课程式 RL 训练：先易后难、先短后长、先辅助任务再主任务的训练编排在 RL 与后训练中常见。DLD-RL 的 DR → LongQA → DR 顺序即属此类，本文的增量在于中间阶段的任务形态是“同一批数据换一种问法”。

6）跨文档证据整合与长上下文幻觉：与 RAG 忠实性、citation faithfulness、多跳推理类工作相邻。本文把“跨文档证据整合失败”直接列为 DR 剩余错误的主要成分，等于把忠实性问题纳入 agent 训练回路。

与既有工作的关键区别（据摘要可确认的部分）：以往长上下文训练数据与 agent 训练数据基本是两条独立供给线；本文让二者共用同一批 rollout，从而在零标注成本下同时获得证据对齐的长上下文数据与可继续 DR 训练的模型状态。摘要未提供与具体同期方法的量化对比清单（待原文确认）。

Q3: 论文如何解决这个问题？

论文的方法由“数据构造”与“训练流程”两个部件组成。

一、数据构造：DR Rollouts to LongContext-QA（DR-to-Long）
• 输入：DR-RL 阶段的 rollout 轨迹。轨迹天然包含四类信息——搜索历史（query 序列）、访问过的网页、证据片段（evidence snippets / 网页摘要）、以及最终答案监督信号。
• 变换：把轨迹中紧凑的 evidence snippet 与网页摘要，替换为其对应 URL 的完整正文内容。
• 输出：显著更长的多文档上下文 + 与原轨迹一致的问答监督。关键在于“保留原始证据关系”——即哪些片段支撑哪一步推理、最终答案来自哪些来源，这一对齐结构在替换后仍然成立，因此长上下文样本不是随机拼接的 distractor 堆叠，而是真实检索路径的展开。
• 成本：零额外人工标注。数据完全由已有 rollout 派生，属于“同一份交互痕迹的形态转换”。

二、训练流程：DLD（DR → LongQA → DR）-RL
1）阶段一：短 DR-RL。先做一轮较短的 DR 强化学习，目的有二——获得初始 DR 策略、并采集用于转换的 rollout 轨迹。
2）阶段二：LongQA-RL。用 DR-to-Long 生成的 LongQA 实例做强化学习，显式强化模型的长上下文理解与跨文档证据整合能力。此阶段是论文主张的“补短板”环节。
3）阶段三：完整 DR-RL。回到 Deepresearch 任务继续做完整 RL，继续提升 DR 能力，检验长上下文增益能否落地为端到端 agent 收益。

三、设计逻辑与取舍（部分为合理推断）
• 之所以采用“先短后长再回主任务”，而不是一次性混训：作者需要一个可复用的轨迹池来生成数据，所以必须先跑一轮 DR；而把 LongQA 放在 DR 之前或之后会带来不同后果——若只用 LongQA 微调/RL 而不回到 DR，可能出现对 agentic 行为的灾难性遗忘；论文用最终 DR 指标不降反升（+7.3%）来回应这一点（推断：论文可能讨论了遗忘问题，需原文核对）。
• 与“直接增加 context window / 继续加长 DR 训练上下文”的替代路线相比，本文选择从数据供给切入，属于数据中心的解法。

四、摘要未覆盖、需要原文确认的技术细节
• LongQA 实例的构造细节：URL 全文如何截断到目标长度、抓取失败/付费墙如何处理、是否做去重与质量过滤、是否混入原始长上下文语料。
• RL 算法与奖励设计：使用 PPO 类还是 GRPO 类、LongQA 阶段的奖励是答案匹配还是含忠实性/引用项、是否做 KL 约束与长度惩罚。
• 轨迹数量、上下文长度分布、训练 token 预算、与基线是否等算力。
• 是否做过“只替换部分片段”“替换成随机网页”“按不同长度截断”等数据侧消融。

Q4: 论文做了哪些实验？

据摘要可确认的实验设置如下（细节大量缺失，需原文补足）：

1）评测范围：三类 Deepresearch 基准 + 三个长上下文基准。两组评测分别对应两个主张——长上下文训练能提升 DR 端到端表现，且确实提升了长上下文能力本身。
2）主基线：standard DR-RL（不使用 LongQA 中间阶段的传统 DR 强化学习流程）。
3）主结果：DLD-RL 相比标准 DR-RL，在三个 Deepresearch 基准上平均提升 7.3%，在三个长上下文基准上平均提升 13.5%。

摘要未说明、需要在原文核对的实验要素（均为待确认，不得当作已有结论）：
• 具体基准名称、base model 及其规模、来源（开源/自研）、是否单一模型验证。
• 长上下文基准是通用长上下文评测还是作者自建；是否与 Deepresearch 场景相关。
• 训练配置：轨迹采集规模、LongQA 样本量、上下文长度上限与分布、RL 算法、学习率/步数、总算力，以及与基线是否等 token 等步数对照。
• 消融维度：是否有“仅做 LongQA-RL”“改变三阶段顺序（如 DR → DR → LongQA）”“同一批数据只做 SFT 而非 RL”“替换全文 vs 保留摘要”等对照。
• 评测协议：答案判定方式（精确匹配 / LLM judge / 人工）、是否报告多次运行方差、是否控制搜索工具与网页快照版本一致。
• 公平性风险（需重点核对）：DLD-RL 比标准 DR-RL 多出一个 RL 阶段，总训练量更大，7.3% 的 DR 增益中可能有一部分来自“训练更多”而非长上下文机制本身；需要等预算对照或“继续 DR-RL 同长度”的消融来分离（合理推断，需原文确认）。

Q5: 发现了什么实验现象？

从摘要能提取的实验现象与张力如下：

1）最有信息量的诊断现象：DR-RL 之后仍有 61.6% 的剩余错误归因于长上下文理解不足。这揭示了一种能力瓶颈的转移——强化学习把“检索策略/工具调用”类错误压下去之后，暴露出来的主导失效模式变成了长上下文幻觉与跨文档证据整合失败。对于做 agent 训练的人来说，这相当于指出 DR-RL 的边际收益正在被一个未被显式训练的能力维度限制。

2）两个维度同时上升，而非此消彼长：DR 基准 +7.3%、长上下文基准 +13.5%。如果只在长上下文基准上提升而 DR 下降，说明存在任务间干扰；论文的结果支持“长上下文能力向 DR 正迁移”的解释。但摘要未给出中介分析（例如“长上下文提升能否解释 DR 提升”的相关性证据），因此“正迁移”目前更接近作者的机制解释而非被直接测量的因果链（待原文确认）。

3）提升幅度的不对称值得注意：长上下文基准的提升（13.5%）明显大于 DR 端到端提升（7.3%），比例约为 1.85:1。可能的解释是能力增益在向端到端任务收益转化时存在衰减——DR 只在一部分步骤上需要长上下文能力，其余收益仍受检索质量、规划、答案抽取等环节制约。这是两个指标之间的张力，也是判断该方法上限的关键：如果 DR 剩余的 38.4% 非长上下文错误成为新瓶颈，继续加强长上下文将出现边际递减（此为推断，需原文验证是否报告了错误构成的变化）。

4）零标注成本这一点属于方法属性而非实验现象，但其隐含的可扩展性主张（数据可以随 RL 迭代不断再生）需要看是否做了迭代式数据再生的实验（待原文确认）。

5）失败模式与负结果：摘要未描述任何失败案例、无提升的子任务、或训练不稳定的现象。例如长上下文 RL 是否导致回答变长、过度引用、或让模型在短上下文任务上退化，摘要均未提及，需在原文的 ablation/附录中查找。

Q6: 有什么可以进一步探索的点？

基于论文提出的框架，可以延伸的探索方向分为数据、训练、评测、机制与效率五类：

1）数据侧扩展
• 把 DR-to-Long 的“摘要→全文”替换思路推广到非网页来源：代码仓库、数据库 schema 与查询结果、科学 PDF/文献、多模态网页（图表、视频）。这对 ai-for-science 类 agent 尤其自然。
• 系统研究“膨胀倍率”的影响：context 从 32k 拉到 128k/256k/1M 时收益如何变化，是否存在最优长度而非越长越好。
• 有选择地保留噪声：全文替换必然引入大量无关内容，可研究“受控噪声比”“硬负样本注入”对跨文档整合能力的影响。
• 自动化数据筛选：用检索相关性或证据覆盖度做样本过滤，避免低质量 rollout 污染长上下文数据。

2）训练流程改进
• 单阶段混合 RL：DR 与 LongQA 样本按比例混合采样，替代三阶段串行，观察是否减少阶段切换带来的分布震荡。
• 课程式上下文长度：从较短上下文逐步拉长，与难度课程结合。
• 奖励设计：在 LongQA-RL 中显式加入引用忠实性、跨文档一致性惩罚，而不只看最终答案对错。
• 抗遗忘约束：研究如何确保 LongQA 阶段不损害 agentic 行为（如参数高效更新、KL 约束、双目标优化）。
• 迭代式自举：让第二阶段产生的模型再跑 DR 生成新轨迹，形成 DR→LongQA→DR→LongQA 的循环，检验是否持续增益。

3）评测与诊断
• 把“错误归因”流程产品化：构建常态化诊断工具，持续监控「长上下文相关错误 / 检索相关错误 / 规划相关错误」的比例变化。
• 构建 DR 场景专用的长上下文诊断集，区分「检索到了但没用好」与「根本没检索到」。
• 分离能力：设计只考察跨文档证据整合、不加检索难度的对照任务，以估计能力提升的上界。

4）机制理解
• 长上下文能力与 agentic 能力之间的迁移与干扰机制：是共享表征带来的正迁移，还是多阶段训练的正则化效应？
• 上下文中的证据位置、证据分散度、干扰文档数量对准确率的影响曲线（位置偏置是否随 LongQA-RL 减弱）。

5）效率与部署
• 长上下文推理成本是该方法落地的现实约束，可与 KV cache 压缩、稀疏注意力、上下文摘要/记忆机制结合，检验是否能保留训练收益。
• 训练端：长上下文 RL 的 rollout 显存与吞吐优化。

6）泛化与边界
• 跨 base model 规模与家族的可复现性；跨语言（中文/小语种网页）与跨环境（企业内网、非公开语料）的迁移。
• 长上下文幻觉的安全与可信维度：高风险的错误整合是否会因训练而被放大或抑制。

Q7: 总结一下论文的主要内容

这篇论文的论证主线可以概括为“先诊断瓶颈，再补数据，再编排训练”。

诊断环节：Deepresearch（DR）agent 的工作方式是多轮 search + visit，与真实网页环境交互，这导致其上下文随交互迅速增长。作者做了一个关键的实证观察：即便经历过 DR Agentic Reinforcement Learning（DR-RL），模型剩余的预测错误里仍有 61.6% 可以归因于长上下文理解能力不足，具体表现为长上下文幻觉与跨文档证据整合失败。这意味着在 DR-RL 之后，制约进一步收益的主要因素不再是搜索/工具使用策略，而是模型阅读与整合超长多文档证据的能力。于是论文把“继续提升 DR 表现”的问题，重新表述为“如何强化模型的长上下文能力”。

数据环节：作者指出，有效的长上下文训练不只是把上下文长度调大，真正的障碍是数据缺口。为此提出 DR Rollouts to LongContext-QA（DR-to-Long）。该方法复用 DR-RL 产生的 rollout 轨迹——这些轨迹天然包含搜索历史、访问过的网页、证据片段与最终答案监督——把其中紧凑的证据片段与网页摘要，替换为对应 URL 的完整正文，由此得到显著更长的多文档上下文；由于替换是“原位展开”，原始证据关系（哪段内容支撑哪一步推理、答案来自哪些来源）被完整保留。整个过程不需要任何人工标注，数据由已有交互痕迹派生。

训练环节：在 DR-to-Long 之上提出 DLD（DR → LongQA → DR）-RL 三阶段流程。第一阶段做一次较短的 DR-RL，用于获得初始 DR 策略并采集可转换的轨迹；第二阶段用转换得到的 LongQA 实例做 LongQA-RL，显式强化长上下文理解；第三阶段回到 Deepresearch 做完整 DR-RL，继续提升 DR 能力，并检验长上下文能力的增益能否转化为端到端 agent 表现。这个编排使同一批交互数据被用两次：一次作为 DR 训练信号，一次作为长上下文训练信号。

实验主线：论文在三个 Deepresearch 基准与三个长上下文基准上评测，主对照是标准 DR-RL。结果为 DLD-RL 在 DR 基准上平均高出 7.3%，在长上下文基准上平均提升 13.5%。两组指标同向上升，支撑了“补长上下文能力可以进一步抬升 DR-RL 上限，且不以牺牲 agent 能力为代价”的核心主张。

需要注意的读法：摘要给出了清晰的动机—方法—结果闭环，但若干关键论证在摘要层面尚不可验证——61.6% 的归因口径、三个 DR 与三个长上下文基准的具体构成、base model 与训练预算、与基线是否等算力、三阶段顺序的消融证据、以及长上下文能力到 DR 收益之间的中介关系，均需回到原文确认。此外，DLD-RL 相比标准 DR-RL 额外引入了一个 RL 阶段，总训练量的差异是评估增益归因时必须剥离的变量。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与画像主方向「agent」（权重 0.10）直接重合：论文研究 deepresearch agent 的强化学习训练、工具调用（search/visit）环境与端到端能力提升，属于 agent 训练范式的前沿问题。

## 基本信息

- 作者：Zihan Wang, Hao Wang, Boyuan Jiang, Yiqun Zhang, Shi Feng, Xiaocui Yang, Yiwen Ye, Jianghang Lin, Xiaozhong Ji, Jinghao Lin, Kai Wu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-09-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.20844`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次未命中任何 PDF 语义检索证据（retrieved_evidence 与 field_evidence_map 均为空），全部内容仅基于标题、作者、摘要与元数据推断生成，方法实现细节与实验设置均需回原文核对；另需注意元数据中的 arXiv 编号与发布年份（2609.20844 / 2026-09-21）属未来时间，引用前应确认版本与出处。
