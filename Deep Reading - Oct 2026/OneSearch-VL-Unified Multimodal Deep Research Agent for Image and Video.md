---
user_id: "cheng tan"
paper_id: 15178
arxiv_id: "2610.12419v1"
title: "OneSearch-VL: Unified Multimodal Deep Research Agent for Image and Video"
publish_date: "2026-10-08"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.12419v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.12419v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-12T12:49:41"
---
# OneSearch-VL: Unified Multimodal Deep Research Agent for Image and Video

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multimodal deep research agent · visually grounded evidence graph · visual grounding · evidence-aware process reward

## 一句话总结

OneSearch-VL 提出以视觉锚定证据图（VGEG）为核心的统一多模态深度研究智能体，将单图、多图与视频三类输入的任务构建、轨迹筛选、过程奖励与细粒度评测统一到同一任务级参考结构上，在 8B 规模下显著超越带工具访问的 Qwen3-VL-8B 基线。

## 摘要

> Single-image, multi-image, and video deep research require different visual operations but share a workflow of visual grounding, external retrieval, and fact composition. A key challenge is to preserve the dependencies linking localized visual anchors, entity relations, source-supported facts, and answer-producing operations. We introduce OneSearch-VL, a unified agent centered on the Visually Grounded Evidence Graph (VGEG), which encodes these dependencies as a shared task-level reference for data construction, process supervision, and operation-level evaluation. Our VGEG-based data engine constructs and verifies multi-image and video questions and filters expert trajectories. Using these data, we assemble OneSearch-VL-SFT-110K and OneSearch-VL-RL-10K for SFT and RL, respectively. We further derive the Evidence-aware Visual-Grounded Rubric reward (EVGR) from VGEG annotations to supervise evidence traceability and visual grounding during RL. For fine-grained evaluation, we construct OneSearch-MI-Bench and OneSearch-Video-Bench, organizing questions by the research operations encoded in their VGEGs. Experiments show that OneSearch-VL-8B improves over Qwen3-VL-8B with tool access by 20.2 and 17.6 percentage points on the two new benchmarks, respectively, while also achieving substantial gains across 7 image benchmarks and VideoDR. Project repository: https://github.com/appletea233/OneSearch-VL

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：能否用同一个策略（one policy）同时学会在单图、多图和视频三种视觉输入类型上进行深度研究（deep research），而不是像此前工作那样分别针对单图或视频单独设计。具体难点包括：
1. 三种输入类型所需的视觉操作不同：单图需要局部区域定位，多图需要跨图实体关联与对齐，视频需要时序定位与跨帧推理；但它们共享从视觉 grounding、外部检索到事实组合的工作流，因此统一训练有天然动机。
2. 训练数据的构建必须把分布在多个区域、多张图像或多个帧中的视觉实体，与分散在多个网页中的相关事实链接起来；已有表示方法（perception–knowledge chains、factual rubrics、evidence graphs）没有联合满足 grounding 关系、事实支撑关系与答案操作依赖这三者的同时编码。
3. 在数据构建、过程监督和评估三个环节需要同一个任务级参考结构，否则会出现训练目标与评测口径不一致的问题。
4. 现有基准大多只覆盖单图或单视频，缺少按研究操作（research operation）分类的多图与视频深度研究基准，难以定位模型在哪类操作上失败。
论文进一步指出一个范式竞争点：此前工作（如 OpenSearch-VL）主要考虑单图或视频输入，而多图深度研究（multi-image deep research）几乎未被研究；OneSearch-VL 首次把三种输入类型纳入统一框架。

Q2: 有哪些相关研究？

论文的相关工作分布在三条线上：
1. 多模态深度研究智能体：近期工作让 VLM 超越直接作答，通过多轮工具使用主动定位视觉线索、检索外部知识并整合证据（Wu et al., 2025; Geng et al., 2025a; Huang et al., 2026; Chen et al., 2026）。其中 OpenSearch-VL（Chen et al., 2026）是单图深度研究的代表，本文在单图评测协议上直接沿用其七个基准与 GPT-4o 评判方式。
2. 视频深度研究：已有工作把能力从图像扩展到视频，结合时序定位、视觉检查与开放网页检索（Liu et al., 2026; Gao et al., 2026; Fang et al., 2026）。VideoDR（Liu et al., 2026）是本文使用的视频基准之一。
3. 统一多模态模型：统一模型扩展了跨图像与视频的时空理解与推理能力（Li et al., 2025a; Feng et al., 2025b），本文的基座 Qwen3-VL-8B（Bai et al., 2025）属于这一类。
4. 证据表示与过程监督：此前工作分别用 perception–knowledge chains、factual rubrics、evidence graphs 来表示证据并用于任务构建、监督与评测（Jiao et al., 2026; Zhang et al., 2026; Xu et al., 2026; Sun et al., 2026）；论文指出这些结构没有联合满足 grounding、support 与 operation 三类关系的编码需求，这是 VGEG 的切入点。
5. 相关 RL 奖励设计：论文沿用 OpenSearch-VL 的答案正确性奖励 Racc 与查询质量奖励 Rquery，并以 EVGR 过程奖励补充，说明其 RL 技术路线属于 GRPO + 混合奖励的延续。

Q3: 论文如何解决这个问题？

论文解决方案的核心是以 VGEG 作为统一任务级参考，贯穿四个环节：
1. VGEG 定义：为每个研究任务构建一张关系图，节点包括视觉锚点（视觉源上的区域或帧）、外部事实（来自网页片段）与答案操作；边编码三类关系：grounding 关系（锚点↔视觉源）、support 关系（事实↔来源片段）、以及任务特定的依赖关系 Ri（锚点、事实与操作之间的依赖，决定所选事实如何产生答案）。论文明确提到 Ri 结合了从基础证据图 GX 继承的 grounding 与 support 关系及任务特定依赖，即 VGEG 是基础证据图向任务级依赖的扩展。
2. VGEG 数据引擎：基于该结构自动构建并验证多图与视频问题（包括负样本/伪问题过滤），并筛选专家轨迹，得到 SFT 数据集 OneSearch-VL-SFT-110K（约 110k 样本，覆盖单图、多图、视频三类输入）与 RL 数据集 OneSearch-VL-RL-10K。RL 数据的筛选标准是：在统一工具环境中对每个候选任务做 8 次独立 rollout，保留满足 0 < ncorrect < 8 的任务（即既有成功又有失败、具有可学习难度的任务），去重后用于 GRPO。
3. EVGR 过程奖励：从 VGEG 标注中为每个训练样本生成结构化 rubric，包含答案核心实体、所需事实跳数（fact hops）、支撑来源与片段、适用的图像/区域/帧位置。评判器（judge）接收 rubric 与轨迹记录，对工具调用及其参数、返回观测进行证据可追溯性评估；对视觉 grounding 评估时，评判器额外接收多模态输入。论文使用 ByteDance Seed 2.0 Pro 作为评判模型（在两个独立评判角色中）。最终奖励为格式有效性门控的加权组合 R(τ)，包含 Racc、Rquery 与 EVGR。
4. 统一智能体训练：以 Qwen3-VL-8B 为基座，SFT 用 OneSearch-VL-SFT-110K（全参数微调，视觉塔与 MM projector 冻结，ZeRO-2，bfloat16，32,768 token 截断，8 epochs，lr 2e-5，cosine 调度，64 张 H800 训练 4 天），RL 用 OneSearch-VL-RL-10K 做 GRPO，策略损失只施加在模型生成的 reasoning、工具命令与最终回答 token 上，工具观测只作为条件上下文；被终端工具错误截断的轨迹只应用部分损失。
5. 统一工具环境：TextSearch、ImageSearch 等外部搜索工具（表 1），专家轨迹合成、在线 RL rollout 与推理共用同一 dispatcher 与工具接口，保证训练/评测协议一致。
6. 操作级评测：构建 OneSearch-MI-Bench 与 OneSearch-Video-Bench，问题按 VGEG 编码的六个主要研究操作（Section 4）分类，从而不只是给总分，还能定位模型在具体操作上的强弱。

Q4: 论文做了哪些实验？

论文的实验包括：
1. 主实验：
- 单图评估：沿用 OpenSearch-VL 的七个知识密集型基准：SimpleVQA、VDR、MMSearch、LiveVQA、BrowseComp-VL、FVQA、InfoSeek。
- 多图与视频评估：使用自建 OneSearch-MI-Bench 与 OneSearch-Video-Bench，外加 VideoDR；在两个 OneSearch 基准上额外按六个主要研究操作报告分项性能。
- 评估协议：沿用 OpenSearch-VL，用 GPT-4o 对最终回答与参考答案做二元正确性比较。
2. 主要结果：OneSearch-VL-8B 在 OneSearch-MI-Bench 与 OneSearch-Video-Bench 上相对带工具访问的 Qwen3-VL-8B 分别提升 20.2 与 17.6 个百分点；在 7 个单图基准与 VideoDR 上也有显著提升。
3. 消融实验：
- 轨迹类型互补性：在 SimpleVQA、OneSearch-MI-Bench、OneSearch-Video-Bench 上做消融，显示三种轨迹来源（单图、多图、视频）在统一训练设置下互补；把多图或视频轨迹加入单图数据都能提升平均分，组合全部三类轨迹进一步改善总体性能。
- RL 奖励消融：表 4(b) 分析答案正确性 Racc、查询质量 Rquery 与 EVGR 各自贡献。
4. 训练配置：SFT 用 64 张 H800（8 节点 × 8），有效 batch size 128，训练 4 天；RL 数据通过 8 次 rollout 筛选保留 0 < ncorrect < 8 的任务。
5. 附录包含完整优化、rollout、奖励与推理配置。
注意：检索到的 results 片段主要是表格残片与消融说明文字，逐项数值（如 16.0、8.5、29.5 等）分不清对应哪个 benchmark/模型列，无法可靠转述具体数字；20.2 与 17.6 两个主要提升数字来自摘要，属论文明确声明。

Q5: 发现了什么实验现象？

论文实验中呈现的现象与趋势：
1. 统一训练的有效性：单一策略同时在单图、多图、视频三类输入上训练后，在各自基准上都有提升，说明三类深度研究任务共享的工作流足以支撑跨输入类型的知识迁移，而不是互相干扰。
2. 轨迹类型的互补性：消融显示单独加入多图轨迹或视频轨迹都能提高平均分，三类轨迹组合后性能最高；这支持“多任务数据互补”而非“数据竞争”的结论。论文原文表述为“Combining trajectory types further improves overall performance. Adding either multi-image or video trajectories to the single-image data raises the average score.”
3. EVGR 的增益：RL 奖励消融（表 4b）表明在 Racc 与 Rquery 基础上加入 EVGR 过程奖励能进一步提升，说明过程级证据可追溯性与视觉 grounding 监督确实转化为最终答案质量，而不是被答案正确性奖励完全覆盖。
4. 规模效率：OneSearch-VL-8B 在多个单图基准与视频基准上超越更强基座 Qwen3-VL-32B 的数值表现（检索到的 results 表格残片中可见 32B 与 8B 的对比行），表明统一数据与奖励设计带来的增益大于单纯模型规模翻倍带来的增益。注意：表格残片中的具体列名与数值对应关系不完整，这里只做趋势性描述。
5. 反直觉点：带工具的多模态智能体在 8B 规模即可大幅超过 32B 基座的 agentic 工作流，说明外部工具调用质量与过程监督可能比参数规模更关键。
6. 负结果与失败模式（来自 limitations）：依赖外部搜索工具与网页动态变化，证据可用性与精确可复现性受影响；工具失败会导致轨迹被截断。
7. 指标间张力：论文同时报整体准确率与按操作分类的细粒度分数，但没有说明是否出现某个操作分数与总分方向不一致的情况；检索证据不足，无法确认。

Q6: 有什么可以进一步探索的点？

论文自己在 Limitations and Future Work 中明确了以下方向：
1. 检索快照保存：保留 retrieval snapshots 以缓解网页动态变化导致的证据可用性波动，提升精确可复现性。
2. 工具失败鲁棒性：评估并增强对 TextSearch/ImageSearch 等工具失败的鲁棒性（当前终端工具错误会截断轨迹）。
3. 自动化 VGEG 构建的可靠性：当前自动构建可能引入标注错误，需要 human calibration。
4. 模型化奖励的偏差：用更强的开放式多模态过程评判器（open multimodal process judges）替代或补充当前评判，提升轨迹级评估可靠性。
5. 多轮交互成本：多轮交互带来额外推理与工具开销，需要研究成本-性能平衡。
此外，从论文结构可合理推断的扩展方向：
6. VGEG 用于可解释性与追溯：把 EVGR 的 rubric 用于推理时可视化，给出证据链而非仅分数。
7. 操作级评测迁移到更多场景：六类研究操作分类可用于诊断其他多模态 agent 的短板。
8. 扩展到音频或更多模态组合；把 VGEG 与更细粒度的帧级/时空 grounding 结合。
9. 数据引擎的进一步自动化：减少对 Seed 2.0 Pro 等外部评判模型的依赖，或将其替换为可复现的开源评判器。

Q7: 总结一下论文的主要内容

OneSearch-VL 是面向单图、多图与视频三类视觉输入的统一多模态深度研究智能体，核心贡献是把“视觉锚定 → 外部检索 → 事实组合”这一共享工作流抽象为 Visually Grounded Evidence Graph（VGEG）。
论证主线：论文从三个观察出发——(1) 三类深度研究任务虽视觉操作不同但工作流共享；(2) 已有证据表示（perception–knowledge chains、factual rubrics、evidence graphs）没有联合编码 grounding、support 与 operation 三类依赖；(3) 多图深度研究几乎未被研究。由此推出需要一个统一的任务级参考结构 VGEG，并让它同时服务数据构建、过程监督与评测。
技术主线：VGEG 编码三类节点（视觉锚点、外部事实、答案操作）与三类关系（grounding、support、任务依赖 Ri）。基于 VGEG 的数据引擎自动构建并验证多图/视频问题、过滤专家轨迹，产出 SFT-110K 与 RL-10K 两套数据；从 VGEG 标注推导 EVGR rubric 奖励，在 GRPO 中与 Racc、Rquery 组合，门控于格式有效性；训练采用 Qwen3-VL-8B 全参 SFT + RL 两阶段，统一工具环境（TextSearch/ImageSearch 等，dispatcher + 工具接口）贯穿专家轨迹合成、在线 rollout 与推理。训练细节：视觉塔与 MM projector 冻结，ZeRO-2，bfloat16，有效 batch 128，8 epochs，lr 2e-5，64 张 H800 训 4 天；RL 数据通过每条候选任务 8 次 rollout 筛选 0 < ncorrect < 8 的任务；策略损失只作用于模型生成 token，工具观测仅作条件上下文。
实验主线：单图用 OpenSearch-VL 的七个知识密集型基准（SimpleVQA、VDR、MMSearch、LiveVQA、BrowseComp-VL、FVQA、InfoSeek），多图/视频用自建 OneSearch-MI-Bench、OneSearch-Video-Bench 加 VideoDR；自建基准按 VGEG 编码的六类主要研究操作组织；GPT-4o 做二元正确性评判。主结果：相对带工具访问的 Qwen3-VL-8B，在两个新基准上分别提升 20.2 与 17.6 个百分点，并在 7 个单图基准与 VideoDR 上显著提升。消融验证三类轨迹来源互补、EVGR 奖励有效。结论称 OneSearch-VL 在七个单图基准与两个操作级基准上一致超过所评估的开源 baseline。局限包括对外部工具的依赖、网页动态性、自动 VGEG 构建错误与模型评判偏差、多轮推理成本。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与智能体（agent）方向直接重合：OneSearch-VL 是多轮工具调用、外部检索与 RL 训练结合的深度研究智能体，可作为多模态 agent 数据构建与奖励设计的参考案例。

## 基本信息

- 作者：Hongyu Li, Manyuan Zhang, Kaituo Feng, Shu Chen, Dian Zheng, Hao Li, Hao Yu, Zhangquan Chen, Zoey Guo, Ray Zhang, Shaofei Huang, Tianrui Hui, Linjiang Huang, Si Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-08
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.12419v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成综合了摘要、Introduction/Method/Results/Conclusion 的 PDF 语义检索证据与元数据；检索证据包含轨迹示例、奖励组合残片、消融说明与表格残片，但完整 VGEG 公式、表头与逐项数值需回原文核对。
