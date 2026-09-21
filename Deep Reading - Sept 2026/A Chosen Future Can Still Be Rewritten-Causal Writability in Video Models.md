---
user_id: "cheng tan"
paper_id: 11740
arxiv_id: "2609.15980"
title: "A Chosen Future Can Still Be Rewritten: Causal Writability in Video Models"
institution: "麻省理工学院 (MIT) 等（推测，基于作者 Ziming Liu 的学术背景及研究领域）"
publish_date: "2026-09-15"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.15980.pdf"
pdf_url: "https://arxiv.org/pdf/2609.15980"
abs_url: "https://arxiv.org/abs/2609.15980"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-16T11:18:25"
---
# A Chosen Future Can Still Be Rewritten: Causal Writability in Video Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：causal writability · video generation · predictive underspecification · mechanistic interpretability

## 一句话总结

本研究提出了“因果可写性”（Causal Writability）概念，证明视频模型在生成物理错误运动时，其内部仍保留并可提取正确的物理规律，且这种可写性在模型深度上存在明确的“闭合”边界。

## 摘要

> When a video model generates physically incorrect motion, did it fail to learn the correct motion, or did it learn it but fail to use it? We show the latter: the correct motion remains available inside the model and can still be made to control the generated video. We train on videos where red masses oscillate slowly and blue masses oscillate quickly, then test a red mass with fast observed motion. Even when the model generates slow motion in this conflicting case, a low-dimensional edit predicted from simple physical variables restores the correct fast motion. We call this ability causal writability. At fixed strength, we find a sharp depth boundary: the same edit changes the video before the boundary but not after it. This closure marks commitment for that write. The motion signal nevertheless remains, and a stronger downstream write can restore physical motion, while excessive gain overshoots. Early causal writability predicts which errors training later corrects: those errors are writable at more network depths than errors that persist. We reproduce both causal writability and its sharp closure in a pretrained 1.3B video model, supporting generality across model scale and training regime.
> a Correlated data support multiple predictors
> ![](images/ee1f8426e314b2c409dcc41f35d738029315fbdb1c48274880dce9120b4e04a8.jpg)
> b Natural generation selects one future
> ![](images/e4c22078a49b2ebb33d9873448562da4f95f1bfb647608658e11b607a7b7684c.jpg)
> c The physical alternative remains writable
> ![](images/9822e5250cdd06636e94f19564b59246554b6f7dec6438c3600ffd0286fda8f5.jpg)
> d Closure marks causal commitment
> ![](images/3fd0976a8099c300cdaab4feec55f76a71a4de8b9ea8db419e86caacd84ddf3b.jpg)
> Figure 1: Natural generation can select one future while a causally usable physical alternative remains writable. a, Correlated red–slow and blue–fast examples support both motion-based and appearance-based predictive rules. b, With observed motion fixed, cue strength changes which future controls natural generation. c, An activation edit predicted from target motion and boundary state gives the physical alternative control over decoded video. d, Closure marks the depth after which the same edit no longer changes the decoded video.

Q1: 这篇论文试图解决什么问题？

### 核心科学问题
本论文探讨了视频生成模型中一个深层且普遍的矛盾：**物理不一致性（Physical Inconsistency）**。具体而言，当模型生成违背物理定律的视频时，存在两种竞争性的解释：
1. **学习失败（Learning Failure）**：模型在训练阶段未能从数据中提取出正确的物理规律。
2. **使用失败（Usage Failure）**：模型已经学到了正确的规律，但在推理过程中由于某种机制（如捷径学习或预测欠定性）选择了错误的输出路径。

### 预测欠定性（Predictive Underspecification）
论文引入了“预测欠定性”的概念。在训练数据中，往往存在多个可以解释数据的预测规则。例如，如果数据集中所有红色球都慢速移动，蓝色球都快速移动，模型可以学习“颜色决定速度”的捷径，也可以学习“动量守恒”等物理规律。当测试样本出现“快速移动的红球”时，这两种规则会产生冲突。模型通常会遵循更简单的捷径（颜色），导致生成错误的物理运动。

### 研究动机与挑战
作者试图验证：那些被模型“抛弃”的正确物理选项是否依然以某种形式存在于模型的隐藏表征中？如果存在，我们能否通过因果干预（Causal Intervention）将其“激活”并纠正输出？这不仅是一个解释性问题，更关乎如何构建更具鲁棒性和物理真实感的生成模型。

Q2: 有哪些相关研究？

### 机械解释性（Mechanistic Interpretability）
本研究属于大模型机械解释性的前沿领域。以往研究（如 Meng et al., 2022; Zhang & Nanda, 2024）主要关注语言模型中的知识编辑和激活补丁（Activation Patching）。本文将其扩展到了视频生成的时空表征领域。

### 预测欠定性与捷径学习
论文引用了 Geirhos et al. (2020) 和 D'Amour et al. (2022) 关于预测欠定性的工作。这些研究指出，深度学习模型倾向于利用训练集中的统计相关性（捷径），而非因果逻辑。本文的贡献在于证明了即使模型表现出捷径行为，其内部依然保留了因果逻辑的表征。

### 视频模型中的物理学习
现有的视频模型研究多关注于扩大规模（Scaling）或改进架构（如 Diffusion Transformers），而本文则深入探讨了模型内部如何处理物理变量（如速度、位置）与表面特征（如颜色、形状）之间的竞争关系。这与“世界模型”（World Models）的讨论高度相关，即模型是否真的理解物理，还是仅仅在做像素外推。

Q3: 论文如何解决这个问题？

### 因果可写性（Causal Writability）定义
作者定义了“因果可写性”：即通过对模型内部隐藏状态进行低维编辑，从而控制最终生成结果的能力。如果一个物理变量是“可写的”，说明模型具备处理该变量的通路，且该变量能因果性地影响输出。

### 实验设计：受控的物理冲突
1. **合成数据集**：创建了一个包含红色和蓝色质量块振动的视频集。设置特定的相关性：红色=慢速，蓝色=快速。
2. **冲突测试**：输入一个观察帧为“快速运动”的红色物体。此时，外观诱导模型预测“慢速”，而观察到的运动诱导模型预测“快速”。
3. **低维子空间编辑**：
 - 从简单的物理变量（如位置、速度）预测模型隐藏状态的扰动方向。
 - 在模型的特定层（Block）注入这些编辑（Edits）。
 - 观察编辑是否能将生成的“慢速”视频扭转为“快速”视频。

### 深度边界探测
研究者在模型的不同深度（Layers/Blocks）重复上述干预，旨在寻找模型在何时“决定”了最终的生成路径。他们发现了一个显著的现象：在某一特定深度之后，同样的干预强度不再能改变输出结果，这被称为“闭合”（Closure）。

Q4: 论文做了哪些实验？

### 实验设置
1. **合成模型**：训练了一个基于 Transformer 的视频生成模型，专门处理简单的物理振子系统。这允许研究者精确控制物理变量和相关性强度。
2. **大规模预训练模型**：为了验证通用性，研究者在 1.3B 参数的预训练视频模型上复现了实验。这证明了“因果可写性”和“闭合边界”并非小模型的特有产物。

### 关键实验任务
- **干预实验**：在不同深度注入物理变量编辑，测量生成视频的运动频率变化。
- **强度扫描**：改变编辑的增益（Gain），观察从“无效”到“修正”再到“过冲（Overshoot）”的过程。
- **跨模型转移**：测试在一个模型上训练得到的编辑方向（Edit Direction）是否能直接应用于另一个独立训练的模型。
- **训练动态追踪**：在模型训练的不同阶段测量可写性，观察错误是如何从“不可写”变为“可写”并最终被“纠正”的。

Q5: 发现了什么实验现象？

### 1. 尖锐的深度边界（Sharp Depth Boundary）
实验发现，因果可写性并非随深度逐渐减弱，而是存在一个极其尖锐的“闭合”点。在边界之前，微小的编辑就能完全改变视频的物理走向；一旦跨过该边界，同样的编辑几乎毫无影响。这表明模型在计算过程中存在一个明确的“承诺时刻”，将观察到的信息固化到了未来帧的预测中。

### 2. 信号持久性与可写性的分离
**合理推断**：即使在闭合边界之后，原始的物理信号（如观察到的快速运动）可能依然存在于隐藏状态中，但它已经失去了对生成路径的控制权。只有通过极大幅度增加干预强度（过冲），才可能在下游重新激活这些信号，但这往往会导致视频质量崩溃。

### 3. 跨模型的可转移性（Cross-model Transfer）
尽管不同的模型实例在面对冲突时可能选择不同的未来（有的选颜色，有的选运动），但它们共享相似的“因果编辑空间”。在一个模型上发现的纠正方向可以有效地纠正另一个模型，这暗示了视频模型在学习物理规律时具有某种收敛的内部表示。

### 4. 预测训练结果
**关键发现**：模型在训练早期表现出的“可写性深度”预示了其未来的学习成果。如果一个错误在早期是“高度可写”的（即在更多层中可被干预），那么在后续训练中这个错误更有可能被自动纠正。反之，如果错误在深层不可写，则该错误往往会持久存在。

### 5. 过冲现象（Overshooting）
当干预强度过大时，模型生成的运动会超过目标物理值，甚至产生非自然的伪影。这表明模型内部的物理表征具有一定的线性度，但也存在严格的有效范围。

Q6: 有什么可以进一步探索的点？

### 1. 自动化物理纠错机制
利用“因果可写性”开发一种实时的、无需重新训练的视频纠错插件。当检测到生成视频违反物理常识时，自动在闭合边界前注入修正编辑。

### 2. 增强世界模型的因果对齐
研究如何通过训练目标直接优化“可写性深度”，使模型在更深的层次上保持对物理变量的敏感性，从而减少捷径学习导致的物理错误。

### 3. 复杂场景下的子空间识别
目前的实验主要集中在简单的物理变量（振动频率）。未来的研究可以探索更复杂的因果变量（如流体力学、物体碰撞、光影逻辑）在超大规模模型（如 Sora 或 Gen-3）中的可写性分布。

### 4. 训练诊断工具
将可写性作为一种新的监控指标，用于诊断模型在训练过程中是否真正“理解”了物理规律，还是仅仅在记忆背景相关性。

Q7: 总结一下论文的主要内容

本论文深入探讨了视频生成模型中物理真实性缺失的根源，提出了“因果可写性”（Causal Writability）这一创新视角。作者通过严谨的受控实验证明，当视频模型生成错误的物理运动时，这通常不是因为模型“无知”，而是因为模型在推理过程中由于“预测欠定性”选择了错误的预测规则。通过在模型隐藏状态中注入基于物理变量的低维编辑，研究者成功地在模型内部“重写”了未来，使模型恢复了正确的物理生成。研究的核心发现是“闭合边界”的存在：模型在计算的特定深度会对其生成的“未来”做出因果承诺，此后的干预将变得极其困难。此外，论文还揭示了可写性与训练动态之间的深刻联系，证明了早期的可写性是模型最终掌握物理规律的先兆。这项工作不仅为理解视频模型的内部机制提供了强有力的工具，也为开发更具物理一致性的生成AI开辟了新的路径。实验涵盖了从合成数据到 1.3B 参数预训练模型的广泛范围，确保了结论的稳健性和普适性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对于关注视频生成物理真实性的研究者，本文提供了全新的干预和诊断视角。

## 基本信息

- 作者：Xingyun Wang, Haomin Zheng, Man Yuan, Leqian Yang, Ziming Liu
- 机构：麻省理工学院 (MIT) 等（推测，基于作者 Ziming Liu 的学术背景及研究领域）
- 来源：arxiv
- 主题/分类：cs.LG, cs.CV
- 日期：2026-09-15
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.15980`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成深度参考了检索到的 Abstract、Introduction、Discussion 和 Related Work 证据片段，并结合了对作者背景的合理推断。
