---
user_id: "cheng tan"
paper_id: 20826
arxiv_id: "2611.12305v1"
title: "HumanSightBench: A Benchmark for Human Visual Perception in MLLMs"
institution: "ByteDance（字节跳动）与 Tsinghua University（清华大学）——依据通讯作者邮箱 suzixin.0803@bytedance.com 与 suzx26@tsinghua.edu.cn 推断；论文元数据未提供机构列表，需以原文作者单位为准。"
publish_date: "2026-11-09"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.12305v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.12305v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-11T02:07:10"
---
# HumanSightBench: A Benchmark for Human Visual Perception in MLLMs

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multimodal large language models · visual perception benchmark · human solvability · counting

## 一句话总结

论文提出 HumanSightBench——一个以「构造成本上人类可解、评测上模型难解」为准则、经五道准入闸门（清晰性、答案唯一、视觉接地、人类可解、纯文本捷径过滤）筛选的 2,876 题短答案视觉感知基准，用 pass@3 在 8 个 MLLM 上暴露了被总体分数掩盖的感知能力缺口（最好 65.6%，Counting 无人超过 27.5%）。

## 摘要

> 摘要可干净复述为六层：
> （1）现状：MLLM 在知识密集与推理型视觉语言基准上表现强劲，但仍无法解决人类轻松可解的视觉问题。
> （2）难点：直接比较 MLLM 与人类视觉感知很困难，原因有二——现有评测是端到端的，把多种技能耦合进一个分数；以及单条题目本身是否有效并不确定。
> （3）具体混淆源：观测到的错误可能来自视觉感知、语言先验、背景知识、问题歧义、答案未定义，这五者混在一起无法分离。
> （4）准则：HumanSightBench 的构造准则是一句话——items that are human-easy by construction and model-hard by evaluation。
> （5）产物：2,876 道短答案视觉问题，覆盖 7 个一级感知类别与 21 个二级子类；只有通过五道准入闸门（问题清晰、答案唯一、视觉接地、人类可解、纯文本捷径过滤）的条目才被收录。
> （6）结果：协议把 error 定义为「在被人类验证的视觉任务上、用被评测接口给出的错误答案」；闸门用于减少歧义与外部知识带来的混淆；八个被评测 MLLM 中最好的 pass@3 为 65.6%，Counting 上没有任何模型超过 27.5%；这些结果揭示了被总体分数掩盖的能力缺口，可用于后续训练实验与 held-out 评测。

Q1: 这篇论文试图解决什么问题？

一、表层问题：能力画像与叙事不匹配。MLLM 在 chart 解读、大学水平学科题等知识密集/多步推理型 VL 基准上持续刷新分数，但在普通成人几乎零成本完成的视觉判断上失败——数物体、沿线穿过交叉、读模糊标签、辨别镜像图案（引言引 [1,5,8,10,12,23,27,35]）。论文用具体例子锚定落差：给三只托盘，问一共能放多少鸡蛋，模型需要从可见的托盘结构推断总容量，而不是检索常识。这类判断是 human-facing 场景可靠运行的前提。
二、测量学问题 A：端到端评测把多种技能耦合成一个分数。一道题往往同时考读图、OCR、常识、算术、格式遵循；总体 accuracy 上升无法说明哪种能力变好，下降也无法归因到感知。论文原话点明这是 end-to-end evaluation which combines various skills。
三、测量学问题 B：条目效度（item validity）不确定。观测到的错误可能来自视觉感知，也可能来自语言先验、背景知识、问题歧义、答案未定义。若不控制这些混淆，「感知失败」可能只是「题目失败」。这是本文最核心的立论：错误必须在条目层面可归因，否则基准分数不可解释。
四、应对策略：把人类可解性从「事后对照的聚合 baseline」改造成「条目级准入条件」。构造端保证人能解（G4）、问题清晰（G1）、答案唯一（G2）、视觉接地（G3）、挡掉纯文本捷径（G5）；筛选端保证模型不易解，避免天花板。误差定义随之收紧：只有人类验证过的视觉任务上、用被评测接口给出的错误答案才算 error。
五、更深一层：answer-only scoring 的边界。论文在结果部分指出，答案正确不能证明证据定位正确——模型可能靠语言先验命中标签（prior substitution），或用代理量冒充测量（pseudo-measurement）。即使条目效度被控制，评分口径本身仍会制造虚假的正确。
六、隐含假设与薄弱处：把「感知」操作化为「单图 + 短答案 + 离散可判定」，会漏掉连续的、开放式的、跨图的感知；G4 的「人类可解」在可见证据中没有给出标注人数、一致性与人类准确率数值，human-model gap 只能定性表述；G5 只挡纯文本可解题，挡不住训练集记忆；entry 难度由模型筛查决定时，基准会向被用作筛查器的模型族的盲区倾斜（推测，需回原文核对筛查协议）。
七、对 heuristic_draft 的校准：该草稿在 problem 层面基本失效，只有一句被截断的摘要，且把分类计数错置到 research_background、把伦理声明当作 limitations。以上分析由摘要、引言、结果与结论片段重建，标注「合理推断/推测」处需回原文核对。

Q2: 有哪些相关研究？

一、知识密集/推理型 VL 基准（论文引 [7,15,16,22,26,36,40,41,43]）。这类工作把 MLLM 推向 chart QA、大学水平学科题、多步推理，是「能力上限」叙事的主战场。本文不与之竞争，而是指出其分数不能推出基础视觉能力——两者测的是不同能力轴。具体基线名单需回参考文献表核对，PDF 片段只保留编号。
二、综合能力评测工具链。论文明确点名 MM-Vet 与 VLMEvalKit，并称这类 benchmark「evaluate the integrated ability in different application fields and model families」。价值在于可复现 harness、统一 prompt 与跨模型横向对比；局限在于聚合打分、类别诊断弱。
三、感知/计数/grounding 专项基准。计数（CountBench、TallyQA 一类）、幻觉（POPE 一类）、指代接地（RefCOCO 一类）在领域内已有长线积累（此为研究线索的合理推断，PDF 片段未保留这些名字）。HumanSightBench 与它们的差别不在题型新颖，而在准入流程：把「人一定能解、模型多半不能解」做成筛选条件，并对每条候选做五重检查。
四、语言先验与捷径研究。VQA 领域的 language prior 分析（不看图也能答题）是 G5 text-only shortcut filter 的直接思想来源。本文把它工程化为硬闸门，并在结果中指出过滤只能降低、不能消除先验贡献。
五、人类基线传统。以人类准确率作参照系在 VQA/视觉推理里历史悠久；本文的推进是把人类基线从「数据集级平均分」下沉到「条目级可解性判定」，允许逐题剔除歧义项，而不是事后解释 gap。
六、评测方法论相邻工作。LLM-as-a-judge（开放短答评分）、pass@k（多次采样取至少一次成功）、process/evidence supervision（要求模型给出证据而非只给答案）。本文同时使用前两者，并用 cited point vs. reference cell 触及第三者，但未把它做成主指标。
七、诊断式评测范式。本文属于「按视觉操作而非应用场景切分能力维度」的一支（7 L1 / 21 L2），目标是让失败可定位而非给出单一排行榜；同谱系工作通常强调 per-ability 报告与 failure taxonomy，本文的差异化在五道闸门这一条目效度协议。
八、与「人类简单、模型困难」对抗集传统的联系。此类工作通常靠人工挑刺构造，本文则把「人类可解」写成可执行的准入检查，使其从技巧变成流程。

Q3: 论文如何解决这个问题？

一、总体准则。设计哲学压成一句可检验的话：items human-easy by construction and model-hard by evaluation。两条腿缺一不可——只保证人类可解会撞天花板（模型全对，无信息量），只保证模型难解会引入歧义与不可解项（错误不可归因）。
二、流水线（据 Fig.1(b) 与引言片段重建）。Phase I 图像来源：从允许研究使用的来源或内部批准来源采集，排除隐私、不安全内容与再分发权不明的图像。Phase II Q-A 构造：每题 = 一张图 + 一个短答案问题 + 经核验参考答案，条目 schema 记为 (I, q, y)。Phase III 筛选：逐条过闸 + 人类验证 + 模型筛查（图中可见 Seed-Evolving、GPT-6-Astra 等模型参与 screening 的痕迹）。
三、五道准入闸门。G1 question clarity（问题清晰，不含歧义指代）；G2 answer uniqueness（答案唯一，避免多解）；G3 visual grounding（答案必须落在图中可定位的视觉证据上，不能靠外部知识）；G4 human solvability（人类可解，human-easy 的操作化定义）；G5 text-only shortcut filter（仅给问题文本、不给图时不可答，挡掉语言先验捷径）。任一不过即淘汰，Fig.1(b) 显示候选条目被逐条打叉筛掉。
四、分类体系。7 个一级类别 × 21 个二级子类，按视觉操作切分而非应用场景：可辨识的包括 OCR、Structured Visual Perception、Semantic Recognition、Fine-grained Discrimination、Space Understanding、Counting，第七类名称被行噪声截断（可能为 Grounding 或 Text Content，需核对）。数量上 OCR ≈1,318、Structured Visual Perception ≈722，两类合计约七成；其余五类分别约 201 / 181 / 172 / 164 / 118 题（这些数字与 2,876 总数相加自洽，但类别—数字的具体对应需回附录 A 核对）。
五、规模。2,876 道短答案视觉问题，每题单图，答案短到可用二值判定。
六、评测协议。8 个模型在同一题面、同一答案格式指令下评测；两种图像输入设置 Resize 与 No Resize；每个 model–question 对独立跑 3 次（同图同 prompt，保留前三条回复）；主指标 pass@3（at-least-once）；类别级报告 best-of-8 与单模型 accuracy。
七、判分机制。不采用精确字符串匹配，改用 DeepSeek-V3.2 thinking mode 作 answer judge，与人工核验参考答案比对后给二值标签；忽略解释与推理链；大小写、标点、空白、数字格式等语义等价表层差异视为正确。短答开放场景下这是必要妥协，但也把部分判定权交给了另一个模型。
八、附加测量：答案 + 证据。结果部分出现 recorded answer and evidence、cited point vs. reference cell 的对照，用于检验答案正确是否伴随正确证据定位，并据此区分 prior substitution 与 pseudo-measurement。
九、误差定义。只有「在被人类验证过的视觉任务上、用被评测接口作答」的错误才算 error——把接口、题面、人类可解性三者边界写进定义，是本文相对一般 benchmark 最讲究之处。
十、证据缺口。PDF 未提供 method 章节正文；人类标注人数、标注者间一致性、G5 的具体实现（用什么模型、什么阈值）、筛查阶段用了哪些模型与淘汰率，均需回原文确认。heuristic_draft 中 core_method 字段只有一句被截断的引用编号残片，不可用。

Q4: 论文做了哪些实验？

一、被评测模型（8 个）。闭源 5 个：GPT-5.6-Sol、GPT-6-Astra、Qwen-3.8-Max、GLM-5.3-Flash、Seed-Evolving；开源 3 个：GLM-4.6V、Kimi K2.6、Qwen3VL-235B-Thinking。覆盖不同厂商与开源/闭源两侧，但未交代选择依据（是否按公开榜单、是否可得性驱动）。
二、输入设置。Resize 与 No Resize 两种图像输入。作者明确声明：由于缩放可能影响细粒度视觉证据，两种设置下的结果应解读为「模型–输入配置」之间的比较，而不是受控的 resize 消融。这句避免读者把差异读成因果。
三、采样与指标。每个 model–question 对跑 3 次独立 trial，同图同 prompt，保留前三条回复；主指标 pass@3，定义为至少一次正确。类别级报告 best-of-8 models 的 accuracy，Counting 被单独拎出报告。
四、判分。DeepSeek-V3.2（thinking mode）作 judge，二值判定，忽略解释与推理链，接受大小写/标点/空白/数字格式等等价形式。可见证据中没有报告 judge 与人类评分的一致性。
五、主要对比维度：（1）总体 pass@3 排行；（2）7 个一级类别 accuracy 分布；（3）Counting 专项（所有模型上限被单独强调）；（4）答案正确性 vs 证据定位正确性（Fig.6 附近）；（5）定性失败模式归纳（prior substitution、pseudo-measurement）；（6）坐标约定与目标区域误读对归因的干扰。
六、构造侧的「实验」。基准构建本身也被当实验报告：候选池 → 五道闸门 → 通过 G1–G5 的条目 → 人类验证 → 模型筛查。Fig.1(b) 给出逐条打叉示意与若干计数（201、181、172、164、118 与 1,318/722 相加恰为 2,876，据此推断它们是类别计数而非淘汰计数——需回原文核对）。图注还出现 unsolved by construction yet human-solvable 表述，提示存在「构造上模型未解、但人类可解」的硬核子集。
七、缺失的实验：（1）没有训练实验——作者把 future training experiments 与 held-out evaluation 列为下游用途，本文只做评测；（2）没有人类准确率数值基线，G4 只作准入闸门，未报告标注者规模与一致性（如 Cohen's κ）；（3）没有 judge 鲁棒性实验（换 judge 模型、换 prompt）；（4）除 G5 的纯文本捷径过滤外，没有显式数据污染检查；（5）可见片段中没有完整的 per-model × per-category 表格，类别级只能用 best-of-8 数字做粗读。
八、复现性。数据集已发布在 HuggingFace 匿名链接（Susan0803/visual-perception-benchmark-anon），题目与参考答案可获取；但 judge 版本、prompt 模板、Resize 的目标分辨率等未在可见片段中给出，实操时需补齐。

Q5: 发现了什么实验现象？

一、总体上限不高，且被宽松指标掩盖。最强模型 pass@3 = 65.6%，意味着即便给三次机会，仍有约三分之一的「人类易题」全错。由于 pass@3 是 at-least-once 口径，单次准确率必然更低（合理推断，可见片段未给 pass@1）。因此 65.6% 应读作上界而非典型表现。
二、类别不均衡比总体分更刺眼。Counting 上所有 8 个模型都不超过 27.5%——最好模型也只有 27.5%。与之对照，总体榜头部能到 63.9%–65.6%。这是本文信息量最大的现象：总体分被大类（OCR 约占 46%，加上 Structured Visual Perception 约七成）主导，一旦按能力维度拆开，数数这类「三岁小孩都会」的操作成为系统性盲区。
三、模型排序不服从通用能力序。从 Fig.1(d) 相邻数字看，GLM-5.3-Flash 46.2 高于 GPT-5.6-Sol 的约 45；Seed-Evolving 58.4 高于 Kimi K2.6 42.2、Qwen3VL-235B 32.4、GLM-4.6V 24.0（数字与模型的对应受图注排版噪声影响，需核对 Table 1）。若成立，说明该基准测的是与知识/推理榜单不同的能力轴，排行榜不可外推。
四、开源与闭源之间落差明显但非单调：最低 24.0 与最高 65.6 相差逾 40 个百分点，远大于通用榜单常见差距——提示感知类能力对训练配方、视觉编码器与分辨率策略更敏感（推测）。
五、答案对 ≠ 证据对。Figure 6 展示 answer-only scoring 的边界：模型最终答案可能与参考答案一致，但引用的证据位置（cited point）与参考单元（reference cell）不匹配。这类样本在只看 accuracy 的报表里记为「正确」，从而高估真实感知能力。论文据此写下 answer correctness does not certify evidence localization。
六、两类伪正确被明确命名：（1）prior substitution（先验替换）——用语言先验中最高频的答案替换图中实际内容，示例题「What animal is in the image?」GT 为 panda，模型给出先验上更常见的动物；（2）pseudo-measurement（伪测量）——不做真实计数/测量，而给出看起来合理的代理量，计数题中表现为整数但系统性偏移。其价值在于它们是可命名的失败类型，而非笼统的「答错」。
七、视角与坐标约定会污染归因。方位类题（「从观察者视角，小狐狸朝哪个方向看」）要求视角转换并遵守坐标约定；结果部分提到，把错误归因于感知之前必须先排除坐标约定与目标区域理解的偏差。这是本文对「错误可归因」主张的自我设限。
八、分辨率/输入配置敏感。Resize 与 No Resize 的差异被作者明确声明不能当受控消融，但两种配置下结果不同说明细粒度证据的可得性依赖输入保真度，评测结论对预处理敏感（合理推断）。
九、稳定性线索。Fig.1(d) 中出现同一模型在某类别上三次运行同分（如 30/30/30）的记录，提示部分失败是稳定失败而非采样噪声；这对「能力缺口」的解释是正面证据，但也意味着 pass@3 对这类题几乎不提供额外召回（推测，需核对图注）。
十、过滤的极限。G5 只能挡掉纯文本即可答对的题；模型仍可能靠先验在需要看图的题上蒙对，残余的语言先验贡献无法从 accuracy 中分离——这是本文承认的口径边界，也是后续需要 process-level 证据才能解决的问题。

Q6: 有什么可以进一步探索的点？

一、训练侧闭环。作者把 future training experiments and held-out evaluation 写进摘要，最直接的下游用法是把 HumanSightBench 当 held-out 目标做感知专项 SFT/RL，再看是否迁移到通用 VLM 榜单；值得做的对照是「专项训练是否以通用能力为代价」。
二、从 answer-only 走向 evidence-level 监督。本文已展示答案对不等于证据对；自然的下一步是把 cited point / box / mask 变成可验证奖励（verifiable reward），做过程监督。挑战在于证据粒度的定义与多解证据的容错。
三、计数专项的机制路线。Counting 全线 ≤27.5% 是最明确的缺口。可探索：object-centric 表示、分割/检测工具调用、代码执行计数、any-resolution（原生分辨率）编码、把计数拆成「定位—去重—求和」的显式子步骤。评测上应区分小数量（≤10）与大数量、密集与稀疏、遮挡与非遮挡。
四、测试时工具使用。crop-zoom、多视图重采样、外部 OCR、计算器/代码执行，都是把「感知难」转成「工程可解」的路径；关键问题是工具调用是否引入新的错误传播，以及如何评测系统级而非模型级上限。
五、judge 可靠性研究。DeepSeek-V3.2 判分是单点依赖。可做：人–judge 一致性、跨 judge 模型（GPT/Claude/Qwen 系）的排序稳定性、数值等价规则边界（106 vs 106.0 vs 约 100 vs 一百零六），以及「部分正确」是否该给分。
六、条目效度协议的形式化。五道闸门是可复用的方法论资产；值得写成可执行 checklist 与统计报告（每道闸门淘汰率、淘汰原因分布、标注者间一致性 κ）。这对任何自建评测集都有直接价值，尤其 ai-for-science 里的图表与图注理解评测。
七、人类基线的量化。当前 G4 只做二值准入；下一步应报告人类准确率、反应时、标注者人数与分歧样本复核流程，使 human-model gap 可计算而非仅可叙述。
八、动态与防污染。静态 2,876 题会随时间被训练集吸收；可探索 seed-evolving 的在线出题、私有 held-out 分片、时间戳版本化，以及 G5 之外的「记忆型捷径」检测（如仅给题干+类别先验的可答性探测）。
九、能力原子化。7 L1 / 21 L2 仍偏粗；可细化为视觉操作原子（同色匹配、遮挡连续性推断、镜像判定、线条追踪、透视一致性），并与心理物理学经典范式对齐，使「人类易、模型难」获得理论解释而非仅经验观察。
十、模态与场景扩展。视频（时序计数、跨帧身份保持）、3D/具身（视角转换、空间关系）、文档与图表（结构感知）、专业图像（医学/遥感）都可沿用同一套闸门协议。
十一、与 agent 的接口。把感知子任务视为 agent 工具调用的环节，评测感知错误如何在多步任务中被放大或自我纠正，是本文与 agent 方向最自然的结合点。

Q7: 总结一下论文的主要内容

一、定位。HumanSightBench 不是新模型，而是一件测量工具：它把「人类可解」这件事从数据集级的平均基准下沉为条目级的准入条件，从而把一个长期被容忍的观察——模型在人类易题上翻车——变成可量化、可归因、可复现的评测结论。
二、动机链。作者先承认 MLLM 在知识密集、多步推理型视觉语言任务上的进展（chart 解读、大学水平题目），随即指出这些成绩不能证明基础视觉能力：数物体、沿线追踪、读嘈杂标签、辨别镜像图案仍是弱项。接着指出比较 MLLM 与人类感知为何困难——端到端评测把多种技能耦合成一个分数；单条题目本身是否有效也不确定，错误可能来自视觉感知、语言先验、背景知识、问题歧义或答案未定义。若这些混淆不受控，所谓「感知失败」可能只是「题目失败」。于是全文立论落到一句话：错误必须在条目层面可归因。
三、构造方法。其准则为 human-easy by construction, model-hard by evaluation。流水线分三段：图像来源（许可与合规、去隐私）→ Q-A 构造（单图 + 短答 + 核验参考答案，条目记为 (I,q,y)）→ 筛选（人类验证 + 模型筛查）。筛选由五道闸门实现：G1 问题清晰、G2 答案唯一、G3 视觉接地（答案必须落在图中可定位证据上）、G4 人类可解、G5 纯文本捷径过滤（不给图就该答不出）。任一不过即淘汰。产物为 2,876 道短答案题，覆盖 7 个一级感知类别与 21 个二级子类；类别按视觉操作而非应用场景组织，可辨识的包括 OCR、Structured Visual Perception、Semantic Recognition、Fine-grained Discrimination、Space Understanding、Counting。数量上 OCR 约 1,318 题、Structured Visual Perception 约 722 题，两类合计约七成，其余五类各约 118–201 题——这意味着总体分由大类主导，类别级诊断必须单独看。
四、评测协议。八个模型（闭源 GPT-5.6-Sol、GPT-6-Astra、Qwen-3.8-Max、GLM-5.3-Flash、Seed-Evolving；开源 GLM-4.6V、Kimi K2.6、Qwen3VL-235B-Thinking）在统一题面与答案格式指令下评测；两种图像输入（Resize / No Resize），作者明确声明这不是受控 resize 消融而是模型–输入配置比较；每题三次独立采样，主指标 pass@3（至少一次正确）；判分由 DeepSeek-V3.2 thinking mode 作 judge 给二值标签，忽略解释与推理链，接受大小写、标点、空白、数字格式等等价形式。此外还有一条辅助测量：把答案与引用证据（cited point）一起记录，与参考单元（reference cell）比对，用来检验答案正确是否伴随正确证据定位。
五、主要发现。（1）总体上限：最好 pass@3 仅 65.6%，即最强模型在三次尝试后仍有约三分之一人类易题全错；由于是 at-least-once 口径，单次表现应更低。（2）类别失衡：Counting 上无模型超过 27.5%，与总体榜 60%+ 形成强烈反差，说明总体分掩盖了真实的感知短板。（3）排序不服从通用能力序：GLM-5.3-Flash 46.2 高于 GPT-5.6-Sol 的约 45，开源与闭源跨度从 24.0 到 65.6，提示感知能力轴与知识/推理轴不同。（4）答案对不等于证据对：模型可给出正确标签却指向错误区域，使「正确」无法归因于感知；论文由此提出两类可命名的伪正确——prior substitution（用语言先验替换图中内容）与 pseudo-measurement（用合理代理量冒充测量）。（5）归因边界：方位/视角类题要求坐标约定与视角转换，在把错误归因于感知前必须先排除接口与坐标理解问题。（6）对输入配置敏感：Resize 与 No Resize 的差异不能当因果消融，但表明细粒度证据可得性依赖保真度。
六、结论与边界。作者在结论中重申立场：HumanSightBench 把人类可解性当作条目级准入条件而非聚合基准；七类、五闸门的结构用于降低歧义与外部知识混淆；pass@3 与类别级结果显示瓶颈持续且不均衡；仅凭答案准确率既不能证明证据定位，也不能隔离错误原因。伦理与使用声明明确该基准不得作为安全、公平或真实部署就绪性的独立指标；论文披露使用 AI 写作助手参与起草与编辑。
七、方法论评价与遗留缺口。优点是把「条目效度」工程化，且以一套可复用的闸门协议解决了长期困扰感知类基准的混淆问题；对 pass@3 与 Resize/No Resize 的自我限定也显得克制。缺口在于：judge 依赖单一模型且未报告人–judge 一致性；没有人类准确率数值基线，human-model gap 只能定性；类别极不均衡使小类分数置信区间偏宽；模型筛查若以特定模型族为筛选器，会向该族盲区偏置；短答单图的离散化把感知窄化，未覆盖连续、开放式、多图与视频感知；静态基准存在污染风险，G5 只挡纯文本捷径。
八、可用性。题目与参考答案已在 HuggingFace 匿名链接发布，可作为 held-out 感知评测集；五道闸门协议可直接迁移到自建评测集（尤其图表/图注理解类 ai-for-science 评测），而 answer + evidence 的双通道记录方式则是把 benchmark 变成诊断工具的起点。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：方法论可直接迁移：五道准入闸门是一套可执行的「条目效度」协议，任何自建评测集（尤其 ai-for-science 中的图表/图注理解、实验图判读）都能照搬，用来剔除歧义项、多解项与语言先验可解项。

## 基本信息

- 作者：Zixin Su, Yangguang Lin, Zegang Peng, Yuntong Sun, Xuanyi Wang, Jiahao Wang, Qiankai Xu, Bing Yuan, Jinze Bai, Youbin Wu, Jingjia Huang, Ruyang Liu, Yanxu Hu
- 机构：ByteDance（字节跳动）与 Tsinghua University（清华大学）——依据通讯作者邮箱 suzixin.0803@bytedance.com 与 suzx26@tsinghua.edu.cn 推断；论文元数据未提供机构列表，需以原文作者单位为准。
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI, cs.CL
- 日期：2026-11-09
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.12305v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先采用了 PDF 语义检索命中的摘要、引言、结果与结论片段，并按 field_evidence_map 的锚点分配证据（results 证据只用于实验与发现类字段）；heuristic_draft 多处截断且字段错位（把分类计数塞进 research_background、把伦理与 AI 使用声明当作 limitations、main_contributions 只剩引用编号残片），仅作线索参考，冲突处一律以检索证据为准。
