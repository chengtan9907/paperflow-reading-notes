---
user_id: "cheng tan"
paper_id: 12661
arxiv_id: "2609.25611v1"
title: "Qwen3.8-Omni: Towards Native Omni-Modal Agents"
institution: "Qwen Team（依据作者署名推断为阿里巴巴通义千问团队；元数据中 institution 字段为空，需以原文单位页确认）"
publish_date: "2026-09-22"
pdf_url: "https://arxiv.org/pdf/2609.25611v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-24T01:08:57"
---
# Qwen3.8-Omni: Towards Native Omni-Modal Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：omni-modal model · multimodal agent · mixture-of-experts · long-context multimodal reasoning

## 一句话总结

Qwen 团队发布 Qwen3.8-Omni-Flash——一个基于稀疏 MoE 主干、上下文扩展至百万 token 的原生多模态 agentic 模型，通过原生多模态共训把文本域的 agent 能力迁移到音频与视频，并配套开源 Qwen-MM-Plugins 插件框架与 Qwen-Live-Harness 实时多模态 agent 框架，使其可承担视频编辑、长音频/长视频翻译、音乐条件视频生成、视频笔记与 omni-skill 创建等生产力任务。

## 摘要

> We introduce Qwen3.8-Omni-Flash, a natively multimodal agentic model for real-world multimodal productivity. Compared with previous omni models, which primarily emphasized perception and interaction, Qwen3.8-Omni-Flash substantially improves multimodal understanding and reasoning, as well as performance on long-horizon agentic tasks. These capabilities are supported by a native multimodal co-training strategy that preserves strong text-domain capabilities while facilitating the transfer of agentic capabilities from text to audio and video tasks. The model inherits the sparse mixture-of-experts (MoE) architecture of Qwen3.8-Next and extends the context window to one million tokens, supporting long-context multimodal reasoning and long-horizon planning. These advances enable integration into production workflows as a primary agent or a specialized sub-agent, supporting video editing, long-form audio and video translation, music-conditioned music video or movie generation, and video-based note or omni-skill creation. To address the lack of native audio and video support in existing agent harnesses, we release Qwen-MM-Plugins, a lightweight open-source plugin framework for multimodal productivity. We further frame real-time multimodal interaction as a system-level challenge requiring orchestration of context and memory management, tool use, and sub-agent delegation. Accordingly, we release Qwen-Live-Harness, an open-source framework for building responsive, real-time multimodal agents based on Qwen3.8-Omni-Flash. Extensive evaluations demonstrate that Qwen3.8-Omni-Flash achieves strong performance across multimodal understanding, reasoning, long-horizon agentic execution, and video productivity tasks. These results and the accompanying open-source tools support Qwen3.8-Omni-Flash as a practical foundation for deploying natively multimodal agents in research and production.

Q1: 这篇论文试图解决什么问题？

【1｜论文自述的问题域】摘要把问题界定为：已有 omni 模型的重心落在“感知与交互”，在 (a) 多模态理解与推理、(b) 长时程 agentic 任务两个维度上投入不足，因而无法作为真实多模态生产力场景中的主力 agent。论文目标是提供一个 natively multimodal 的 agentic model，把理解/推理与长程执行同时做上去，并真正嵌入生产工作流。

【2｜问题被拆成的子问题（依据摘要，括号内标注推断强度）】
- 能力结构问题：感知强 ≠ 推理强 ≠ 会执行长任务。摘要把“multimodal understanding and reasoning”与“long-horizon agentic tasks”并列，说明作者视其为彼此独立的缺口维度。（合理推断：这三者在已有 omni 模型上并非同步增长）
- 跨模态能力迁移问题：文本域已积累成熟的 agentic 能力，但能否迁移到 audio/video 需要专门的“native multimodal co-training strategy”，且必须保证文本域能力不被稀释（“preserves strong text-domain capabilities”）。这实质上是在处理迁移收益与灾难性遗忘之间的跷跷板。
- 上下文与规划长度问题：长时程规划、长音频/长视频处理对上下文长度敏感，论文把窗口扩到 one million tokens，并与“long-context multimodal reasoning and long-horizon planning”直接绑定。（合理推断：作者认为上下文窗口是长时程多模态任务的瓶颈之一）
- 系统集成问题（论文特别强调）：即便模型具备音频/视频能力，现有 agent harness 并不原生支持音频与视频，模型能力无法进入产品链路。论文把“harness 缺支持”当作一等问题，而非工程细节。
- 实时交互的定义问题：摘要明确提出“frame real-time multimodal interaction as a system-level challenge requiring orchestration of context and memory management, tool use, and sub-agent delegation”。这是问题定义层面的主张——实时性不应由模型单点承担，而需系统编排。这是全篇最值得检验的命题之一。

【3｜隐含假设与边界条件】
- 假设更强的多模态理解/推理可单调转化为更强的长时程 agent 执行，二者无结构性冲突。（合理推断）
- 假设共训下文本能力可被“保留”，不存在严重能力跷跷板。
- 假设通用 agent 能力可跨模态迁移，而非各模态需独立训练。
- 假设 long-horizon 规划的主要约束是上下文长度与 harness 能力，而非数据、奖励或环境设计。
- 假设“生产力任务”（视频编辑、翻译、MV 生成等）是可被统一模型覆盖的同质族。

【4｜证据强度校准】以上全部来自摘要层面的问题陈述。可获取材料中没有任何对“已有 omni 模型具体弱在哪”的定量诊断、失败案例归因或消融证据。因此动机的严谨程度、以及上述子问题是否被方法分别回应，必须回到原文 Introduction / Method 与消融实验逐一核对。

Q2: 有哪些相关研究？

【总说明】本次可获取的材料仅含标题、作者、摘要与元数据，论文 Related Work 章节文本不在其中。以下只能依据摘要的定位语句反推技术谱系，具体引用对象（模型名、benchmark 名、论文列表）必须回原文核对，切勿据本段引用。

【1｜摘要显式界定的对照组：此前的 omni 模型】摘要以“Compared with previous omni models, which primarily emphasized perception and interaction”直接设定对标群体——一类以多模态感知与实时交互为主卖点的 omni（any-to-any）模型。差异化主张是：这类模型在“看得见、听得见、能对话”上达标，但在“想得清、能长程干活”上未达标。（合理推断：该群体通常包含语音对话类 omni 模型与音频-视频统一理解模型；具体名单需查原文）

【2｜架构谱系：sparse MoE 与长上下文】摘要明确写出“inherits the sparse mixture-of-experts (MoE) architecture of Qwen3.8-Next”，说明工作直接继承同代际稀疏 MoE 语言主干，并扩展到 one million tokens 上下文。相邻研究线包括：(i) 稀疏 MoE 的专家路由、负载均衡与 scaling 规律；(ii) 超长上下文建模与位置编码外推；(iii) 长上下文多模态理解。本工作的增量在于把三条线汇合到 agentic 场景，而非提出全新架构组件。（推测，摘要未提示新的架构创新点）

【3｜多模态训练的范式竞争：从拼接式到原生共训】摘要强调“native multimodal co-training strategy”，这是对更大范式竞争的回答：早期做法多将视觉/音频编码器外挂到语言模型（modular / 后接式），代价是跨模态推理弱、模态间能力难对齐；原生共训主张在预训练期就混合模态。摘要未披露共训的数据配比、模态采样策略、是否使用统一离散 token 或连续特征融合——这些是判定其范式归属的关键，需查 Method。

【4｜Agent 基础设施谱系：harness 与 plugin 生态】Qwen-MM-Plugins（多模态生产力插件框架）与 Qwen-Live-Harness（实时多模态 agent 框架）属于“LLM agent 工具调用 / function-calling 基础设施”“多 agent 编排与子 agent 委派”“上下文与记忆管理”这一方向。论文的定位差异由摘要直接支持：现有 harness 生态以文本与图像为主，缺乏对音频/视频的原生一等公民支持。

【5｜生产力应用谱系】摘要列出的场景分别对应：视频编辑 agent、长音频/长视频翻译（含同传与长文翻译）、music-conditioned video/movie generation（音乐驱动视频生成）、以及从视频中抽取可复用技能的 skill induction / 视频笔记。与这些方向的主流做法相比，本工作的主张是用单一模型 + 统一 harness 覆盖，而非维护多个专用模型。

【6｜评测谱系（推测）】摘要称评测覆盖多模态理解、推理、长时程 agent 执行与视频生产力任务，暗示评测同时包含学术式多模态理解 benchmark 与端到端生产力/agent 轨迹评测两类。两者的可比性与可复现性（尤其 agent 任务）是读原文时需要重点核查之处。

Q3: 论文如何解决这个问题？

【1｜模型层：原生多模态共训 + MoE 主干 + 百万上下文】摘要给出的技术骨架为三点：(a) native multimodal co-training strategy——设计目标双约束，既要保留强文本域能力，又要促成 agent 能力从文本迁移到音频与视频；(b) 继承 Qwen3.8-Next 的 sparse mixture-of-experts 架构；(c) 上下文窗口扩展至 one million tokens，用于长上下文多模态推理与长时程规划。摘要未披露共训的数据构成、模态混合比例、编码器/分词方案、注意力或位置编码改造、MoE 专家规模与路由策略，这些均为需回原文 Method 确认的关键缺口。（合理推断：共训在此被当作能力迁移的机制，而非单纯的数据增广）

【2｜系统层一：Qwen-MM-Plugins】针对“现有 agent harness 缺原生音频/视频支持”的问题，论文开源一个轻量插件框架，用于多模态生产力场景。其角色是把模型的音视频能力以插件形式接入既有 agent 运行时，从而绕开对 harness 的侵入式改造。（合理推断：插件很可能封装音视频读写、时间轴操作、转写/翻译、渲染等能力；具体插件清单摘要未给）

【3｜系统层二：Qwen-Live-Harness】论文把实时多模态交互重新定义为系统级挑战，需要三类编排能力协同：上下文与记忆管理、工具使用、子 agent 委派。Qwen-Live-Harness 即基于 Qwen3.8-Omni-Flash 构建的开源实时多模态 agent 框架。这是一个显式的方法论主张：实时性不作为模型单点指标，而作为系统指标（时延预算分担到记忆检索、工具调用与委派调度上）。（推断强度：定义层次的表述由摘要直接支持；具体调度机制属推测）

【4｜部署形态与应用面】模型被定位为可插入生产工作流的“primary agent 或 specialized sub-agent”，应用面包括：视频编辑、长音频与长视频翻译、以音乐为条件的 MV/电影生成、基于视频的笔记或 omni-skill 创建。摘要直接支持这些场景列举，但每个场景的具体实现路径（是否通过插件、是否需要人工确认、是否有回滚机制）未披露。

【5｜与既有路线的实质差异】相较“感知优先”的 omni 模型，本工作把能力重心从感知/交互迁移到理解/推理 + 长程执行；相较“文本 agent + 外挂视觉模块”的组合式方案，本工作主张单一原生多模态主干加统一 harness；相较只发布模型的路线，本工作同时交付模型、插件框架与实时框架三件套。

Q4: 论文做了哪些实验？

【1｜摘要声称的评测覆盖面（原文仅此一句口径）】“Extensive evaluations demonstrate that Qwen3.8-Omni-Flash achieves strong performance across multimodal understanding, reasoning, long-horizon agentic execution, and video productivity tasks.” 即四类评测：多模态理解、多模态推理、长时程 agentic 执行、视频生产力任务。

【2｜四类评测各自可能包含的子维度（合理推断，非原文）】
- 多模态理解：图像/视频/音频的内容识别、时序定位、跨模态对齐类任务。
- 推理：需要多步或跨模态联合推理的任务，可能是学科/图表/长视频因果类。
- 长时程 agentic 执行：多步工具调用、任务分解、长轨迹完成率、子 agent 委派成功率。
- 视频生产力：视频编辑指令遵循、长音频/长视频翻译质量、音乐条件视频生成、视频笔记/技能抽取的可用性。

【3｜缺失的评测信息（必须在原文核对）】摘要未给出任何具体数据集名、baseline 名单、评测指标定义、样本量、推理预算（是否 allow tool use、thinking budget）、pass@k 或人工评审协议，也没有给出具体数值。因此不能从摘要推断其相对优势幅度，也无法判断“strong performance”是与谁比较、在何种设置下成立。

【4｜可复现性相关的核对清单】
- 文本域能力是否单独报告（用于验证“preserves strong text-domain capabilities”是否可量化）。
- 与同代际纯文本模型的对照是否受控（同数据、同算力、同解码设置）。
- 长时程 agent 评测的环境是否开源、是否为自建私有任务集（自建任务集需警惕过拟合与选择偏差）。
- 视频生产力任务是否包含人工评估、评估者间一致性如何、是否报告失败率与拒答率。
- 是否报告时延/吞吐（与 Qwen-Live-Harness 的“实时”主张强相关）。
- 是否报告安全与版权相关边界（视频编辑、音乐条件生成、翻译场景均有内容合规问题）。

【5｜证据强度声明】本字段的“实验设计”部分无法从可获取材料中还原，上述均为基于摘要口径的推断与核对清单，不构成对论文实验内容的陈述。

Q5: 发现了什么实验现象？

【1｜可获取材料中的“现象级”表述】摘要只给出四个方向的定性结论（多模态理解、推理、长时程 agent 执行、视频生产力“均强”），没有报告任何具体数值、消融趋势、scaling 曲线或失败案例。因此本字段只能区分“摘要显式陈述的观察”与“需要回原文验证的现象假设”。

【2｜摘要显式陈述的观察性论断】
- 能力迁移现象：agentic 能力可从文本迁移到音频与视频任务，且文本域能力可被同时保留。这是一个双条件下的现象断言（迁移有效 + 无显著遗忘），本质上是消融问题：若去掉共训、或改变模态配比，迁移收益与文本能力会如何变化？（合理推断其为关键消融点）
- 系统级瓶颈现象：现有 agent harness 缺乏原生音频/视频支持，是能力无法落地的直接原因。这是一个关于“瓶颈位置”的判断——瓶颈在 harness 而非模型。若成立，意味着纯模型侧优化已经边际递减。
- 长上下文与长时程的耦合：百万 token 上下文被明确与长时程规划绑定，暗示存在“上下文长度 → 规划深度/任务完成率”的正向趋势。（合理推断：这可能体现为 scaling trend，需在原文找对应曲线）

【3｜值得在原文中重点查验的反直觉/张力点】
- 张力一：模态能力跷跷板。共训通常会在某一模态上带来增益与在另一模态上的损失，论文声称“保留文本能力”，需核对文本 benchmark 是否真无回退，以及回退是否被平均指标掩盖。
- 张力二：上下文长度 vs 有效利用率。扩到 1M token 与“真正在 1M 上做有效推理”是两件事；需查是否报告长上下文检索/定位类评测（如 needle-in-haystack 变体）以及有效上下文长度。
- 张力三：实时性 vs 质量。实时多模态交互与深度推理天然冲突，系统级编排（记忆管理、工具委派）可能以牺牲单轮质量换时延；需查是否报告时延-质量折中曲线。
- 张力四：agent 任务评测的自治度。长时程成功率高有可能来自强脚手架（harness 提供大量提示与纠正），而非模型本身；需查是否有“裸模型 vs harness 加持”的对照。
- 张力五：生产力任务的评估口径。视频编辑、MV 生成等任务缺乏公认指标，需查其用何种代理指标，以及是否报告生成失败/任务放弃的比例（负结果常见但不常被报告）。

【4｜负结果与失败模式的可见度】摘要未提及任何失败案例、拒答率、退化场景或安全边界。对于同时宣称推理、长程执行与生成能力的工作，缺少失败模式披露本身就是一个需要在阅读时主动寻找的信号。（推测：完整版可能位于 Discussion / Limitations 或附录案例）

Q6: 有什么可以进一步探索的点？

【1｜能力迁移的机制问题】“文本 agent 能力迁移到音频/视频”目前是效果层面的主张。可进一步探索：迁移发生在哪一层（表示层、专家路由层、还是数据分布层）；是否存在模态不对称（例如视觉→音频可迁移但音频→视觉不可）；共训配比与迁移收益的定量关系曲线；以及迁移失败的最小反例构造。

【2｜长上下文的“有效长度”与记忆表示】1M token 窗口与长时程规划绑定后，值得追问：多模态长上下文的注意力是否出现模态偏置（例如视频 token 挤占文本推理能力）；是否需要显式的分层记忆（短期/长期、可检索/可压缩）而非单纯扩大窗口；长视频场景下的时间轴索引结构是否比原始 token 序列更有效。

【3｜实时交互的系统级优化空间】论文把实时性定义为系统级挑战，后续可探索：时延预算在记忆检索、工具调用、子 agent 委派之间的最优分配；流式多模态输入下的增量推理与提前退出策略；以及在保持响应性的同时维持深度推理的调度策略（何时“快答”、何时“深思”）。

【4｜Agent harness 与插件生态的开放问题】Qwen-MM-Plugins 与 Qwen-Live-Harness 的开源为社区实验提供了基座。可探索：插件权限与沙箱化（音视频文件读写、渲染、发布皆涉及权限与版权）；插件组合的冲突检测；跨 harness 的互操作标准；以及 harness 层面的可复现实验协议（任务集、随机性控制、日志格式）。

【5｜评测方法学的缺口】多模态 agent 的长时程评测目前缺少公认基准。可探索方向包括：可复现的长轨迹任务集构建方法、过程性指标（而非只看终态成功）、成本归一化指标（token/时间/工具调用次数）、以及人工评估与自动评估的一致性研究。

【6｜安全、版权与滥用边界】视频编辑、长视频翻译、音乐条件生成三类应用同时涉及版权、肖像权与内容合规。可探索：生成内容的来源标注、翻译/配音的同意机制、编辑操作的不可逆性防护，以及模型在敏感请求下的拒答与降级行为。

【7｜与生物医学/科学场景的交叉机会（相对当前研究画像的延伸）】omni-agent + 长上下文 + 音视频生产力的组合，在实验室与临床场景中有直接映射：手术/操作视频的结构化笔记与技能抽取（对应摘要中的“video-based note / omni-skill”）、长时程实验记录的跨模态检索、以及多模态病例/影像-语音-文本联合推理。这类迁移的关键风险在于领域数据分布与评估标准差异，需要领域特定的失败模式分析，而非直接沿用通用结论。

Q7: 总结一下论文的主要内容

【材料边界说明】本次可用于归纳的材料只有标题、作者、摘要与元数据；PDF 正文（Introduction / Method / Experiments / Discussion）与检索证据均不可得。因此以下“论证主线、技术主线、实验主线”均在摘要明确支持范围内复述，并对推断部分逐处标注。

【1｜论证主线】论文的问题设定是：已有的 omni 模型把能力重心放在感知与交互，能够看、听、对话，但在多模态理解与推理、以及长时程 agentic 执行上不足，因而无法进入真实的多模态生产工作流。基于这一判断，作者的论证链条是：(i) 需要同时提升多模态理解/推理与长时程 agent 执行，而不是继续加码感知；(ii) 文本域已经积累出成熟的 agent 能力，应通过原生多模态共训把这种能力迁移到音频与视频，同时不牺牲文本域能力；(iii) 长时程规划与长音频/长视频处理要求极长上下文，因此把窗口扩至一百万 token；(iv) 即便模型具备这些能力，现有 agent harness 缺乏原生音频/视频支持，能力仍无法落地，所以必须同时交付插件框架；(v) 实时多模态交互本质上是系统级问题，需要上下文与记忆管理、工具使用、子 agent 委派的联合编排，因此需要专用实时框架。第 (iv)(v) 两步把一篇“模型论文”扩展成“模型 + 系统 + 生态”的三件套主张，这是本工作最显著的结构特征。

【2｜技术主线】技术上可还原的要点有三：其一，主干架构继承 Qwen3.8-Next 的稀疏 MoE 结构（sparse mixture-of-experts），说明本工作沿用同代际的 MoE 语言基座而非另起炉灶；其二，训练策略是 native multimodal co-training，其明确的双重目标为“保留强文本域能力”与“促成 agent 能力从文本向音频、视频迁移”；其三，上下文窗口扩展至 one million tokens，直接服务于长上下文多模态推理与长时程规划。在系统侧，Qwen-MM-Plugins 作为轻量开源插件框架承接多模态生产力（把音视频能力接入既有 agent 运行时），Qwen-Live-Harness 作为开源实时框架承接低时延多模态交互（合理推断：通过记忆管理、工具调用与子 agent 委派来分担时延预算）。摘要未披露数据配比、编码方案、专家规模、路由策略、注意力改造或位置编码外推方法，这些均是方法层面必须回原文确认的空白点。

【3｜实验主线与结果口径】摘要只给出四类评测维度的定性结论：多模态理解、推理、长时程 agent 执行与视频生产力任务上均取得“strong performance”。没有数据集名、基线、指标、数值、消融或时延数据。因此可以确认的“结果”仅限于维度覆盖，而不能确认任何相对优势幅度。

【4｜交付物与部署定位】模型被定位为可嵌入生产工作流的主 agent 或专用子 agent，应用场景包括视频编辑、长音频与长视频翻译、以音乐为条件的 MV 或电影生成、基于视频的笔记与 omni-skill 创建。配套开源 Qwen-MM-Plugins 与 Qwen-Live-Harness。

【5｜需要在原文中重点验证的三件事】
- 共训迁移是否真的“无损文本”：文本域 benchmark 是否单独报告且无回退。
- 1M 上下文是否等价于有效的长时程能力：是否报告有效上下文长度与长上下文检索类评测。
- agent 任务成绩中有多少来自 harness 脚手架：是否存在“裸模型 vs 完整系统”的对照。

【6｜命名与元数据一致性问题（不构成对论文内容的判断，仅提示核对）】标题为“Qwen3.8-Omni”，摘要正文一律写作“Qwen3.8-Omni-Flash”，二者的从属关系（Flash 是否为该系列的一个型号）需在原文确认；摘要引用的基座名“Qwen3.8-Next”与常见命名习惯存在差异，建议以原文为准；另外元数据给出的发布年份与常见 arXiv 编号惯例存在时间上的不一致，建议核对 arXiv ID 与版本号后再行引用。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与当前研究画像中的 agent 方向直接重合（权重 0.10）：本工作把 agent 能力迁移、长时程执行、子 agent 委派、工具调用编排作为一等议题，且明确提出“实时多模态交互是系统级问题”的方法论立场，适合作为 agent 系统研究的参照样本

## 基本信息

- 作者：Qwen Team
- 机构：Qwen Team（依据作者署名推断为阿里巴巴通义千问团队；元数据中 institution 字段为空，需以原文单位页确认）
- 来源：arxiv
- 主题/分类：cs.CL, cs.CV, cs.MM
- 日期：2026-09-22
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.25611v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成未获得任何 PDF 语义检索证据（retrieved_evidence 与 field_evidence_map 均为空、sections 各字段为空），全部内容仅基于标题、摘要与元数据推演，并已在各字段内逐条标注“摘要显式支持 / 合理推断 / 推测”的证据强度。
