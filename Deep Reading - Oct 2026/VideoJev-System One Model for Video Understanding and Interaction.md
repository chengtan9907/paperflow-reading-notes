---
user_id: "cheng tan"
paper_id: 16054
arxiv_id: "2610.18615v1"
title: "VideoJev: System One Model for Video Understanding and Interaction"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.18615v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.18615v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-18T02:04:24"
---
# VideoJev: System One Model for Video Understanding and Interaction

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：system one paradigm · temporal grounding · multiple-choice video question answering · video multimodal large language model

## 一句话总结

VideoJev 将视频理解与交互重构为结构化决策任务：在预训练多模态大语言模型（MLLM）之上，用共享打分头对时间区间和文本选项做非自回归评分，统一时间定位与多项选择视频问答，并通过 Cached 与 Joint 两种推理模式复用视觉计算，在三个时间定位基准和四个视频问答基准上分别获得 6.7/10.8 点 mIoU 提升与超过 100 倍的推理加速。

## 摘要

> Visual agents must turn video observations into timely decisions to track progress and choose what to do next. Building on the emerging System One paradigm, we introduce VideoJev, a family of models that cast video understanding and interaction as structured decision tasks. We adapt pretrained multimodal large language models (MLLMs) by replacing autoregressive answer decoding with a shared head that scores temporal intervals and textual options against video evidence. These scores yield event spans or answer choices, unifying temporal grounding and multiple-choice video question answering. This combines the semantic flexibility of MLLMs with efficient decisions that agents can use directly. As an interaction unfolds, new goals and intermediate results prompt further queries about the same visual history. VideoJev therefore reuses visual computation through Cached inference for queries as they arrive and Joint inference for multiple queries in a single forward pass. Across three temporal grounding benchmarks, VideoJev-4B and VideoJev-35B-A3B improve mean mIoU over their backbones by 6.7 and 10.8 points, respectively, with measured inference speedups of $2.8$-$3.1\times$. On four video QA benchmarks, both achieve performance comparable to their backbones in Thinking mode, with measured inference speedups exceeding $100\times$. These results offer a unified, reusable decision interface for composing structured visual judgments into complex, open-ended workflows.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：视频理解系统如何在保持语义灵活性的同时，为智能体提供及时、直接可用、可高效复用的结构化决策输出。具体问题拆解如下。第一，现有视频 MLLM 用自回归生成表达决策：时间定位需要生成时间戳 token，视频问答需要生成答案文本，即使最终输出是一个紧凑的结构化决策（一个时间区间、一个选项），也必须经过逐 token 的自回归解码，带来延迟高、与智能体程序接口不直接的问题。第二，判别式视觉模型虽然高效，但通常依赖任务特定设计或固定标签空间，难以覆盖开放语义。第三，交互场景中，同一段视觉历史会被反复查询（如烹饪助理检查配料是否添加后再决定下一步），现有模型每次查询都需要重新编码视觉输入，推理开销随查询次数线性增长，缺少跨查询的视觉计算复用机制。第四，时间定位与多项选择问答在现有文献中通常作为两个独立任务建模，缺少一个统一接口来输出事件区间或答案选项，难以组合成更高层的智能体工作流。第五，System One 范式的核心主张——用类型化概率判断代替开放文本生成——需要验证其是否能在视频理解场景中落地，并需要回答效率与质量是否可以兼得：即结构化决策头带来的加速是否以语义能力下降为代价。

Q2: 有哪些相关研究？

相关研究可以从四条线索梳理。一是视频多模态大语言模型（Video MLLM）：Qwen3-VL、Qwen3.5、VideoChat3、Video-ORA、TimeLens2、Molmo2 等模型提供了强大的视频语义理解与语言接口，但决策通常通过自回归生成表达；论文把它们作为 VideoJev 的骨干模型和对比基线。二是时间定位（temporal grounding）方法：传统方法大多依赖专用模型或固定标签空间，生成式时间戳输出格式不稳定，VideoJev 用候选区间评分取代时间戳 token 生成。三是 System One 范式：Jev 提出用类型化概率判断（typed probabilistic judgments）代替逐 token 答案生成，软件可以直接消费分数而不需要解析生成文本；VideoJev 是该范式在视频领域的具体化，把时间区间和文本选项统一为两类可评分假设。四是视频问答与长视频理解基准：LVBench、Video-MME、LongVideoBench、MLVU 等用于多项选择 QA 评估；CharadesTL、ActivityNetTL、QVHighlightsTL 用于时间定位评估。另外还涉及高效推理与 KV cache 复用方向：Cached inference 复用同一视频的视觉计算，Joint inference 在单次前向中处理多个查询，与多查询交互场景下的计算复用研究（如多轮对话缓存）相关，但论文强调其针对同一视觉历史的结构化决策场景做了专门设计。需要注意，heuristic_draft 中的相关内容并不可靠，正文引用只能确认部分模型与基准名称。

Q3: 论文如何解决这个问题？

VideoJev 的方法可以概括为“保留 MLLM 语义编码、替换自回归解码为共享结构化打分头”。具体机制如下。第一，任务重构：把时间定位和多项选择视频问答都建模为对候选假设（时间区间、文本选项）的评分任务。第二，共享打分头：一个共享的预测头同时输出时间区间分数和文本选项分数，即对给定的候选时间区间和候选文本选项分别计算与视频证据的匹配分数，取最高分对应的区间或选项作为决策结果；这种设计让时间定位与 QA 共享同一接口。第三，适配训练：在预训练 MLLM 之上进行训练，训练数据方面，时间成员关系（temporal membership）使用 TimeLens-100K 和 TimeLens2-93K，把标注区间转换为候选级别标签；选项选择使用内部整理的语料，二分类偏差从零初始化开始训练。第四，Cached inference：同一视频的多轮查询共享视觉编码与 KV 缓存，后续查询只计算新增的查询部分，避免重复视觉编码。第五，Joint inference：多个查询在单次前向传播中并行打分，进一步摊销视觉计算。第六，推理模式适配：自回归 QA 在 Direct 模式下支持联合处理——直接预测一个选项 token 而不做推理；多 token 输出（如时间区间、推理链）需要顺序 token 反馈，因此使用 Cached inference。这里需要指出一个证据缺口：heuristic_draft 和检索片段显示论文比较了联合训练单任务与多任务、消融了训练数据影响并评估了置信度，但方法章节的完整细节（打分头结构、损失函数、候选区间如何生成）在提供的片段中没有完整覆盖，需要回原文确认。

Q4: 论文做了哪些实验？

论文的实验围绕预测质量和推理效率展开，涵盖时间定位与多项选择视频问答两大类任务。时间定位基准为 CharadesTL、ActivityNetTL、QVHighlightsTL，QA 基准为 LVBench、Video-MME、LongVideoBench、MLVU。模型规模两档：VideoJev-4B（4B 语言骨干）与 VideoJev-35B-A3B（30–35B 总参数、3B 激活的 MoE）。对比基线包括专有模型 GPT-5、Gemini-2.5-Pro，以及开源稠密模型 Molmo2-4B、Qwen3-VL-4B、Mage-VL、Qwen3.5-4B、VideoChat3-4B、Video-ORA-4B、TimeLens2-4B，开源 MoE 模型 Nemotron-3-Nano-Omni-30B-A3B、Qwen3-VL-30B-A3B、Qwen3.5-35B-A3B、Qwen3.6-35B-A3B。生成式基线默认使用 Instruct 模式，VideoJev 默认使用 Joint 推理，指标用 R1@.3、R1@.5、R1@.7 和 mIoU，平均秒数（seconds）也被列出以反映推理延迟。消融研究控制训练数据、比较单任务与多任务训练、对比直接选项与推理基线，并评估置信度。推理效率实验测量同一视频上 1–10 个查询的延迟，比较 Cached 与 Joint 模式，同时也报告基线模型的 Instruct、CoT、Thinking、Direct 模式延迟。附录 A 说明评估设置，附录 A.2 说明质量设置与得分来源，附录 A.4 说明推理模式。需要注意，部分表注显示网络格式被检索切碎，具体设置的完整细节需回原文核对。

Q5: 发现了什么实验现象？

实验现象和数据如下。第一，时间定位提升显著：VideoJev-4B 在 CharadesTL、ActivityNetTL、QVHighlightsTL 上分别达到 60.1、62.8、73.2 平均 mIoU，三项平均 65.3；VideoJev-35B-A3B 分别达到 63.7、65.5、74.0，平均 67.7。相比骨干模型，VideoJev-4B 平均 mIoU 提升 6.7 点，VideoJev-35B-A3B 提升 10.8 点（摘要数据）。第二，推理延迟大幅下降：VideoJev-4B 平均秒数仅 0.276，远低于所有开源基线（TimeLens2-4B 为 0.646、Qwen3.5-4B 为 0.763、Video-ORA-4B 为 2.347、Mage-VL 为 1.437）；VideoJev-35B-A3B 为 0.336，远低于 MoE 基线的 1.021–3.882 秒。第三，QA 性能相当但推理加速超过 100 倍：在两个 QA 基准上测量到对 Thinking 基线的加速为 129× 和 100×，同时 VideoJev 在 Video-MME 和 MLVU 上有一定优势。第四，Joint 与 Cached 的权衡：Qwen3.5-4B Direct 模式在 Joint 处理下性能下降，准确率从 Cached 推理的 60.0% 降到 Joint 的 58.7%，低于 VideoJev 在可比延迟（1.67 秒对 1.64 秒）下的 67.9%；多 token 生成仍然昂贵，Qwen3.5-4B Instruct 的延迟远高于 VideoJev。第五，多任务联合训练存在互利：联合训练（单任务相比）在全部七个基准上都有提升，平均 grounding mIoU 提高 0.7 点，平均 QA 分数提高 1.3 点，最大 QA 增益在 LongVideoBench（+1.9 点），说明时间监督可能强化证据定位，而 QA 强化语义判别。第六，效率随查询数量变化的规律：图 3 比较了 Cached 与 Joint 推理随查询数增长的计算效率，Joint 对多查询更优，而多 token 输出（时间区间、推理链）需要顺序 token 反馈所以只能用 Cached。反直觉之处在于：牺牲自回归生成后，QA 精度没有损失反而在部分基准上有优势，说明结构化决策头没有削弱语义理解；另一个张量是 Direct 自回归基线在 Joint 模式下受挫，说明简单的“直接选项”生成并不天然适合批量联合处理。

Q6: 有什么可以进一步探索的点？

论文自己在结论中指出了三条延伸方向，也可以从机制上继续挖掘。第一，从封闭集合决策到开放式智能体工作流：高层智能体把开放式目标分解为视觉检查（标准操作程序或生成计划），提出候选事件、动作、状态供 VideoJev 评估，再用判断指导规划与执行；模糊或意外观测触发候选修订或进一步推理。这里 System Two 组织决策空间，System One 对照视觉证据评估决策空间。应用包括过程监控、交互式助理、具身智能体、计算机使用系统。第二，扩展到更通用的视觉决策任务：动作识别、事件检测、场景状态分类、视觉条件验证都可以表述为对候选假设的决策，扩展候选接口与输出结构可以减少对任务特定模型和预测头的依赖。第三，流式与可靠的视觉交互：流式输入需要增量视觉编码、高效记忆管理和及时决策，即把当前离线视频上的决策接口推进到在线流式场景。第四，可以进一步探索的方向包括：候选区间与候选选项的生成质量对最终性能的上界影响——如果候选覆盖不全，打分头再准也无法召回；打分头的校准性与置信度估计如何用于智能体的不确定性驱动决策；Joint 推理在查询数量更多、查询异构性更强时的扩展性；Cached 与 Joint 的组合策略（先缓存后联合）的调度优化；把同样的结构化决策接口推广到音频、图文交错等模态；以及 System One 决策接口与 System Two 规划器之间的反馈闭环设计。这些后半部分属于合理推断，论文仅提供了初步线索。

Q7: 总结一下论文的主要内容

VideoJev 的核心主张是：结构化视觉判断可以支撑开放式行为，而不需要在每一步都进行开放式生成。论文从智能体的需求出发——定位证据、检查进度、评估候选动作，且经常复用同一段视觉历史——提出把视频理解与交互统一为结构化决策任务。方法上，VideoJev 在预训练 MLLM 之上用一个共享打分头替代自回归答案解码：该头对时间区间和文本选项分别打分，产生事件区间或答案选项，从而统一时间定位与多项选择视频问答。这结合了 MLLM 的语义灵活性与智能体可直接消费的高效决策。针对交互过程中同一视频历史被反复查询的问题，VideoJev 提供两种计算复用机制：Cached inference 让后续查询复用已计算的视觉表示，Joint inference 在单次前向中处理多个查询。实验主线分两条。第一条是质量与效率的权衡：在 CharadesTL、ActivityNetTL、QVHighlightsTL 三个时间定位基准上，VideoJev-4B 与 VideoJev-35B-A3B 相对骨干分别提升平均 mIoU 6.7 与 10.8 点，推理加速 2.8–3.1 倍；在 LVBench、Video-MME、LongVideoBench、MLVU 四个 QA 基准上，性能与骨干 Thinking 模式相当，实测加速超过 100 倍。第二条是机制验证：消融显示多任务训练（时间定位 + QA）在所有七个基准上都优于单任务训练，平均 grounding mIoU 提高 0.7 点、平均 QA 分数提高 1.3 点，最大增益在 LongVideoBench（+1.9 点），表明两类监督互惠互利；推理模式对比显示 Joint 对多查询场景更高效，而 Direct 自回归基线在 Joint 处理下精度下降。论文最后把 VideoJev 定位为面向通用视觉决策的 System One 组件：未来可扩展到动作识别、事件检测、场景状态分类等候选假设决策任务，并由 System Two 规划器组织决策空间、由 VideoJev 对照视频证据评估候选，应用于过程监控、交互式助理、具身智能体与计算机使用系统。整体来看，该工作的论证主线是从智能体决策需求出发论证结构化决策接口的合理性；技术主线是用共享打分头统一两类任务并用两种推理模式复用视觉计算；实验主线是用七基准双规模验证“质量不降、效率大增、多任务互利”。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体方向直接相关：论文的动机就是视觉智能体的及时决策，结尾明确讨论 System Two 规划器组织决策空间、VideoJev 负责视觉证据评估的智能体工作流，可应用于过程监控、交互式助理、具身智能体与计算机使用系统。

## 基本信息

- 作者：Yuhan Zhu, Xiangyu Zeng, Qingyi Si, Chenxu Yang, Yifei Li, Luozheng Qin, Junhao Zhou, Dingyu Yao, Jun Zhang, Chuanyu Qin, Zhiqiu Zhang, Yuandong Yang, Yuqi Xu, Tianyu Zhang, Hongcheng Guo, Jiaqi Wang, Nan Duan, Limin Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-10-13
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.18615v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据片段（method/results/discussion 命中），并结合摘要与论文章节内容补全；部分方法细节与实验设置存在证据缺口，已在对应字段标注需回原文确认。
