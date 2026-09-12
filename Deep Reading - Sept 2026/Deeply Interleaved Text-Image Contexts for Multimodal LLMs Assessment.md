---
user_id: "cheng tan"
paper_id: 10416
arxiv_id: "2609.02573v1"
title: "Deeply Interleaved Text-Image Contexts for Multimodal LLMs Assessment"
publish_date: "2026-09-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.02573v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.02573v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-05T01:32:19"
---
# Deeply Interleaved Text-Image Contexts for Multimodal LLMs Assessment

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multimodal large language models · interleaved text-image reasoning · benchmark · cross-modal association

## 一句话总结

针对现有多模态大模型评测主要聚焦多图任务、忽略文本与图像深度交织场景的问题，论文提出 TIC-Bench 基准，围绕逻辑、时间与空间三类关联关系细分为8个任务类型、共2280个问题，系统性评估10个主流 MLLM，发现模型与人类专家之间仍存在显著差距，且在跨图文分布式证据整合上存在持续困难。

## 摘要

> Current evaluations and training of multimodal models predominantly focus on multi-image tasks, largely overlooking interleaved text-image scenarios. In such multi-image tasks, text typically serves merely as task instructions, lacking deep semantic interaction with the visual content. In contrast, realworld applications like text-image co-creation, character tracking, and spatial reconstruction require constant interaction between text and images. Consequently, models must possess a deep understanding of these interleaved contexts. To bridge this gap, we introduce a novel benchmark, TIC-Bench (deeply interleaved Text-Image Contexts), designed to evaluate the capability of models to integrate text-image clues and recover the ground truth facts within deeply interleaved contexts. This benchmark encompasses three core domains: Logical, Temporal, and Spatial Association, which are further categorized into eight specific types, comprising a total of 2,280 questions. We evaluated 10 state-of-the-art MLLMs and observed a substantial performance gap compared to human experts, together with persistent difficulties in integrating evidence distributed across interleaved visual and textual inputs. Ultimately, this benchmark provides a valuable analytical tool for assessing and advancing the ability of multimodal models to effectively integrate text and image information in deeply interleaved contexts. TIC-Bench is publicly available at https://huggingface.co/datasets/pino10010/TIC-Bench

Q1: 这篇论文试图解决什么问题？

1. 核心问题定位
 - 现有视觉语言评测主流是「单图+文本」或「多图+文本」的多图任务。这类任务的一个隐含设计是：文本几乎只担任指令（task instruction），图像则是彼此独立的证据单元。模型在回答问题时并不需要让文本与图像之间形成深层、双向、随时更新的语义绑定，因此无法衡量真实场景所需的跨模态持续推理。
 - 论文把这种缺失的评测维度定义为 deeply interleaved text-image contexts，即文本和图像在同一个上下文中交替出现，每一处视觉片段或文本片段都可能携带后续问题所需的线索，且线索是碎片化、分布式的。

2. 现实世界中的需求缺口
 - 多模态文档理解：文档排版天然包含文字+图表交替的布局，需要模型在段落与插图之间来回切换、跟踪同一实体在不同媒介中的描述。
 - 文本-图像协同创作：文案与配图互为依据，创作过程需要反复检查图文一致性并提供修改，属于典型的「写一点看一张图再写一点」的交错流程。
 - 多视角事件追踪：同一个事件的多张照片与多段文字报道按时间/地点交替出现，模型需要将各条线索对应到事件中同一个人、同一个时间点或同一个空间关系，再做因果或时序推理。
 - 空间重建与空间推理：多个视角或按顺序展示的空间信息与文字描述交织，模型需要建立并维护跨视角的对象对应关系。

3. 为什么现有评测范式无法覆盖上述需求
 - 在多图 benchmark 中，问题通常是先给若干条件图再给一个统一指令，图像之间虽然共享主题，但文本没有与其深度交互；模型只需依赖视觉相似性或独立的视觉 caption，即可完成大部分题目。
 - 这类设计无法回答模型是否具备持续「绑定、整合、传播」（bind, integrate, and propagate）分散线索的能力，也无法暴露模型在处理长交错图文序列时的记忆丢失、注意力漂移和模态间错误关联等问题。

4. 论文要解决的方法论问题
 - 缺少一个可控、可诊断、可复现的评测工具，用来度量模型在多轮图文交替场景下执行跨模态关联推理的能力，而 TIC-Bench 的目标就是系统化填补这个缺口。

5. 附带问题
 - 除了能力度量，该基准还希望回答：现有 SOTA MLLM 中是否存在能够胜任深度交错推理的模型；不同模型在任务子类型上的强项分布如何；引入推理性思考模式（thinking mode）是否足以弥补长交错上下文带来的困难。

Q2: 有哪些相关研究？

论文在引言中主要回顾了以下几条研究线，但由于本 PDF 片段缺少完整 Related Work 章节，下面仅能按作者引用与上下文作归纳性说明，细化的文献评述请以原文为准。

1. 多模态大模型的发展
 - 论文列举了近期 MLLM 的代表性工作（Bai et al. 2023；Yang et al. 2025；Team et al. 2026a；Hong et al. 2025；Team et al. 2025, 2026b 等），表明通用图文理解与推理取得了显著进步。
 - 这类模型的共同能力基线是单轮或多轮的图文对话，通常一次输入一张或一组图片并返回文本答案。

2. 单图文对与多图视觉语言评测
 - 传统单图文对任务（Li et al. 2023 所代表的指令微调/对齐主线）主要考察模型能否建立「一张图+一段文本」的语义绑定。
 - 多图任务（如 Suhr et al. 2019；Huang et al. 2016 所代表的视觉推理/多图推理），此类任务让模型同时看多张图并回答一个统一问题；但文本往往只发生在问题端。
 - 论文指出，这些数据集大多没有将图片与文本在输入序列上深度交替，因而无法检验长时间跨媒体证据链构建能力。

3. 交错无关但与本文密切相关的应用方向
 - 多模态文档理解（Yan et al. 2025；Hu et al. 2024），强调版面图文交错，是现实世界常见需求。
 - 文本-图像协同创作（Cui et al. 2025；Wang et al. 2026a；Deng et al. 2025；Wang et al. 2026b；Li et al. 2026a,b；Wang et al. 2025），需要模型在多轮图文交替中保持一致性。
 - 多视角事件追踪与跨视角推理（Tang et al. 2019；Feng, Ablavsky, and Sclaroff 2021；Zhou et al. 2023），通常包含多张图像与多段文本描述。

4. 现有评测范式与本工作的空白
 - 论文认为，尽管多图评测和文档评测已经存在，但没有一个基准显式地把「深度交织的文本-图像序列」这一输入格式作为中心变量，也没有围绕逻辑、时间、空间这三类跨模态关联来组织任务。
 - TIC-Bench 可以看作对现有视觉问答、多图推理、文档理解和长上下文多模态评测的交叉补充，强调的评测点是证据片段在图文两种模态之间的运动、匹配与整合。

5. 信息缺口提示
 - PDF 检索证据只覆盖摘要、引言和结论，未包含系统的 related work 小节，因此上述归纳中能够直接归属到论文的只有引用脉络和问题表述；具体的基线数据集对比、评测指标、任务难度标定等细节需要回原文确认。

Q3: 论文如何解决这个问题？

1. 整体策略：构建一个以「输入格式」为核心变量的评测基准
 - TIC-Bench 不是简单把文本和图像混在同一 prompt 中，而是要求每一道问题都处在一个需要持续跨模态指称和证据恢复的结构里。模型需要不断把当前看到的图像/文本片段与已看到的片段进行关联，并从这些交织的上下文中找回问题所指向的 ground truth facts。
 - 任务表述强调「deeply interleaved」这一关键条件：文本片段穿插在图像之间，或图像夹在文字段落之间，没有明显的边界把「指令」和「视觉证据」分开。

2. 评测维度设计：三大领域、八个子类型
 - Logical Association（逻辑关联）：需要基于图文线索进行演绎、比对、反事实或常识逻辑推断。
 - Temporal Association（时间关联）：关注事件顺序、时间指代、跨图文的变化检测或按时间排列的发展轨迹。
 - Spatial Association（空间关联）：要求在多图与文本描述中建立对象定位、相对位置、空间变换或跨视角一致性推理。
 - 三大领域进一步被细分为8个具体任务类型，每个类型针对一种不同的推理结构（differences in reasoning structures）。具体任务名称、样例格式与难度控制在摘要和引言片段中未展开，需查阅论文正文与图1/数据卡。

3. 任务样例的输入格式：交错序列
 - 每道题由多段文本片段与多张图片按照任务所需逻辑交叉排列，形成一份交错上下文。
 - 问题置于交错序列之后或穿插在序列中，正确答案以可判定的 ground truth 形式给出，从而支持自动评测。设计思想是模拟现实中的文档/创作/日志场景，而不是把全部图片堆在前面、把文字堆在后面。

4. 数据规模与公开性
 - 总计 2280 个问题，每个问题都有明确的任务类型标签与领域归属。
 - 数据已公开，托管于 Hugging Face，便于研究者直接使用和复现评估。

5. 评测对象和对比协议
 - 选择10个 state-of-the-art MLLM 作为被测系统，并使用人类专家分数作为能力上限参照。
 - 评测中纳入普通模式与 thinking mode 的对比，检验「更长的链式思考/内部推理」能否帮助模型跨越交织上下文整合证据。
 - 评测指标、推理预算控制、prompt 模板等细节本文 PDF 片段未能提供，属于信息缺口。

6. 论文方法部分的开放性问题
 - 从证据片段无法看出 TIC-Bench 的具体构建流程，例如是人工编写、真实网页/文档采集，还是基于半自动生成再校验。合理推断是作者先定义3域×8类的任务模板，再据此生成或收集图文序列并标注问题与答案，但这一流程属于推断，需要核对论文方法章节。
 - 同样需要核对的还有：是否采用多选、短答案生成或开放式回答等答题格式；是否对每一道题都做了人类回答的稳定性检验；以及模型回答的自动评估方式是字符串匹配、LLM-as-judge 还是人工评分。

Q4: 论文做了哪些实验？

论文在摘要与结论中给出了评估设计的总体信息，但实验小节正文未包含在当前 PDF 检索证据中，因此以下实验描述只能覆盖已明确声明的问题，并标明推断成分。

1. 主评测：10个 SOTA MLLM
 - 评测对象包括10个当前代表性的多模态大模型，具体模型名称、版本和参数量在已获取片段中未列出，因此无法在此提供精确清单。合理推断这些模型覆盖了不同视觉编码器与语言主干组合，且可能包含开源与闭源模型的混合。
 - 每个模型在相同的交错上下文提示下回答问题，根据与 ground truth 的一致性计算准确率或得分。

2. 人类专家对照
 - 报告指出存在 substantial gap between current models and human experts，因此实验中必然包含人类专家在 TIC-Bench 上的表现作为参考基线。
 - 人类专家的参与人数、人员专业背景、作答题目数量与筛选条件未知，需要原文说明。

3. thinking mode 对照
 - 结论明确提到“thinking mode improves performance in some cases”，说明实验包含同一模型在普通推理与长篇思考推理两种设置下的对比。
 - 具体哪些规模/架构的模型受益、哪些场景无效，以及 thinking 是否伴随更长推理预算，当前并不清楚。

4. 按领域与子任务维度的分析
 - 论文声称 TIC-Bench 覆盖逻辑、时间、空间三大领域并细分为8个子任务，因此合理推断实验中至少会按领域、子任务类型分别报告性能，还可能包含错误类型分析。
 - 由于结果表格与图表未在检索证据中出现，无法获取每个领域的原始准确率、不同模型之间的排序、以及各子任务难度排序。

5. 信息缺口总结
 - 缺失的关键实验信息包括：模型名单、各模型总体得分、人类专家得分、显著性检验、每类任务的例数与平均字长、交错序列长度分布、答案格式评估方式。建议读者直接查阅原论文的 Experiments / Results 部分的表1-表4及以上。

Q5: 发现了什么实验现象？

从摘要、引言片段和结论中能够确证的实验现象主要有以下几个方面，具体数值与图表无法从当前证据获得，需以论文正文为准。

1. MLLM 与人类专家之间存在显著性能差距
 - 论文明确表示，在 TIC-Bench 上几乎所有被测模型与人类专家之间都有较大差距（substantial gap）。这表明深度交错图文推理远未饱和，且人类在该类任务上仍保有稳健优势。
 - 合理推断该现象在时间或空间关联任务上尤为明显，因为这两类任务要求模型在长上下文中维持并更新状态；不过论文文本并未给出分领域差距的比较，此处只是结合一般多模态推理规律的推断。

2. 不同模型表现出互补优势
 - 结论指出“different models exhibit complementary strengths across tasks”。这意味着不存在在所有8个子任务上都全面领先的单一模型，而是不同模型分别擅长逻辑、时间或空间中的某些类型。
 - 这类现象提示：现有模型的优势更多来自视觉表征或语言先验的局部强化，而不是建立了一套通用的跨模态证据链推理机制。这是对结果的解读性推断。

3. thinking mode 只在部分情况下带来改进
 - “thinking mode improves performance in some cases”说明增加内部推算并不总能弥合交错上下文理解缺口。
 - 推测原因包括：长思考并不能解决底层注意力对远端图文片段的遗忘或模态间定位失败；甚至可能带来更多幻觉式联想。该解释属于推测，原文尚未给出来源与机制分析。

4. 跨多图与多文本片段的证据连接仍不可靠
 - 论文反复强调模型“struggle to reliably connect evidence distributed across multiple images and text segments”，这是 TIC-Bench 评测中最重要的负面现象。
 - 即使在单一图文对或普通多图任务中表现良好的模型，进入深度交替序列后也可能出现指代丢失、把某一图片的信息错误归因到另一图片、受无关文本干扰等问题。这些问题目前缺乏精确的错误统计支撑，但在结论中被明确描述为共同瓶颈。

5. 缺失的观测维度
 - 当前证据没有提供模型在三种任务域上的准确率趋势、任务长度与性能下降曲线、不同交错位置（图文交替密度）对各模型的影响，也没有针对失败样本的人类归因标注。若论文实验部分包含这类可解释性分析，它们将是值得深挖的负结果来源。

Q6: 有什么可以进一步探索的点？

论文在结论中直接给出了面向未来模型的能力要求，也可以基于 TIC-Bench 的出发点展开更多可探索方向。

1. 模型侧：长交错多模态上下文管理
 - 论文明确要求未来模型具备更强的 long interleaved multimodal context 管理能力。具体可尝试：扩展视觉 token 压缩与记忆机制、引入显式的跨模态缓存/工作记忆、改进位置编码以区分字节/图像块/语义片段。
 - 与长上下文架构（如稀疏注意力、状态空间模型、递归记忆）结合是自然方向，因为 TIC-Bench 的困难点在「保留相关信息、抑制无关信息、建立一致关联」。

2. 训练侧：用深度交织数据做对齐与微调
 - TIC-Bench 目前定位是评测分析工具，但其 2280 道结构明确的题目可以改造成训练语料，帮助模型学习跨图文证据链的偏好。合理推断作者可能认为评测与训练应双向打通，但论文并未明确表示提供训练划分，该方向属于对 benchmark 资源的合理延伸。

3. 任务侧：覆盖更开放的结构与更长的情景
 - 现有设计覆盖逻辑、时间、空间三类，未来可以扩展至因果、情感、多智能体对话等富含异质线索的场景；同时可以增加文本与图像交替频率与序列长度的控制变量，使基准支持能力曲线测量。
 - 可将图1的任务分布图扩展到「难度-长度-交错率」三维控制。

4. 评测侧：细粒度失败归因
 - 当前结论停留在整体差距与跨域互补。未来可以输出带错误类型标签的失败分析：哪些错误来自视觉编码噪声、哪些来自长文遗忘、哪些来自跨图混淆。这类分析能直接把评测结果转成模型架构改进信号。
 - 还可以把错误归因与思维链轨迹结合，检测模型在哪个中间步骤丢失线索，该方向需要新增人工标注或流程型评测，原文目前未涉及。

5. 应用侧：与 agent、文档理解、科学数据叙述结合
 - 对用户关注的 agent 方向，长图文交错上下文管理是多模态 agent 在工具使用、环境观察、多轮记录中的核心挑战；TIC-Bench 三类关联可以直接迁移为 agent 轨迹评测脚本。
 - 对 AI for Science 方向，时间与空间关联任务与实验记录、显微镜图像序列、时序监控等多模态科学数据有天然相似性，基准任务模板可复用于领域数据上的能力检验。
 - 论文本身未讨论这些扩展方向，以上为基于用户报告偏好和领域格局的合理推断。

Q7: 总结一下论文的主要内容

TIC-Bench 论文围绕一个此前被 MLLM 评测忽略的格式维度展开：深度交织的文本-图像上下文。论文首先指出现有评测习惯的两级错位：一是主流 multi-image 任务把多张图和一段指令放在一起，文本与图像缺乏持续的互指和更新；二是只在单图文对上进行能力判断的模型评估，无法覆盖文档理解、图文协同创作与多视角事件追踪等真实应用。这些应用要求模型在输入序列里反复执行跨模态的绑定、追溯、更新与传播，而不是「看完图再读题」式的独立感知。
为把这种能力显式化，论文提出 TIC-Bench：一个专门评估深度交织上下文理解的新基准。TIC-Bench 将任务划分为逻辑关联（Logical Association）、时间关联（Temporal Association）与空间关联（Spatial Association）三大域，并再细分为8个具有不同推理结构的任务类型，总计2280个问题。每个问题的组成特点是：多段文本与多张图片以交错方式排列，文本和图像共同贡献答案线索；模型需要从分散的证据中恢复 ground truth facts，而不是直接基于单张图作出判断。论文认为，这类设计能够测量模型在长交错序列中保持跨模态对应关系（cross-modal correspondences）和整合分布式证据的能力。
在评测协议上，论文选择了10个 state-of-the-art MLLM，并把人类专家表现纳入对照；部分实验还引入了 thinking mode 与默认模式的对比，以观察更深推理是否能补偿交错输入带来的额外负担。论文报告的总体结论是：当前最先进的 MLLM 与人类专家之间的性能差距依然显著。不同模型在不同任务上展现出互补优势，说明尚未出现统一的跨模态关联推理能力；thinking mode 只在部分任务中带来收益，不能从根本上解决长交错序列下的信息丢失与误关联。最突出的失败模式表现为：模型难以可靠地把分散在多个图像与多个文本片段中的证据连成一条完整推理链，容易遗漏远端线索或被不相关片段干扰。
论文由此提出未来模型需要具备四个能力：管理更长的交错多模态上下文；在上下文中保留与任务相关的信息；抑制无关干扰；建立并保持一致的跨模态关联。TIC-Bench 因而被定位为一种分析工具，既可诊断当前模型在哪些关联类型上存在桎梏，也可用于后续训练方法的迭代验证。基准已在 Hugging Face 上公开。
从论文信息完整性来看，目前证据只覆盖摘要、引言部分与结论，无法详细复述每个子任务的数据来源、构造方式、题目模板、评分协议及具体实验表格。若需要完整把握论文的贡献，还应结合原论文的方法、数据统计、评测 Setups 和附录中的样例进行二次精读。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对于注意系统性工作的用户：TIC-Bench 提供了一种以输入格式为核心变量的 benchmark 组织范式，其「任务域-子类型-题目量-人类基线」的分层设计可迁移到其他多模态评测方向。

## 基本信息

- 作者：Zihao Wang, Xi Xiang, Yuwen Sun, Yingyu Li, Yabo Zhang, Yihan Zeng, Fan Li, Wangmeng Zuo
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-09-02
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.02573v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Abstract、Introduction 和 Conclusion 证据片段；由于未获取全文方法与实验细节，部分描述基于现有证据的合理推断或推测，并已在对应字段中标注信息缺口。
