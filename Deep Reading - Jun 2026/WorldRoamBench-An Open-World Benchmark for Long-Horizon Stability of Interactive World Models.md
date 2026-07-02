---
user_id: "cheng tan"
paper_id: 2159
arxiv_id: "2606.31672v1"
title: "WorldRoamBench: An Open-World Benchmark for Long-Horizon Stability of Interactive World Models"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.31672v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.31672v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-07-02T02:01:30"
---
# WorldRoamBench: An Open-World Benchmark for Long-Horizon Stability of Interactive World Models

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：interactive world models · benchmark · long-horizon stability · action following

## 一句话总结

WorldRoamBench 是一个针对交互世界模型长期稳定性的开放世界基准，通过逐帧动作指标、分段视觉漂移度量、受控物理评估和动作解耦内存协议，揭示了现有模型在多维度长期交互中的系统性失效。

## 摘要

> Despite rapid progress in interactive world models (IWMs), existing benchmarks evaluate action following only at trajectory level and ignore memory and interaction physics. We introduce WorldRoamBench, an open-world benchmark for long-horizon stability across four dimensions, each with tailored innovations: (i) Action: per-frame action metric bypassing cross-model semantic scale disparity and exposing failures hidden by trajectory; (ii) Vision: segment-based drift metric capturing non-monotonic mid-sequence collapse missed by start-vs-end comparisons; (iii) Physics: controllability-gated evaluation over mechanics, optics, and 3D consistency, scoring plausibility under faithful action execution; (iv) Memory: action-decoupled protocol evaluating scene memory via transition-localized 3D point-cloud reconstruction and subject memory via tracking-plus-VLM reasoning. The benchmark comprises 600+ test cases across Nature, Urban, and Indoor scenes in first/third-person views with WASD 10-60s continuous interaction. Evaluating 10+ open/closed-source models reveals none reliably satisfies all dimensions; even the best achieves only moderate scores. Advances on WorldRoamBench are steps toward IWMs that are stable, physically grounded, memory-faithful, and deployable in real-world applications.

Q1: 这篇论文试图解决什么问题？

现有交互世界模型基准存在三个关键缺口：1) 动作遵循仅在轨迹层级聚合评估，无法暴露单帧级别的动作偏差，且跨模型语义尺度不一致导致隐藏失败；2) 视觉稳定性评估只比较首尾帧，忽略中间序列的非单调漂移或突然崩溃；3) 交互物理和交互过程中的记忆保持被完全忽视，缺乏在长期交互中对物理一致性及场景/主体记忆的系统化评测。

Q2: 有哪些相关研究？

相关研究包括：WorldOlympiad（通过逐块展开联合评估物理、几何和交互）；WildWorld（在单一游戏中评估游戏手柄动作遵循）；以及大量基于短视频片段（~5-10秒）的基准如 VideoGPT, Phenaki 等。这些工作均局限于短时交互或单一维度，缺乏对长期稳定性、交互物理和记忆的全面评估。

Q3: 论文如何解决这个问题？

WorldRoamBench 提出四维度评估协议：1) Action：逐帧动作度量，使用键击级对比避免跨模型语义差异，暴露轨迹级聚合隐藏的失败；2) Vision：基于段落的视觉漂移度量，捕获中间序列的非单调退化，克服首尾帧比较的盲区；3) Physics：可控生成下的物理评估，对力学（重力、碰撞）、光学（反射、阴影）和 3D 一致性（深度、视角连续性）进行评分，要求模型在忠实执行动作时保持物理合理性；4) Memory：动作解耦的内存协议，场景内存通过过渡定位的 3D 点云重建评估，主体内存通过联合跟踪与 VLM 推理评估。测试集合包含 600+ 案例，覆盖 Nature、Urban、Indoor 三类场景，支持第一/三人称视角，交互时长 10-60 秒，动作空间为 WASD/IJKL。

Q4: 论文做了哪些实验？

论文使用 WorldRoamBench 对 10+ 个开源和闭源交互世界模型进行了系统评估，包括知名视频生成模型（如 CogVideo 等，具体列表见原文 Appendix）。每个模型接收相同初始帧和动作程序，自回归生成视频，然后在四个维度上分别评分。实验涵盖不同场景类型、视角和交互时长，并进行了维度间的消融分析（如移除记忆解耦的影响）以验证各度量的有效性。

Q5: 发现了什么实验现象？

评估揭示以下关键现象：1) 动作维度：逐帧度量暴露了轨迹级度量无法发现的间歇性动作失败，例如模型在长序列中偶尔忽略关键动作；2) 视觉维度：分段漂移度量捕捉到中间帧的突然质量崩塌，而首尾帧比较显示稳定；3) 物理维度：模型普遍在力学（物体穿模、重力异常）和光学（阴影闪烁、反射错误）上表现糟糕，3D 一致性在相机旋转时下降明显；4) 内存维度：场景内存（通过点云重建准确度）在 20 秒后快速衰退，主体内存（追踪+VLM 问答正确率）在涉及遮挡时大幅下降；5) 无模型在所有维度达到可靠表现，最佳模型也仅在动作和视觉获得中等分数，物理和内存明显不足；6) 存在可能反直觉的发现：动作遵循好的模型不一定视觉稳定，反之亦然，说明维度间独立性高。

Q6: 有什么可以进一步探索的点？

1) 扩展场景多样性，覆盖更多开放式环境（如水下、太空）以提升泛化性；2) 引入连续动作空间（如模拟摇杆）和复合动作（同时按键）评估；3) 将基准用于训练或强化学习奖励设计，直接优化长期稳定性；4) 研究超过 60 秒的超长交互行为，探索记忆退化边界；5) 结合多模态信号（音频、触觉）评测世界模型的多感知交互能力；6) 开发专门针对内存和物理一致性的模型架构改进方向。

Q7: 总结一下论文的主要内容

WorldRoamBench 是一项针对交互世界模型长期稳定性的开放世界基准工作。论文首先指出现有基准在评测长时交互时的核心不足：仅在轨迹层级聚合评估动作遵循，忽略单帧偏差；视觉稳定性仅比较首尾帧，遗漏中间崩溃；交互物理和交互记忆被完全忽视。为了填补空白，作者设计了四个评估维度并各自提出创新度量：动作维度采用逐帧键击对比，消除跨模型语义差异；视觉维度引入基于段落的分段漂移度量，捕捉非单调退化；物理维度在可控生成下对力学、光学和 3D 一致性评分，保证动作执行的物理合理性；内存维度通过动作解耦协议，分别用过渡定位的 3D 点云重建评估场景记忆，以及联合跟踪与 VLM 推理评估主体记忆。基准包含 600+ 个测试案例，覆盖自然、城市、室内三种场景，支持第一/三人称视角，使用 WASD/IJKL 动作控制，交互时长 10-60 秒。论文评估了 10+ 个开源和闭源模型，实验表明没有任何模型能同时满足所有维度要求，即便最佳模型也仅在动作和视觉上达到中等水平，在物理和内存维度表现显著不足。具体观察包括：逐帧动作度量揭示了隐藏的间歇性失败，分段视觉度量捕获了中间质量的突然崩塌，物理模拟在长序列中频繁违反重力、阴影和深度一致性，内存快速衰退。这些发现强调了发展稳定、物理合理、记忆可靠的交互世界模型的必要性，并确立了 WorldRoamBench 作为未来标准化评估平台的基准。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：如果你研究交互世界模型，WorldRoamBench 提供了标准化框架以系统诊断模型的长期稳定缺陷，可直接用于模型对比和改进

## 基本信息

- 作者：Ting-Bing Xu, Jiacheng Sui, Zhe Gao, Kewei Shi, Wenjin Yang, Zhicheng Liu, Zhaoxu Sun, Mingchao Sun, Hongyu Pan, Fan Jiang, Mu Xu, Qi Fan, Yong Li, Baoquan Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.31672v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据。
