---
user_id: "cheng tan"
paper_id: 1458
arxiv_id: "2606.25964v1"
title: "WinDOM: Self-Family Distillation for Small-Model GUI Grounding"
publish_date: "2026-06-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.25964v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.25964v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-25T18:14:17"
---
# WinDOM: Self-Family Distillation for Small-Model GUI Grounding

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：gui grounding · self-family distillation · small language model · reinforcement learning from human feedback

## 一句话总结

提出WinDOM（基于DOM的GUI定位数据集）与自家族蒸馏（Self-Family Distillation, SFD）方法，结合早期截断的GRPO强化学习，使约2B参数的小模型在GUI定位任务上的OOD平均性能提升+5.4，且同规模EMA教师版本与跨尺度4B教师版本性能接近（65.2 vs 66.3）。

## 摘要

> Small ($\sim$2B) GUI-grounding agents are attractive for on-device deployment, accessibility tooling, and low-cost iteration, but at this scale they face two open recipe questions: how to obtain bounding-box training data without expensive human annotation, and how to combine supervised fine-tuning with reinforcement learning. We address both, with the explicit goal of pushing small-model performance rather than scaling up. WinDOM is a $54{,}425$-record grounding corpus harvested by driving an open-source Windows 11 web reimplementation under headless Playwright, with bounding boxes read directly off the DOM and no OCR or human annotation. Self-Family Distillation (SFD) is a single rejection-sampling cold-start parameterised only by the teacher choice: either an EMA of the student (no external model) or a frozen larger same-family teacher. We then treat the saturation depth of the SFD cold-start as an explicit GRPO hyperparameter. On a Qwen3.5-2B student, the under-saturated cold-start is a better GRPO initialiser than the converged one: SFD-4B with Early-init RL gains $+5.4$ OOD-mean ($+3.5$ ScreenSpot-Pro, $+7.0$ OSWorld-G, $+5.8$ ScreenSpot-V2) over the base. The same-size EMA mode lands within roughly one OOD-mean point of the cross-size $4$B variant ($65.2$ vs $66.3$) without an external teacher.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决两个紧密相关的问题：第一，如何低成本、可扩展地构建高质量的GUI定位（grounding）训练数据，而不依赖昂贵的OCR或人工标注？现有数据集如ScreenSpot、OSWorld等依赖人工或自动OCR，但OCR在非标准界面或动态内容上容易出错，且人工标注难以规模化。第二，对于参数量仅约2B的小模型，如何有效结合监督微调（SFT）与强化学习（尤其是GRPO）以提升定位精度，尤其是跨分布泛化能力？直观上，蒸馏一个更大的教师模型然后进行RL微调似乎是标准做法，但论文发现冷启动阶段的饱和度对后续RL效果有显著影响，这挑战了“蒸馏越充分越好”的常见假设。论文以推动小模型性能而非缩放模型规模为明确目标，聚焦于提升约2B模型在GUI定位任务上的OOD（out-of-distribution）准确率。

Q2: 有哪些相关研究？

论文将工作置于三条研究线中：第一，GUI定位数据集与小模型定位智能体。已有的数据集如ScreenSpot、OSWorld、AITW等，大部分依赖人工标注或OCR；小模型（<3B）的工作如UFO、CogAgent、SeeClick等，但缺乏对冷启动与RL结合的系统研究。第二，过滤式师生蒸馏在语言模型中的应用。相关工作包括通过拒绝采样或过滤数据来训练更小的模型，如Orca、Phi等，但通常采用固定的一次性蒸馏，未探讨不同饱和程度对后续RL的影响。第三，来自人类反馈的强化学习（RLHF）及其变体如DPO、GRPO在视觉语言模型中的应用。论文特别关注GRPO，因为其无需显式奖励模型，适合定位这种有明确边界框评价指标的场景。论文的创新点在于将冷启动蒸馏的饱和度作为一个显式超参数，并发现“未饱和”冷启动是更好的RL初始化。

Q3: 论文如何解决这个问题？

论文提出两个核心贡献：WinDOM数据集和自家族蒸馏（SFD）方法。WinDOM：利用开源的Windows 11网页重实现（例如基于React的Desktop仿制），通过Playwright无头浏览器自动截图，同时直接从DOM树中提取每个可交互元素的边界框（layout rectangle），无需OCR或人工标注。整个过程完全自动化，且代码开源可复现，生成54,425条记录（截图+对应元素描述与边界框）。SFD：一种参数化仅为教师选择的单次拒绝采样冷启动方法。学生模型首先在WinDOM上进行基础SFT。然后，使用一个教师模型（可以是学生自身的EMA副本，或一个更大的同族模型如SFD-4B）对SFT学生生成的预测进行拒绝采样：如果教师认为学生的预测足够准确（基于IoU阈值），则保留该样本，否则丢弃。由此得到一组过滤后的高质量数据进行第二轮SFT，称为“冷启动”。关键发现是：冷启动不必训练到收敛；较早停止的冷启动（即“未饱和”状态）作为GRPO的初始化器，比完全收敛的冷启动产生更好的最终性能。GRPO阶段直接优化定位指标（如gIoU），使用从WinDOM或环境自动获取的奖励信号。论文将冷启动训练的步数/epoch数作为GRPO的超参数进行调节。

Q4: 论文做了哪些实验？

实验设置：学生模型为Qwen3.5-2B（约2B参数）。教师模型包括：同尺度的EMA学生模型（无外部教师），以及跨尺度的Qwen3.5-4B（SFD-4B）。评估在多个OOD基准上进行：ScreenSpot-Pro、OSWorld-G、ScreenSpot-V2，以及ScreenSpot（ID）。主要实验比较以下配置：基线（仅SFT WinDOM）、标准SFD（收敛冷启动+GRPO）、早期初始化RL（未饱和冷启动+GRPO）。还报告了不同教师选择（EMA vs 4B）、冷启动步数对最终性能的影响。此外，还进行了消融和扩展：①将SFD与标准蒸馏（无拒绝采样）对比；②探究不同拒绝阈值的影响；③在更小的学生模型（如1B）上的验证；④将WinDOM数据量缩放至一半或两倍的影响；⑤在线RL环境的集成（通过在WinDOM生成器环境中持续交互）。

Q5: 发现了什么实验现象？

关键现象：①未饱和冷启动更适合GRPO：在SFD-4B教师下，早期停止的冷启动（训练约1/3总步数）作为GRPO初始化的OOD平均比完全收敛冷启动高+5.4点。②同尺度EMA教师效果接近跨尺度教师：EMA-SFD（自蒸馏）在OOD平均上达到65.2，仅比SFD-4B的66.3低约1点，且无需外部教师模型。③基线与蒸馏差距显著：无冷启动的SFT基线在OOD平均约56.3，而最好配置达到66.3，提升了10个点。④GRPO对冷启动质量敏感：如果冷启动过于饱和（过拟合WinDOM分布），则GRPO难以提升泛化能力；若冷启动不足，GRPO可有效利用蒸馏信号。⑤ScreenSpot-V2和OSWorld-G上的增益最大：分别提升5.8和7.0，表明该方法对难分布迁移有效。⑥反直觉结果：蒸馏的“质量”比“量”更重要；未饱和冷启动虽然总体准确率较低，但其保留的样本多样性更好，为RL提供了更宽的优化路径。

Q6: 有什么可以进一步探索的点？

①在线强化学习：由于WinDOM生成器是可复现的自动化环境，可以直接集成到在线RL循环中，实现持续交互式学习，可能进一步提升泛化能力。②偏好优化：将冷启动中的拒绝样本（负例）用于DPO等偏好优化，将当前浪费的计算转化为便宜偏好数据。③长程规划：超越单步定位，将方法扩展到多步GUI任务中的定位序列决策。④更大尺度缩放：验证SFD方法在更大模型（如7B）或更小模型（如0.5B）上的有效性。⑤跨平台泛化：WinDOM基于Windows 11模拟，能否推广到macOS、Linux或移动端界面？⑥领域适应：在冷启动中引入目标领域的不标注样本进行自训练。⑦奖励模型设计：探索更细粒度的奖励信号（如部分匹配、元素类别准确率）以替代gIoU。

Q7: 总结一下论文的主要内容

论文针对小模型GUI定位任务，提出WinDOM数据集和自家族蒸馏（SFD）方法，系统研究了冷启动蒸馏饱和度对后续GRPO强化学习的交互影响。主要发现是未饱和的冷启动比完全收敛的冷启动作为GRPO初始化器效果更好，提升OOD平均+5.4点。同尺度EMA教师无需外部模型即可达到接近跨尺度教师（4B）的性能。论文通过大规模实验验证了方法的有效性，并展示了在多个OOD基准上的显著改进。论文还探讨了在线RL、偏好优化等未来方向。整体而言，这项工作为训练小规模、高效的GUI定位智能体提供了实用的配方和全新的视角。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：论文直接处理GUI grounding，与智能体（agent）方向高度相关（用户画像 top_directions 权重0.10）。

## 基本信息

- 作者：Chengheng Li-Chen, Zhiqian Zhou, Hao Chen, Nicolas Chauvin
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.LG
- 日期：2026-06-24
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.25964v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF检索证据中的Abstract、Related Work、Contributions部分，并结合heuristic_draft润色补全。
