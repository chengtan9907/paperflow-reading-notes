---
user_id: "cheng tan"
paper_id: 16441
arxiv_id: "2610.26826v1"
title: "RefCraft: Benchmarking Visual-Reference-Conditioned Agentic Generation through Web Development"
institution: "依据论文脚注，“Work done during an internship at Alibaba ATH”，即第一作者在阿里巴巴 ATH 实习期间完成该工作；Hongsheng Li 与 Shuai Bai 为通讯作者。元数据中 institution 为 null，其余作者的机构归属未在检索片段中给出（推测可能包含高校与阿里巴巴体系内团队，需回原文作者信息页确认）。"
publish_date: "2026-10-19"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.26826v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.26826v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-20T13:01:04"
---
# RefCraft: Benchmarking Visual-Reference-Conditioned Agentic Generation through Web Development

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agentic generation · visual reference conditioning · webpage development benchmark · multimodal llm evaluation

## 一句话总结

RefCraft 构建了一个包含 300 个“自然语言指令 + 1–5 张参考网页截图”多模态网页开发任务的智能体基准，要求模型在渲染—视觉检查—迭代精修的闭环中选择性利用视觉参考，对九个前沿 (M)LLM 评测后最高综合得分仅 52.12/100，表明视觉参考条件化的智能体生成仍是前沿模型远未解决的能力。

## 摘要

> Beyond basic tool use and coding skills, effectively incorporating visual references
> into agentic generation is equally essential for applying AI agents to real-world
> productivity tasks. We study this crucial capability through webpage development,
> a practical and readily evaluable setting in which existing benchmarks remain
> largely idealized: they typically focus on reconstructing visual targets or gener-
> ∗Work done during an internship at Alibaba ATH.
> † Hongsheng Li and Shuai Bai are corresponding authors.
> 1
> REFCRAFT: Preprint Under Review
> ating webpages from textual instructions alone, overlooking the more common
> workflow of developing from visual examples. Specifically, we introduce RefCraft,
> a benchmark comprising 300 realistic multimodal webpage-development tasks that
> combine natural-language instructions with screenshots from one to five reference
> webpages. Agents must selectively interpret and utilize these references while ren-
> dering, visually inspecting, and iteratively refining their outputs. We evaluate the
> resulting open-ended artifacts using independently verifiable keypoints and com-
> plementary programmatic and VLM judges. Across nine frontier (M)LLMs, the
> strongest system achieves an overall score of only 52.12 out of 100, revealing that
> visual-reference-conditioned agentic generation remains a substantial challenge for
> current models in practical webpage-development settings.

Q1: 这篇论文试图解决什么问题？

论文瞄准的问题是：视觉参考条件化的智能体生成（visual-reference-conditioned agentic generation）这一关键能力被现有基准系统性忽略。人类专家在真实生产力任务中普遍遵循“先研究成功范例，再在视觉检查中反复打磨”的工作模式——设计师参考成功案例后产出新方案，CAD 工程师检视现有零件来设计新零件。作者据此认为，仅具备工具调用与编码能力不足以覆盖完整生产力能力谱系，把视觉参考的理解、选择性采信和检查闭环纳入智能体生成同等重要。

现有网页相关基准在这一能力上存在两个系统性缺陷，使问题从未被有效度量：（1）多数基准只提供自然语言指令，无任何视觉参考，模型无从展示“看懂例子并加以利用”的能力；（2）另一类基准退化为重建任务（reconstruction），要求模型对照目标截图尽量复原，这与真实开发中“参考多个页面、选择性借用、再创造性改造”的工作流不符。真实工作流更常见的是“从视觉示例开发”：用户给出若干参考截图加文字要求，模型要自行判断哪些设计元素该借用、哪些该调整，并在产出后做视觉自检与迭代。论文用“largely idealized”概括现有基准，正是针对这两点。

作者选择网页开发作为测试床，理由有三：网页开发在数字生活中无处不在；其输出是高度视觉化的工件，便于人类与模型检查；网页结果（代码、截图、可交互功能）都是可验证证据，评估相对容易。在此场景下，RefCraft 的任务设计迫使智能体具备完整闭环能力：选择性解读 1–5 张参考截图（不能盲目复制）、把它们与自然语言指令融合成草案、渲染出网页、通过视觉检查发现问题、再迭代修改。

评测层面同样存在难题：开放式生成产物没有标准答案，不能只用单一整体指标。论文的解法是把每条查询分解为“可独立验证的关键点”，再用程序化规则与 VLM 裁判互补打分。这一“可验证关键点”设计本身也是问题意识的一部分：它希望把开放式任务的评估从主观整体打分转化为可复核、可追溯的细粒度判定，从而支撑跨模型比较与归因分析。

Q2: 有哪些相关研究？

1. 网页任务与网页智能体基准：论文指出已有大量网页相关基准，但绝大多数只用自然语言指令（引言中引用 Lu et al., 2026; Zhu et al., 2025 等），不提供视觉参考；另一类专注重建任务，给定目标截图要求复原。RefCraft 与它们的核心区别是引入“参考条件 + 选择性利用 + 检查迭代”的真实开发工作流，而不是“文字→页面”或“截图→复原”的单向映射。

2. 智能体生成与编码助手：引言以近期 (M)LLM 编码/协作助手为背景（OpenAI 2026b; Anthropic 2026a; Qwen Team 2026b; Kimi et al., 2026），并提到 GPT-6 Astra（OpenAI 2026d）与 Claude Fable 5.1（Anthropic 2026b）等前沿系统向更广泛生产任务的扩展。RefCraft 属于“用可验证基准约束和评测这类新能力”的路线。

3. 视觉参考条件生成：与文生图、UI 生成中的 reference-conditioned generation 在输入形式上有关联，但本文强调 agentic 设置——模型不是在单步中完成生成，而是在多轮工具调用中渲染、检查、修改；这与真实开发中的“初稿—审视—打磨”更接近，也对应智能体评测从静态指标向过程指标演进的趋势。

4. 网页模板与开源生态：基准参考池来自 Colorlib、Start Bootstrap、Creative Tim、AdminLTE、Themefisher、CodeWithSadee 等开源模板站点，保留 367 个 MIT、76 个 CC BY 3.0、1 个 Apache-2.0 许可页面，并记录来源 URL、作者与许可。这属于“用真实人工设计而非纯合成数据构建视觉参考池”的路线，与全合成 UI 数据基准形成方法论对照。

5. 智能体评测方法论：评测采用关键点分解 + 程序化判分器 + VLM 裁判的互补组合；附录 A.6 中的跨 harness 敏感性分析（OpenCode、Claude Code、Codex）则属于 agent 评测标准化讨论的一部分，检验主结论对运行环境选择的稳健性。

6. 注：论文引用的文献与模型名称（如 OpenAI 2026d、GPT-6 Astra 等）以原文标注为准，完整相关工作综述应回原文 Reference 部分核对；本字段基于引言与检索片段归纳，存在覆盖不全的可能。

Q3: 论文如何解决这个问题？

RefCraft 的解决方案由“基准构建三阶段 + 智能体运行协议 + 互补评测协议”三部分组成。

1. 基准构建三阶段：
（1）参考池构建（§3.2）：从 Colorlib、Start Bootstrap、Creative Tim、AdminLTE、Themefisher、CodeWithSadee 等模板源收集真实网页，最终保留 367 个 MIT、76 个 CC BY 3.0、1 个 Apache-2.0 许可页面，逐页记录来源 URL、作者与许可，为任务合成提供多样且合规的视觉参考素材。
（2）可控用户查询合成（§3.3）：把参考页组织成“自然语言指令 + 1–5 张截图”的用户请求。检索到的任务示例揭示了典型形态：“借用 A + B。粗体字、通透的表格；白色卡片、绿色按钮和蓝色细节。构建 + 检查。链接报价和预订；桌面、平板和手机。船体选中报价 (AUD)”——即指令明确要求选择性借用多个参考的设计元素，并指定设备形态（桌面/平板/手机）与交互功能。查询合成存在多种方式（片段显示“template / synthetic / specification”等列名残片，具体定义需回原文确认）。
（3）关键点分解（§3.4）：把每条查询分解为可独立验证的关键点。关键点由 Claude Opus 5 生成、GPT-5.6 Sol 验证；构建与审查时考虑参考使用一致性（reference-use consistency）与可验证性（verifiability），审查者可返回修订版关键点并说明修改理由与未解决问题，保证评估标准本身可审计；评分规则与聚合规则固定，不允许智能体修改。

2. 智能体运行协议：九个模型统一使用 OpenCode v1.18.25 harness，高推理强度，最多 100 轮交互；每个工作目录与基准仓库隔离，禁止访问仓库或评估关键点，但保留网络访问以贴近真实开发环境。任务形态为“从参考与指令产出初稿 → 渲染 → 视觉检查 → 迭代精修”的闭环。

3. 评测协议：总体分 S 由三个分量构成——关键点得分、参考使用得分（R）与核心功能得分（三个分量的具体定义与加权方式在检索片段中未完整呈现，需回原文确认）。参考保真度采用分级量表，例如 Good 级要求色差 <5%、间距偏差 1–2px、几乎不可分辨；“对应偏差”级允许可量化偏差但整体设计仍可辨认。程序化判分器负责客观可验证项，VLM 裁判（Gemini 3.1 Pro）负责整体性与主观视觉项，两者互补以覆盖开放式产物的评估。

Q4: 论文做了哪些实验？

1. 主评测（§5.1–5.2）：对九个前沿 (M)LLM——GPT-6 Astra、Claude Fable 5、Qwen3.8-Max、Grok 4.6、Kimi K3、GLM-5.3、DeepSeek-V4-Pro、Seed2.1 Pro、Gemini 3.1 Pro——统一在 OpenCode v1.18.25 上以高推理强度、100 轮上限运行。关键点由 Claude Opus 5 生成并由 GPT-5.6 Sol 验证；VLM 裁判为 Gemini 3.1 Pro。报告总体分 S、三个分量分数，以及效率统计（token 用量、交互轮数、工具调用次数）。

2. 领域分解：把 300 个任务按八个高层应用领域（内容/媒体、活动/社区、数据/工具、组织、个人/作品集、商业、模型、品牌等，表头为 OCR 残片，部分领域名需回原文确认）报告任务级 Overall 得分（表 6），用于检验模型在不同应用场景下的难度差异。

3. 细粒度类别分析：按视觉维度（如调色板 Palette、布局 Layout）与需求类型做细粒度性能分析；检索片段显示调色板类得分（如 98.87 / 92.4 / 100 / 87.68）与布局类得分（82.42 / 83.1 / 75.49）存在明显差异，但对应图表文字不完整，精确含义需回原文确认。

4. 查询合成方式对比：存在按查询合成方式（模板 template / 合成 synthetic / 规格说明 specification 等）划分的得分表（如 GPT-6 Astra 82.7 / 78.8 / 80.7 / 91.6），用于检验任务生成方式带来的难度差异与分数稳健性。

5. 跨 harness 敏感性分析（附录 A.6）：在固定 32 任务子集上，用 Claude Code 与 Codex 两种替代 harness 重跑 GPT-6 Astra 与 Kimi K3，并把官方 OpenCode 产物用同一评估协议重新评分作为主 harness 基线，检验结论对 harness 选择的稳健性（该附录的具体数值结果未在检索片段中给出）。

6. 隔离与防作弊验证设置：工作目录与基准仓库隔离、禁止访问仓库或评估关键点、保留网络访问——作为实验协议的一部分被实现与说明，以保证任务不是在“看到答案”的条件下完成。

Q5: 发现了什么实验现象？

1. 整体远未解决：最优模型（GPT-6 Astra）综合得分 52.12/100，多数模型聚集在 40 上下，Gemini 3.1 Pro 低于 20。最高分与“可用水平”之间仍有很大距离，说明视觉参考条件化生成是当前模型的系统性短板。

2. 关键点得分与总体分之间的张力：关键点得分范围为 61.45–75.68，明显高于总体分且更集中——说明模型能满足许多单点需求，但难以同时保证核心功能与整体设计意图的实现。直观地说，“局部会做、整体不会”是普遍现象。

3. 互补的失败模式：某模型参考使用得分 R=65.00，却因核心功能得分仅 33.44 而在九个模型中排名第九；GPT-6 Astra 拥有最高的核心功能得分 94.89，但 R 反而较低。这说明“可靠实现”（把功能与细节真正做对）与“恰当利用参考”（把视觉参考用到位）是两个互补且近乎正交的能力维度，只看单一指标会误判模型；这也直接支撑了联合评测的必要性。

4. 效率与成绩解耦：Qwen3.8-Max 综合表现超过 Grok 4.6，且总 token 用量显著更少（0.68M vs 1.28M），轮数与工具调用也更少——说明高分不是靠蛮力多轮试错堆出来的，存在更高效的智能体策略。

5. 领域难度不均：任务级得分按领域差异明显。GPT-6 Astra 在活动/社区类最高（57.6），在个人/作品集或模型类最低（约 46.0–46.8）；Gemini 3.1 Pro 在所有领域都显著落后，最低值仅 11.3。基准的领域覆盖带来了可感知的难度梯度。

6. 评测设计的潜在混淆：Gemini 3.1 Pro 既是九个被评模型之一，又担任 VLM 裁判，且其自身得分远低于其他模型；这种“选手兼裁判”设置可能引入系统性偏差，检索片段未说明任何校正措施（需回原文确认）。

7. 保真度分级显示评测对参考忠实度有精细量化标准（Good 级要求色差 <5%、间距 1–2px、几乎不可分辨），这类近似可判的阈值设计有助于降低主观评分噪声，但也意味着“像素级接近参考”本身可被精确度量。

8. 跨 harness 稳健性检验存在（附录 A.6），但检索片段未提供数值结果，其结论方向（主结果是否随 harness 改变）需回原文确认。

Q6: 有什么可以进一步探索的点？

1. 能力提升方向：针对“选择性参考利用”做专门优化——模型需要学会区分可复用设计（布局骨架、色彩系统、组件样式）与必须重新设计的部分（文案、数据、品牌元素），并判断何时应该偏离参考；当前 R 与核心功能得分的割裂说明这两个能力需要联合优化。

2. 训练与后训练：把可独立验证的关键点作为奖励信号，进行强化学习或指令微调；利用程序化判分器提供密集、低成本的反馈，减少对昂贵 VLM 裁判的依赖，并探索“参考利用一致性”作为训练目标的可行性。

3. 评测方法论改进：研究关键点自动生成与校验的可靠流程，降低对专有模型（Claude Opus 5、GPT-5.6 Sol）的依赖；研究 VLM 裁判与被评模型重叠时的偏差校正；公开关键点构建协议、聚合权重与评分细则，提升可复现性。

4. 规模与多样性扩展：从 300 任务扩展到更大规模；补充更多模板源与风格（非英语页面、不同文化设计风格、暗色模式、动效页面）；从静态页面扩展到动态应用（登录、状态管理、API 交互、实时数据），提高任务生态真实性。

5. 闭环能力的细粒度研究：把“渲染—视觉检查—迭代”拆解为可度量的子技能（首稿质量、检查频率、修改有效性、停止时机），研究 100 轮上限内的 scaling 行为——多轮迭代是否持续带来增益、收益何时饱和，以及不同模型在“检查质量”上的差异。

6. 跨 harness 与跨框架标准化：当前敏感性分析仅在 32 任务、2 模型上进行；可扩展到更多 harness（Claude Code、Codex、自定义框架）与更多模型，形成系统化的 harness 效应报告，并探索与任务难度、模型能力交互的 harness 效应。

7. 跨领域迁移：把同一“视觉参考条件化智能体生成”范式搬到 CAD、UI/产品设计、文档与幻灯片生成、科学图表与实验报告生成等场景（与 ai-for-science 用户画像相关的场景尤其值得对照），检验该能力是通用维度还是网页专属技能。

8. 真实工作流对齐：加入协作角色（设计师/开发者分工）、版本管理、外部用户验收与多轮反馈，使基准更贴近真实生产力环境，并研究“参考污染”——模型过度复制参考内容时的版权与原创性问题。

Q7: 总结一下论文的主要内容

论文的核心主张是：在真实生产力任务中，“把视觉参考纳入智能体生成”是与工具调用、编码同等重要的能力，但现有基准没有度量它。作者以网页开发为测试床，提出 RefCraft 基准与配套评测协议，并对九个前沿 (M)LLM 做了系统评测。

动机与问题（引言）：(M)LLM 作为编码与协作助手已极大提升生产力，并正向更广泛任务扩展；但审视真实工作流（设计师先研究成功案例再迭代；CAD 工程师检视现有零件再设计新件）可见，视觉参考的理解与检查闭环是不可或缺的一环。现有网页基准要么只给文字指令、要么只做目标重建，都“理想化”地回避了“从视觉示例开发”的常见工作流。RefCraft 的目标是补上这一缺口。

基准构成（§3）：300 个多模态网页开发任务，构建分三阶段。①参考池：从 Colorlib、Start Bootstrap、Creative Tim、AdminLTE、Themefisher、CodeWithSadee 等采集开源模板，保留 367 个 MIT、76 个 CC BY 3.0、1 个 Apache-2.0 许可页面，记录 URL、作者与许可；②查询合成：把 1–5 张参考截图与自然语言指令组合成可控、真实的用户请求（示例任务要求“借用 A+B”的粗体与通透表格、白色卡片、绿色按钮、蓝色细节，构建链接报价与预订的桌面/平板/手机响应式页面）；③关键点分解：每条查询由 Claude Opus 5 生成、GPT-5.6 Sol 验证的关键点集合进行评分，审查时考虑参考使用一致性与可验证性，允许修订并说明遗留问题。

任务形态（agentic 设置）：模型在隔离工作目录中运行（禁止访问基准仓库与关键点，保留网络访问），通过 OpenCode harness 完成“从参考和指令产出初稿 → 渲染 → 视觉检查 → 迭代精修”的闭环，上限 100 轮。

评测协议（§5/附录）：总体分 S 由关键点得分、参考使用得分（R）与核心功能得分三个分量构成；参考保真度用分级量表（Good 级要求色差 <5%、间距 1–2px、几乎不可分辨）；程序化判分器与 VLM 裁判（Gemini 3.1 Pro）互补覆盖客观与主观维度。

主结果：GPT-6 Astra 以 52.12 领先，多数模型约 40，Gemini 3.1 Pro 不足 20；关键点得分集中且偏高（61.45–75.68），但核心功能与整体设计意图难以同时达成。出现互补失败模式：R=65.00 的模型因核心功能仅 33.44 而排名第九，GPT-6 Astra 核心功能达 94.89 而 R 偏低——“可靠实现”与“参考利用”是两个近似正交的能力。效率上 Qwen3.8-Max 以 0.68M token 超过 Grok 4.6（1.28M）。领域分解显示个人/作品集等域对多数模型最弱，Gemini 在所有域都明显落后（最低 11.3）。

稳健性分析（附录 A.6）：在固定 32 任务子集上用 Claude Code 与 Codex 两种 harness 重跑 GPT-6 Astra 与 Kimi K3，并用同一协议重评 OpenCode 产物作基线，用于检验结论对 harness 的敏感性（该附录具体数值未在检索片段中给出，需回原文确认）。

结论与定位：作者希望 RefCraft 成为“视觉参考条件化智能体生成”这一被忽视能力的测试床，并刺激后续研究。论文整体属于“能力定义 + 基准构建 + 大规模评测 + 稳健性分析”的系统性工作，其可验证关键点与双裁判评测范式具有方法论参考价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像第一方向 agent 直接相关：本文度量的是“参考条件 + 渲染检查 + 迭代精修”的智能体闭环能力，而非单次生成质量。

## 基本信息

- 作者：Yuchi Wang, Rongyao Fang, Weihao Yu, Xilin Wei, Tiancheng Gu, Dingkang Yang, Hang Zhang, Dayiheng Liu, Hongsheng Li, Shuai Bai
- 机构：依据论文脚注，“Work done during an internship at Alibaba ATH”，即第一作者在阿里巴巴 ATH 实习期间完成该工作；Hongsheng Li 与 Shuai Bai 为通讯作者。元数据中 institution 为 null，其余作者的机构归属未在检索片段中给出（推测可能包含高校与阿里巴巴体系内团队，需回原文作者信息页确认）。
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.CV
- 日期：2026-10-19
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.26826v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考 retrieved_evidence 的语义检索命中片段（introduction/method/results/conclusion 共 65 chunks、命中分数约 0.79–1.09）与 field_evidence_map 的字段锚点，并结合论文 abstract/sections 元数据补全；heuristic_draft 中 one_sentence_summary、core_method、main_contributions 等多处字段为不完整残片，已按证据重写；部分表格数值来自 OCR 残片（如领域表头、细粒度得分），精确值建议回原文复核。
