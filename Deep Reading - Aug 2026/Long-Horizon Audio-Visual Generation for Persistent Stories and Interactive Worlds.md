---
user_id: "cheng tan"
paper_id: 9151
arxiv_id: "2608.23383v2"
title: "Long-Horizon Audio-Visual Generation for Persistent Stories and Interactive Worlds"
publish_date: "2026-08-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.23383v2.pdf"
pdf_url: "https://arxiv.org/pdf/2608.23383v2"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:07:41"
---
# Long-Horizon Audio-Visual Generation for Persistent Stories and Interactive Worlds

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：audio-visual generation · long-horizon video generation · world model · cross-shot memory

## 一句话总结

JoyAI-Echo-1.5 是一个统一音视频生成系统，包含面向长视频的跨镜头记忆变体和面向交互世界模型的几何控制变体，并通过渐进式教师强制与自生成轨迹上的短/长时域 Self-Gradient Forcing 实现因果少步生成，在长视频一致性与世界模型基准上取得领先结果。

## 摘要

> Video generation is progressing beyond isolated clips toward long-form narratives and interactive worlds, requiring models to preserve identities, follow user controls, and remain stable over extended rollouts. We present JoyAI-Echo-1.5, a unified audio-visual generation system with two purpose-built variants. The long-video variant introduces composable cross-shot memory that aggregates visual evidence across multiple prior shots and speaker cues derived from speech-filtered full-shot audio, enabling persistent character appearance and voice identity across flexible combinations of text, image, and memory conditioning. The world model variant converts heterogeneous navigation inputs into calibrated metric 6-DoF camera trajectories and injects them through a geometry-aware conditioning pathway, enabling controller-agnostic interaction across flexible viewpoints. To support efficient long-horizon generation, we transform a bidirectional audio-visual backbone into a causal few-step generator using progressive teacher forcing and short- and long-horizon Self-Gradient Forcing on self-generated rollouts. Experiments demonstrate strong performance in both settings. JoyAI-Echo-1.5 achieves improvements over existing long-video baselines in cross-shot consistency, visual quality, text alignment, and speech fidelity. Its world-model variant ranks first on WBench, with an average score of 81.7, and achieves leading visual quality and long-horizon persistence on SANA-WM-Bench. Together, these results indicate that memory, geometric control, and rollout-aware training provide a practical foundation for generating coherent stories and continuously evolving interactive worlds.
> Project Page: https://echo-team-joy-future-academy-jd.github.io/Echo-1.5-Page/Code: https://github.com/jd-opensource/JoyAI-Echo

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决视频生成从单片段走向长时程叙事和交互世界时面临的核心问题：1. 跨镜头一致性：长时间故事中角色外观、语音音色需要跨镜头保持一致；现有模型通常在孤立片段上训练，缺乏对先前镜头的记忆利用。2. 用户控制：交互式世界模型需要响应导航控制，但异构输入（如键盘、鼠标、文本、轨迹）难以统一转换为模型可用的精确几何信息，导致控制不准确、不灵活。3. 长时程稳定性：在扩展 rollout 中，模型容易发生漂移、累积误差和身份丢失；现有双向骨干生成器依赖迭代式去噪，推理成本高且难以直接用于流式长时生成。4. 统一框架缺失：长视频生成和世界模型通常作为分离的任务研究，论文希望用一个系统同时支撑叙事生成和交互世界建模，并让记忆、几何控制和 rollout 感知训练成为共同基础。论文隐含的假设是：通过显式记忆机制、6-DoF 轨迹几何条件注入，以及针对自生成轨迹的训练策略，可以同时提升长视频一致性和世界模型可控性，且两者在方法论上可共享。

Q2: 有哪些相关研究？

相关研究主要分布在以下几类：1. 长视频生成：现有方法多基于扩散模型生成短视频片段，再通过拼接或条件扩展生成更长序列；JoyAI-Echo-1.0 已提出长时音视频记忆机制，复用紧凑的视觉与声学线索来保持角色外观和说话人音色一致。2. 世界模型与交互式视频生成：近期的世界模型（如 SANA-WM-Bench 上评测的模型）试图模拟环境对动作的响应，通常以文本或离散动作作为条件；导航类任务常用相机轨迹控制，但缺乏统一几何表示。3. 可控视频生成：基于相机轨迹、深度图或姿态条件的方法已有探索，但多数只处理单目或同构输入，异构导航输入的校准与注入仍是难点。4. 高效生成：从双向扩散骨干转为因果或 few-step 生成器是提高推理效率的方向；Self-Gradient Forcing 可视为 Teacher Forcing 的变体，用于序列生成训练中缓解暴露偏差。论文在前人基础上，将跨镜头记忆、度量轨迹控制和 rollout 感知训练整合到同一系统。需注意：证据片段有限，相关内容多为对论文自身方法与前作（JoyAI-Echo-1.0）的引述，具体外部基线对比需查看原文引用列表。

Q3: 论文如何解决这个问题？

论文提出 JoyAI-Echo-1.5，其解决方案分三个层面：1. 长视频变体：引入可组合的跨镜头记忆（composable cross-shot memory），聚合多个先前镜头的视觉证据，并从语音过滤后的全镜头音频中提取说话人线索，从而在生成当前镜头时能复用角色外观和声音身份信息；条件输入支持文本、图像和记忆的灵活组合。2. 世界模型变体：将异构导航输入（如键盘/鼠标操作、轨迹描述等）转换为校准的度量 6-DoF 相机轨迹，再通过几何感知条件通道（geometry-aware conditioning pathway）注入生成器，实现控制器无关、视角灵活的世界交互。3. 高效长时程生成训练：将双向音视频骨干转换为因果 few-step 生成器；采用渐进式教师强制（progressive teacher forcing）和短时程/长时程 Self-Gradient Forcing，在自生成 rollout 上训练，以减少训练-推理暴露偏差并支持长序列流式生成。整体上，论文强调记忆（memory）、几何控制（geometric control）和 rollout 感知训练（rollout-aware training）三者的协同。方法细节（如网络结构、记忆聚合的具体实现、轨迹校准方式、Self-Gradient Forcing 的公式化描述）在摘要和简介中仅给出概要，具体需参考原文 Method 部分。

Q4: 论文做了哪些实验？

论文在两类任务上进行了实验：1. 长视频生成：对比现有长视频基线，评估跨镜头一致性、视觉质量、文本对齐和语音保真度；报告了相对 JoyAI-Echo-1.0 的提升（具体指标如某某 score 提升 0.0238、0.0144、0.0395，以及 Imaging 0.7467、CLIP 0.2868、speech Recall 0.9674 等，但片段的截图可能为部分指标）。2. 世界模型：在 WBench 上排名第一，平均分 81.7；在 SANA-WM-Bench 上取得领先的视觉质量和长时程持久性。此外还评估了轨迹控制、指令跟随和语音保真度。由于检索证据仅覆盖摘要和部分章节，实验设置的详细描述（数据集规模、baseline 列表、评估协议、消融设计）未在证据中完整呈现；需要查看原文实验章节。

Q5: 发现了什么实验现象？

从检索到的片段可以观察到以下实验现象：1. 长视频变体在跨镜头视觉和说话人一致性上有显著提升：相比 JoyAI-Echo-1.0，三个指标分别提升 0.0238、0.0144、0.0395，表明跨镜头记忆机制有效缓解了身份漂移。2. 在多项指标上取得最佳：JoyAI-Echo-1.5 达到 best Imaging (0.7467)、CLIP (0.2868) 和 speech Recall (0.9674)，同时美学质量保持竞争力，说明一致性的提升没有损害视觉质量。3. 世界模型变体在公开基准上领先：WBench 平均分 81.7 排名第一，SANA-WM-Bench 上视觉质量和长时程持久性领先，表明几何条件注入能有效支持长期交互。4. 轨迹控制和指令跟随表现良好（结论片段提到），但具体数值缺失。注意：这些观察都来自摘要和结论中的高亮描述，缺少消融和负结果细节；例如，长时程 Self-Gradient Forcing 单独贡献多少、渐进式教师强制能否替代双向迭代去噪、是否存在长序列崩溃的边界等，论文原文应包含更多分析，但当前证据未提供。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向：1. 长时程记忆的容量与压缩：当前跨镜头记忆聚合多个先前镜头，但记忆长度、遗忘机制、以及如何在超长故事中保持长期因果依赖（如数百镜头）尚未明确，可研究记忆压缩和检索策略。2. 世界模型的交互闭环：当前支持相机轨迹控制，未来可扩展至对象级交互、物理合理响应和因果动作影响；控制器无关的输入可进一步涵盖语音指令和体感输入。3. 训练效率与少步生成极限：Self-Gradient Forcing 和渐进式教师强制在更长 rollout 下的收敛行为、是否可进一步减少推理步数、是否可与蒸馏方法结合，值得深入。4. 多模态一致性评测：当前指标（Imaging、CLIP、speech Recall 等）可能未完全刻画叙事连贯性和语义记忆，可构建更细粒度的长故事评测集。5. 统一框架的拓展：将记忆机制与几何控制结合到同一模型中，探索叙事生成和交互世界任务之间的迁移收益。6. 失败模式分析：长序列漂移的临界点、罕见身份丢失场景、以及几何轨迹冲突时的行为，需要系统性研究。7. 应用落地：实时交互、可编辑生成的记忆机制、以及将世界模型用于具身智能或游戏内容生成。

Q7: 总结一下论文的主要内容

论文提出 JoyAI-Echo-1.5，一个面向长时程叙事和交互世界的统一音视频生成系统。核心动机是：视频生成正在从孤立片段走向长故事和交互世界，需要模型保持身份、遵循用户控制并在扩展 rollout 中保持稳定。为此，论文构建了两个专用变体。长视频变体采用可组合跨镜头记忆，聚合先前的视觉证据，并从语音过滤后的全镜头音频中提取说话人线索，从而支持文本、图像和记忆条件的灵活组合，实现角色外观和语音身份的持久一致。世界模型变体将异构导航输入转换为校准的度量 6-DoF 相机轨迹，并通过几何感知条件通道注入生成器，实现控制器无关的交互式视角控制。为了让双向音视频骨干适配长时程生成，论文将其转化为因果 few-step 生成器，使用渐进式教师强制和短/长时程 Self-Gradient Forcing 在自生成轨迹上训练，以缓解暴露偏差并提高推理效率。实验结果表明：长视频变体在跨镜头一致性、视觉质量、文本对齐和语音保真度上超过现有基线；世界模型变体在 WBench 上排名第一（平均分 81.7），并在 SANA-WM-Bench 上取得领先视觉质量和长时程持久性。论文总结指出，记忆、几何控制和 rollout 感知训练共同构成了连贯故事和持续演化交互世界的实用基础。整体上，论文的主要贡献在于将三个关键组件（跨镜头记忆、几何轨迹注入、自回归式训练策略）系统整合到统一框架中，并通过公开基准验证了有效性。局限方面（根据推测和片段），当前证据未明确讨论记忆容量上限、长序列失效边界、计算开销细节以及与其他世界模型方法的详细对比；这些在完整论文中应有更全面分析。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文直接属于生成方向，与用户画像中 generation 领域（权重 0.1）契合，且属于系统性工作，符合偏好。

## 基本信息

- 作者：Nan Duan, Haoyang Huang, Weiyang Jin, Haoran Li, Yaowei Li, Yuming Li, Yijun Liu, Xin Lu, Xiaoxiao Ma, Yanwen Ma, Yaofeng Su, Yilang Sun, Haoyu Wang, Zeyue Xue, Songchun Zhang, Junhao Zhuang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-08-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.23383v2`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（abstract、introduction、long-horizon 章节、结论等片段），并结合启发式草稿进行补全；部分细节因证据不足标注为推断或需原文确认。
