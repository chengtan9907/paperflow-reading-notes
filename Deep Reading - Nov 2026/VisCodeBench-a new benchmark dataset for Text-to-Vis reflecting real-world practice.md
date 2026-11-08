---
user_id: "cheng tan"
paper_id: 20317
arxiv_id: "2611.02353v1"
title: "VisCodeBench: a new benchmark dataset for Text-to-Vis reflecting real-world practice"
institution: "University of Sydney（依据 Conclusion 片段中的“...cs Committee at The University of Sydney”推断）；Cecile Paris、Andrew Reeson 可能隶属 CSIRO Data61（推测，需回原文核对完整机构列表）。"
publish_date: "2026-11-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.02353v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.02353v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-08T02:08:37"
---
# VisCodeBench: a new benchmark dataset for Text-to-Vis reflecting real-world practice

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：text-to-visualization · benchmark · llm code generation · chart generation

## 一句话总结

VisCodeBench 是一个面向真实实践场景的端到端 text-to-vis 基准，包含教育、科研与公众传播三个来源的 2,672 个样本，并以七项指标系统评估 Codex、Claude Code 与 LIDA 搭配多种开源或商业 LLM 的表现；核心结论是：基础执行已接近解决，数据忠实度仍远未解决，系统设计可与模型选择同等重要。

## 摘要

> Generating visualisations from natural language requests (text-to-vis) enables users to explore data more efficiently. While LLMs can complete simple visualisation requests, their performance on challenging ones is not well covered by existing benchmarks. We introduce VisCodeBench, an end-to-end text-to-vis benchmark with 2,672 samples from three different domains: educational, scientific, and general audience scenarios with varying levels of complexity. We benchmark three visualisation systems---Codex, Claude Code, and LIDA---paired with a range of open and commercial LLMs across seven metrics covering execution, chart content, and visual quality. While basic execution is near-solved for strong systems, no system comes close to reproducing the reference data, with the best displayed data similarity reaching only $0.81$ and falling to $0.69$ on our hardest subset for this metric. We further find that frontier models separate on chart correctness and efficiency, with ClaudeCode-Opus-4.7 improving chart correctness by around 5 points over Codex-GPT-5 while running roughly 20 seconds faster. Separately, we find that system design matters as much as the underlying model: despite both running on GPT-5, Codex clearly outperforms LIDA, showing that the surrounding prompting, execution, and refinement pipeline can shape outcomes as much as the model itself. Finally, latency remains a practical obstacle, as even the strongest systems take 39-78 seconds to produce results. VisCodeBench is available at \href{https://github.com/SAIPLL/VisCodeBench}{https://github.com/SAIPLL/VisCodeBench} and accessed via a leaderboard; the full dataset is kept private to prevent LLM training use.

Q1: 这篇论文试图解决什么问题？

【核心问题】论文要解决的不是“让 LLM 画出一张图”这一单点能力，而是“在真实数据与真实自然语言请求下，端到端完成可视化”的系统性问题。一个可用的 text-to-vis 系统必须读取数据文件、理解字段语义、执行过滤、聚合、排序、连接等转换、选择合适图表类型与编码通道、处理标签、刻度、配色与可访问性，并在代码报错时调试；这些环节任一出错都会导致图表“能跑但不对”。论文指出，LLM 已能完成简单请求，但在 challenging requests 上的表现没有被现有基准充分覆盖。
【现有基准的缺口】论文点名 nvBench、nvBench2.0、ChartX、Text2Chart31、MatPlotBench、Text2Vis，并批评多数数据集是合成或自动构造的，缺少有意义的真实世界上下文，生成代码常遵循固定模板；作者此前工作 Nguyen et al. 2024 也表明真实使用分布与基准分布不匹配。因此缺口不只是“样本更多”，而是任务分布、数据复杂度、请求自然度和评估目标都偏离真实实践。
【为什么难】真实请求往往欠定义，数据文件脏且异构，同一请求可能有多个合法图表；执行成功不等于数据正确；参考图表、代码或数据只是单点 ground truth；公开基准容易被 LLM 训练数据污染；视觉质量、可访问性与内容正确性可能相互冲突；延迟和成本直接影响交互可用性。
【论文的切入】VisCodeBench 从 GitHub 课程作业、arXiv 论文作者提供、Our World in Data 三个来源构造 2,672 个样本，请具有四年以上编程经验和丰富学术可视化经验的专家核验，并以七项指标做系统级而非单模型级评测。
【隐含假设与张力】合理推断：论文把参考图表、代码和底层数据当作忠实度金标准，但可用片段未讨论多解性与标注者一致性；displayed data similarity 可能奖励字面复现而非语义等价；chart correctness 可能混合图表类型、编码映射与数据正确性；三个领域不能代表企业 BI、实时仪表盘等场景；私有测试集虽防污染，却牺牲可复现性与细粒度错误分析。以上均需回 Method 与 Results 核实。

Q2: 有哪些相关研究？

【Text-to-Vis / NL2VIS 基准】论文在 Introduction 中列出的直接相关工作包括 nvBench、nvBench2.0、ChartX、Text2Chart31、MatPlotBench 与 Text2Vis。按名称与任务定位可粗略分为：自然语言到可视化查询或图表生成基准（nvBench 系列）、图表生成与图表理解混合基准（ChartX）、面向特定图表类型或代码库的生成基准（Text2Chart31、MatPlotBench），以及较新的 text-to-vis 端到端评测（Text2Vis）。论文只点名而未在可用片段中逐一展开规模、构造方式和指标，因此这里不虚构各基准的具体数字；其共同被批评的点是合成或自动构造、固定模板、缺少真实世界上下文。
【LLM 代码生成与可视化代理】VisCodeBench 把 Codex、Claude Code 与 LIDA 作为“系统”而非单纯模型来比较。Codex 与 Claude Code 属于代码代理式系统，具备读文件、写代码、执行与调试的流水线；LIDA 是面向可视化的 LLM 生成系统或库。论文的核心相关工作是“模型 + scaffold”的系统级评测，而不是只比较 LLM 的零样本代码能力。
【评估维度】已有工作常看执行成功率、图表类型匹配或视觉相似度；VisCodeBench 把执行、图表内容与视觉质量拆成七项指标，并引入 displayed data similarity、chart correctness、readability、accessibility 等更贴近真实使用的维度。这与此前以 BLEU、代码匹配或像素相似度为主的评估形成对照。
【与论文的关系】论文的定位不是提出新模型，而是指出“基准与真实实践脱节”这一评测基础设施问题，并用多源真实数据、专家标注和系统级比较来补位。相邻但未在片段中展开的方向包括：图表理解或 ChartQA 类基准、LLM-as-judge 评估、可视化可访问性评估，以及代码执行沙箱与 agent 评测。上述相邻方向为合理外推，需回 Related Work 核对论文实际引用范围。

Q3: 论文如何解决这个问题？

【任务定义】VisCodeBench 把 text-to-vis 定义为端到端任务：输入是底层数据与自然语言可视化请求，输出是可执行代码及其渲染出的图表。与只生成图表规格或单段绘图 API 调用的基准不同，系统需要自己完成数据读取、转换、图表生成与运行调试。
【数据来源】三个子集：1）GitHub-T2V，843 个样本，来自 GitHub 上在线课程使用的 assignment notebooks，对应教育场景；2）arXiv-T2V，805 个样本，来自学术论文，作者通过联系论文作者获取图表、代码、底层数据和一段描述该图表的自然语言请求，对应科研场景；3）OWID-T2V，1,024 个样本，来自 Our World in Data，一个公开的全球议题数据驱动分析平台，对应普通受众或公共传播场景。合计 2,672 个样本，复杂度不一。
【标注与伦理】收集原始数据后，由每位具有四年以上编程经验和丰富学术可视化经验的专家标注者核验。Conclusion 片段还显示，研究取得参与者知情同意，收集数据以 CC BY-SA 4.0 许可发布；外部数据源涉及 GitHub 与 Our World in Data。具体标注指南、一致性指标与样本字段需回原文确认。
【样本形态】从 Introduction 的图示可见，样本至少包含数据文件（如 CSV）、自然语言请求、参考代码与参考图表；示例中出现 pandas 读取数据、计算性别比例、用 matplotlib 画饼图的代码，以及 “Create a pie chart presenting the male/female proportion” 一类请求。合理推断：评测时把数据与请求交给系统，比较生成图表与参考图表或数据。
【评测系统】论文比较三个可视化系统：Codex、Claude Code 与 LIDA，并为它们搭配多种开源与商业 LLM。这里的“系统”包括提示、代码执行、错误修复或 refine 等外围流水线，因此可以分离“模型能力”与“系统设计”的贡献。
【七项指标】任务完成率与执行时间对应执行层；chart type accuracy、displayed data similarity、chart correctness 对应图表内容层；readability 与 accessibility 对应视觉质量层。displayed data similarity 衡量生成图中呈现的数据与参考数据的接近程度，论文报告最佳 0.81、最难子集 0.69。chart correctness 的具体判定方式（人工、规则还是 LLM judge）在可用片段中未说明，是重要证据缺口。
【发布方式】代码与入口在 GitHub，评测通过 leaderboard 访问；完整数据集保持私有，以防被用于 LLM 训练。这是典型的 contamination-aware benchmark 设计，但也带来复现与错误分析的限制。

Q4: 论文做了哪些实验？

【实验矩阵】论文在 2,672 个样本上评测三个系统（Codex、Claude Code、LIDA）与多种开源或商业 LLM 的组合，覆盖 GitHub-T2V、arXiv-T2V、OWID-T2V 三个子集，并用七项指标汇总。摘要明确给出的配对包括 Codex-GPT-5、ClaudeCode-Opus-4.7 与 LIDA-GPT-5；其他模型清单、参数量、版本号与调用方式在可用片段中未完整出现。
【对比维度】1）系统级比较：同一模型换不同 scaffold；2）模型级比较：同一系统换不同 LLM；3）前沿模型对比：chart correctness 与运行时间；4）子集难度：整体与 hardest subset 的 displayed data similarity；5）请求类型：检索片段出现 machine-generated requests 与人工请求的对比，涉及 GitHub-T2V 与 arXiv-T2V；6）多样性与覆盖面：用 GPT-2 tokenizer 统计输入与输出内容多样性，并讨论测试集规模与图表类型范围；7）视觉风格参数：片段中出现 pie-radius、errbar-cap-size、ann-fontsize 等样式参数表，说明评估可能细到图表样式属性。
【指标】任务完成率、执行时间、chart type accuracy、displayed data similarity、chart correctness、readability、accessibility。论文把它们归为执行、图表内容、视觉质量三类，但每个指标的精确公式、阈值、人工与自动判定比例未在可用文本中给出。
【实验控制】是否零样本、是否允许多轮修复、温度、最大 token、执行沙箱、依赖版本、超时设置、失败重试策略均未见说明；这会影响系统间比较的公平性。合理推断：Codex 与 Claude Code 作为 agent 可能具备执行与调试循环，LIDA 的流水线不同，因此“系统设计”效应与“模型”效应存在混杂，需要原文的消融来支撑因果解释。
【统计与稳健性】可用片段未报告多次运行的均值与方差、置信区间、显著性检验或人工标注者一致性；对于 0.81 与 0.69 这类差异，应谨慎看待其统计稳定性。论文的结论“system design matters as much as model”在方向上由 Codex 与 LIDA 的对比支持，但“as much as”是强度判断，需看完整表格与消融。
【附加分析】machine-generated request 实验显示，某些模型在机器生成请求下于 GitHub-T2V 与 arXiv-T2V 明显提升，而质量类指标大体不受影响；这提示基准对请求措辞敏感，也提示人工请求更难。arXiv-T2V 在输入与输出上多样性较高，可能解释其难度。以上来自检索片段，标题与表格上下文不完整，需回原文核对。

Q5: 发现了什么实验现象？

【执行接近解决，内容忠实度未解决】最强系统的基本执行已接近解决，只有小型本地模型在稳定产出有效图表上明显吃力；但没有任何系统能接近复现参考数据，最佳 displayed data similarity 仅 0.81，在最难子集上降到 0.69。这是全文最重要的张力：任务完成率或可运行性高，不等于图表呈现的数据正确；错误主要集中在数据读取、过滤或聚合、字段映射与图表编码，而非语法错误。
【前沿模型在正确性与效率上分化】ClaudeCode-Opus-4.7 相比 Codex-GPT-5 的 chart correctness 高约 5 分，同时运行快约 20 秒。这说明“更强模型一定更慢”的简单权衡不成立；模型能力、工具调用策略与执行流水线共同决定正确性-延迟前沿。合理推断：Claude Code 的 scaffold 可能减少了无效尝试或更有效地修复错误，但可用片段未给出机制解释。
【系统设计可与模型同等重要】同样运行 GPT-5，Codex 明显优于 LIDA。由于两者共享底层模型，差异只能来自提示、上下文组织、代码执行、错误反馈与 refine 策略。这个结果对“只换更大模型”的实践有直接反例意义：在 text-to-vis 中，scaffold 可能和模型选择一样关键；但“as much as”是作者基于特定配对的总结，是否可推广到其他模型仍需更多 factorial 实验。
【延迟是实际障碍】最强系统生成结果需 39–78 秒，对交互式数据探索偏慢。论文没有在可用片段中给出延迟-正确性散点或成本数据，因此无法判断是否存在“快但不够准”的可用折中点；这是后续阅读应重点核对的表。
【请求措辞影响结果】检索片段显示，在 machine-generated requests 下，实验模型在 GitHub-T2V 与 arXiv-T2V 上有显著提升，而质量类指标大体不受影响。合理解释是机器生成请求更结构化、更接近基准模板，人工请求则更欠定义；这既是基准敏感性，也是现实部署的提示：用户请求的自然度会显著改变系统表现。
【多样性与覆盖】arXiv-T2V 在输入与输出上均显示较高多样性；论文还强调测试集规模虽有限但支持更广的图表类型。多样性高可能同时意味着难度高与评估不稳定，需结合 per-subset 结果判断。
【指标间张力】chart type accuracy 可能较高而 displayed data similarity 较低；readability 与 accessibility 可能独立于数据正确性；执行时间短的系统可能牺牲修复轮次。这些张力说明单一“准确率”不足以概括 text-to-vis 质量，也说明 leaderboard 需要多指标并列而非总分掩盖。
【失败模式】可用片段未给出系统化错误分类，但结合任务定义可合理推断：数据加载或路径错误、聚合或过滤逻辑错误、字段语义误读、图表类型选择错误、标签、刻度、图例缺失、可访问性属性缺失、运行时异常与超时。论文若附失败案例，应优先阅读。

Q6: 有什么可以进一步探索的点？

【内容忠实度与可验证执行】最直接的方向是把“图表看起来对”推进到“数据可验证地对”：为生成代码加入单元测试或断言（行数、列和、聚合值、过滤条件），用执行轨迹检查数据转换，区分“代码能跑”“图表类型对”“呈现数据对”三层。可以研究 reference-free 或多参考的语义等价评估，避免单点参考图表惩罚合法替代方案。
【评估指标与裁判校准】displayed data similarity、chart correctness、readability、accessibility 的公式与判定标准需要公开并做消融；应报告人工一致性、LLM-as-judge 与人类判断的相关性，以及不同图表类型上的指标偏差。可引入多参考图表、人类偏好、任务效用（用户能否从图中回答数据问题）与可访问性标准（对比度、色盲友好、alt text）。
【系统与模型解耦】论文的 Codex 与 LIDA 结果提示 scaffold 效应很大，后续可用 factorial design 分离：固定模型换提示、执行或 refine；固定 scaffold 换模型；记录重试次数、工具调用与错误修复轨迹。进一步研究 planner-executor-verifier 架构、代码沙箱、自动调试、检索外部文档，以及何时向用户澄清需求。
【效率与成本】39–78 秒的延迟是产品化障碍；可探索并行工具调用、缓存、增量执行、早停、小模型路由、草稿-验证分离、成本感知 leaderboard。需要报告 p50/p95 延迟、token 成本与正确性的联合分布，而不只是平均值。
【交互与多轮设定】真实请求常欠定义；可研究多轮澄清、交互式编辑、对用户反馈的增量修正，以及“先给草图再精修”的工作流。这与 agent 方向高度相关。
【数据与防污染】私有测试集与 leaderboard 是正确方向，但需动态刷新、canary、版本化、许可审计与数据来源追踪；可扩展到多语言、多模态（表格、文本与图像）、企业 BI、实时仪表盘、移动端等场景。三领域之外的外部效度是明显空白。
【科学可视化与 AI-for-Science】arXiv-T2V 已显示科研图表的特殊性：统计图、误差棒、多面板、单位与标注都要求领域知识。后续可研究科学图表的数据 provenance、可复现性、统计正确性，以及与论文正文的一致性。
【可复现性与错误分析】应公开 prompt、模型版本、执行环境、依赖版本、评分脚本与失败案例；报告多次运行方差、置信区间与显著性；建立错误分类法（数据、转换、编码、样式、可访问性、运行时），让 benchmark 不只是排名，而是诊断工具。

Q7: 总结一下论文的主要内容

【定位】VisCodeBench 是一篇 benchmark 与 dataset 论文，目标是把 text-to-vis 评估从合成、模板化的图表生成，推进到更接近真实实践的多源、端到端评测。论文不提出新的生成模型，而是提出数据、任务协议与系统级比较。
【问题】text-to-vis 要求系统根据自然语言请求和底层数据生成可视化代码。LLM 已能处理简单请求，但现有基准（nvBench、nvBench2.0、ChartX、Text2Chart31、MatPlotBench、Text2Vis）多为合成或自动构造，缺少真实上下文，代码常遵循固定模板；作者此前工作也指出真实使用与基准分布不匹配。因此，简单请求上的高分不能说明系统能处理真实、复杂、欠定义的数据可视化任务。
【基准构造】VisCodeBench 含 2,672 个样本，来自三条链路：1）GitHub-T2V 843 个，来自 GitHub 上在线课程的 assignment notebooks，代表教育场景；2）arXiv-T2V 805 个，通过联系论文作者收集图表、代码、底层数据和自然语言请求，代表科研场景；3）OWID-T2V 1,024 个，来自 Our World in Data，代表普通受众或公共传播场景。样本复杂度不一，覆盖数据加载、转换与图表生成。专家标注者均具有四年以上编程经验和丰富学术可视化经验，负责核验；研究取得知情同意，收集数据以 CC BY-SA 4.0 发布。完整数据集私有，通过 leaderboard 访问，以防 LLM 训练使用。
【任务与评估】任务输入是数据与自然语言请求，输出是可执行代码和图表。论文用七项指标：任务完成率、执行时间、chart type accuracy、displayed data similarity、chart correctness、readability、accessibility；分别覆盖执行、图表内容与视觉质量。displayed data similarity 是关键内容指标，衡量生成图中呈现的数据与参考数据的接近程度。
【实验】论文比较 Codex、Claude Code、LIDA 三个系统，并搭配多种开源与商业 LLM，在三个子集上评测。摘要给出的代表性配对包括 Codex-GPT-5、ClaudeCode-Opus-4.7、LIDA-GPT-5。除主表外，还分析 hardest subset、请求类型（机器生成与人工）、内容多样性（GPT-2 tokenizer）与图表样式参数。
【发现】1）强系统的基本执行接近解决，小型本地模型仍难稳定产出有效图表；2）内容忠实度远未解决，最佳 displayed data similarity 0.81，最难子集 0.69，没有系统能很好复现参考数据；3）前沿模型在 chart correctness 与效率上分化，ClaudeCode-Opus-4.7 比 Codex-GPT-5 高约 5 分且快约 20 秒；4）系统设计可与模型同等重要，同样 GPT-5 下 Codex 明显优于 LIDA，说明提示、执行、refine 流水线影响巨大；5）延迟 39–78 秒，仍是实际障碍；6）机器生成请求会提升部分子集表现，提示基准对请求措辞敏感。
【贡献】论文的贡献可概括为：真实多源数据、端到端任务、专家标注、七指标系统级评测、防污染发布机制，以及若干反直觉发现（执行不等于正确、scaffold 约等于 model、正确性与效率可同时改善）。对 agent 评测、生成式可视化、AI-for-Science 与评测方法论都有参考价值。
【局限】可用全文片段缺少 Method 与 Results 正文，无法核验指标公式、prompt、执行环境、统计检验与完整模型清单；单参考 ground truth 的多解性问题、三领域外部效度、私有数据对复现的限制、模型版本时效性、延迟与成本权衡都需在原文中进一步确认。总体而言，这篇论文的价值更在“评测基础设施与系统级诊断”，而非单一模型刷分。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对 agent 方向：VisCodeBench 评的是“代码代理 + 执行 + 调试”的端到端系统，而不是单轮代码生成；Codex 与 LIDA 的结果直接说明 scaffold 效应，可作为 agent 评测设计的参考。

## 基本信息

- 作者：Hy Nguyen, Thomas James Elton, Ella Wilson, Andrew Reeson, Cecile Paris, Josiah Poon, Jonathan K. Kummerfeld
- 机构：University of Sydney（依据 Conclusion 片段中的“...cs Committee at The University of Sydney”推断）；Cecile Paris、Andrew Reeson 可能隶属 CSIRO Data61（推测，需回原文核对完整机构列表）。
- 来源：arxiv
- 主题/分类：cs.CL, cs.HC
- 日期：2026-11-02
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.02353v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 检索命中的摘要、Introduction、Conclusion 片段以及少量方法或结果线索；由于 Method 与 Results 正文缺失，涉及具体实现、完整模型清单与统计细节处均已标注证据缺口。
