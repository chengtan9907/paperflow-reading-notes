---
user_id: "cheng tan"
paper_id: 10038
arxiv_id: "2608.30716v1"
title: "SocialReasonBench: A Video-QA Benchmark for Social Reasoning with Counterfactual Narrative Videos"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.30716v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.30716v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-02T01:47:25"
---
# SocialReasonBench: A Video-QA Benchmark for Social Reasoning with Counterfactual Narrative Videos

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：video question answering · social reasoning · counterfactual reasoning · benchmark construction

## 一句话总结

本文提出 SocialReasonBench，一个基于《底特律：成为人类》分支叙事游戏视频的多选题视频问答基准，用于检验大型多模态模型在意图识别、情感共情、道德困境、反事实推理等七个社会推理维度上的能力，并揭示模型在因果与反事实推理上的明显短板。

## 摘要

> Recent advances in Large Multimodal Models (LMMs) have greatly improved video understanding, yet their ability to reason about human-centered social situations remains limited. Existing benchmarks typically rely on videos with a single observed trajectory, making it difficult to determine whether models truly understand social dynamics or merely exploit recurring narrative patterns. We introduce SocialReasonBench, a video multiple-choice QA benchmark for evaluating socially grounded reasoning in scenarios derived from interactive narratives. Built from gameplay videos of Detroit: Become Human, the benchmark leverages branching storylines where player decisions lead to alternative social outcomes that can be checked against the game's own script, flowchart, and recorded branches. We develop a multi-agent curation pipeline that localizes socially meaningful clips, grounds answer labels in game-state signals, and generates theory-guided questions with diagnostic distractors. SocialReasonBench covers seven reasoning dimensions, including intent recognition, emotional empathy, moral dilemma, counterfactual reasoning, and causal antecedent. Experiments on contemporary LMMs show that models perform reasonably well on basic social understanding but struggle with counterfactual and causal reasoning. Further ablation and diagnostic error analyses reveal that models often depend on incomplete modality cues and fall into reasoning traps such as visual shortcuts, highlighting a gap between observable event recognition and deeper reasoning over latent social states.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的问题是：现有视频问答基准难以真正评估 LMMs 对人类社会动态的理解，因为它们通常只提供单一观察轨迹，模型可以通过记忆或利用叙事模式来“作弊”，而不需要真正进行社会推理。具体而言：1) 现有基准缺乏反事实和因果推理的检验能力，无法区分模型是在理解潜在社会状态还是仅做表面模式匹配；2) 多数视频来源没有可验证的“如果当初做出不同选择会怎样”的分支结果，导致答案标注困难，也难以构造高质量的因果与反事实问题；3) 社会推理是多维的，现有基准往往只覆盖意图或情绪等单一维度，缺少对道德困境、因果前因等深层推理维度的系统覆盖；4) 数据构建过程缺乏可扩展、可验证的流程，人工标注成本高且难以保证标签的客观性。因此，论文希望构建一个既能提供分支叙事、又有客观答案锚点的视频问答基准，并诊断 LMMs 在这些维度上的具体失败模式。

Q2: 有哪些相关研究？

相关研究主要来自两个方面：一是视频问答（VideoQA）与视频理解基准，现有基准大多基于现实视频或电影片段，通常只包含一条时间线，无法支持反事实推理；二是社会推理与意图理解方面的研究，例如 Shi et al. (2025) 的工作关注某种交互，但论文指出其更偏向动作交互而非富含社会性的推理。此外，多模态大模型在通用视觉问答上已有大量工作，但针对社会情境的细粒度推理评估仍然缺乏。由于检索到的证据有限，上述对比主要基于摘要中的一句引用和论文描述；更详细的相关工作综述需要查阅原文 Introduction 部分。合理推断，论文还会引用社会推理理论（如心智理论、道德判断框架）和游戏驱动的 benchmark 构建方法。

Q3: 论文如何解决这个问题？

论文提出的解决方案包含两个核心组件：一是利用《底特律：成为人类》这一交互式叙事游戏作为数据源，二是设计一个多智能体数据策展流程。具体来说：1) 数据源选择：游戏具有分支式故事线，同一场景下玩家不同决策会产生不同的社会结果，且游戏脚本、流程图和已记录分支提供了可验证的客观标签，这为构造反事实问题提供了天然基础。2) 多智能体策展流程：该流程大概包括三步（依据摘要描述，具体细节需原文确认）：（a）从长形式叙事游戏视频中定位具有社会意义的片段；（b）基于游戏状态信号（如角色关系、剧情分支点、决策后果）对答案标签进行 grounding，使正确答案可被游戏内部的客观机制验证；（c）生成理论引导的问题（theory-guided questions）和诊断性干扰项（diagnostic distractors），以覆盖七个推理维度——意图识别、情感共情、道德困境、反事实推理、因果前因等。3) 质量控制：据摘要提到结合多智能体与人工验证，确保问题和标签质量。论文还强调 pipeline 的可扩展性，暗示可迁移到其他交互式游戏。

Q4: 论文做了哪些实验？

论文进行了以下实验（基于摘要和 introduction 片段，具体数据集规模、模型列表和数值需查阅原文）：1) 在当代多个大型多模态模型（LMMs）上评估 SocialReasonBench 上的视频多选题问答表现，报告了总体准确率和各推理维度上的表现。2) 模态消融实验（modality ablation），比较仅视觉、仅音频以及视听组合输入的效果，尤其关注音频线索对情感共情和因果推理的影响。3) 诊断性错误分析（diagnostic error analyses），设计“推理陷阱”（reasoning traps）如视觉捷径，考察模型是否利用显著视觉线索而非深层社会状态来回答问题。4) 消融研究（ablation）可能包括问题类型、干扰项设计的影响，但摘要未明确，需原文确认。5) 与基线或现有 benchmark 的对比也可能存在，但摘要未明确提及。

Q5: 发现了什么实验现象？

实验发现的主要现象包括：1) 模型在基础社会理解任务上表现“相当好”（perform reasonably well），例如意图识别和行为预测，说明 LMMs 已具备一定的社会感知能力。2) 模型在因果前因（causal antecedent）和反事实推理（counterfactual reasoning）上表现挣扎，这是最一致的薄弱环节，表明模型更擅长对已观察事件的分类，而非对潜在因果链和替代可能性的推理。3) 模态消融显示音频线索对情感类（affective）和因果类推理很重要——去掉音频后模型性能下降明显，这反映了人类社交中语音语调、情绪声等非视觉线索的价值。4) 诊断陷阱分析揭示模型常依赖显著视觉线索，例如角色的面部表情或动作，而忽略更深层的社会语境，导致“视觉捷径”（visual shortcuts）式的错误；这些陷阱可能使模型在表面上得分不低，但实际并未触及社会推理核心。5) 模型可能只利用部分模态线索，而非整合全部信息，这解释了为何多模态融合仍不充分。这些观察共同指向一个关键差距：可观察事件识别与潜在社会状态推理之间存在鸿沟。

Q6: 有什么可以进一步探索的点？

基于论文的局限性和实验发现，可以进一步探索的方向包括：1) 将 SocialReasonBench 扩展到多种语言、多种文化背景的交互式游戏，以检验社会推理基准的跨文化泛化性，并避免单一英语游戏带来的文化偏差。2) 尝试更多样的叙事媒体（如电影、电视剧、互动小说）以增加场景多样性，但需保证答案可验证性。3) 针对模型在反事实和因果推理上的弱点，设计新的训练目标或提示策略，例如显式的反事实生成、因果干预模拟，甚至结合世界模型进行推理增强。4) 利用 SocialReasonBench 的细分维度，开发细粒度的诊断工具，帮助识别模型在社会推理过程中具体缺失的是感知、记忆还是推理能力。5) 将基准从视频扩展到第一人称视角或具身交互环境，使社会推理与动作决策闭环。6) 研究如何利用游戏流程图的图结构来引导模型进行结构化推理，明确推理链条。7) 探索人类表现与模型表现的对比，建立人类基线，并分析人类与模型在哪些推理陷阱上存在差异。8) 结合大型语言模型的多智能体辩论或自我反思机制，看是否能提高反事实推理准确率。

Q7: 总结一下论文的主要内容

SocialReasonBench 是一个用于评估大型多模态模型（LMMs）社会推理能力的视频问答基准，其核心动机是：现有视频理解评估通常只提供单一观察轨迹，模型可能通过利用重复出现的叙事模式来获得高分，而并未真正理解社会动态。为了区分“表面模式匹配”和“深层社会状态推理”，论文利用《底特律：成为人类》这款交互式游戏作为数据源，因为其分支式故事线天然提供了反事实分支和可验证的客观结果——游戏脚本、流程图和已记录分支可以作为答案标注的金标准。论文设计了一个多智能体数据策展流程，该流程定位具有社会意义的视频片段，依据游戏状态信号（如决策点、关系变化、结局）标注答案标签，并生成理论引导的多选题和诊断性干扰项。基准覆盖七个推理维度，包括意图识别、情感共情、道德困境、反事实推理和因果前因等，每个维度都试图捕捉社会认知的不同侧面。实验在当代多个 LMMs 上进行，总体显示模型在基础社会理解任务（如意图识别、行为预测）上表现尚可，但在因果前因和反事实推理上明显落后。进一步的模态消融表明音频线索对情感和因果推理至关重要；诊断性错误分析则揭示了视觉捷径问题——模型倾向于依赖显著的视觉特征，而非整合潜在的社会状态信息。论文还讨论了基准自身的局限性：它来自单一英语游戏，且主角是机器人，尽管接近人类，但与真实人类互动仍有距离。总体而言，SocialReasonBench 提供了一个可扩展、可验证的社会推理评估平台，并展示了当前 LMMs 在深层社会推理上的显著缺口，为未来研究指明了改进方向。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与你的研究方向（智能体权重 0.1）直接相关：该基准强调社会推理中反事实和因果能力，这对构建有社会感知能力的智能体至关重要。

## 基本信息

- 作者：Zheyu Huang, Zijing Shi, Haozhe Luo, Huadong Tang, Mingyu Liu, Meng Fang, Ling Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.CV
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.30716v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据（Abstract 与 Introduction 的语义片段），并基于这些片段与摘要进行总结；具体数值和细节需查阅原文。
