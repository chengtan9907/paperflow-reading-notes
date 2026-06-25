---
user_id: "cheng tan"
paper_id: 1160
arxiv_id: "2606.23543"
title: "VeriEvol: Scaling Multimodal Mathematical Reasoning via Verifiable Evol-Instruct"
institution: "Tsinghua University, Tencent"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23543.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23543"
abs_url: "https://arxiv.org/abs/2606.23543"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T00:38:49"
---
# VeriEvol: Scaling Multimodal Mathematical Reasoning via Verifiable Evol-Instruct

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：visual mathematical reasoning · reinforcement learning · data scaling · evol-instruct

## 一句话总结

VERIEVOL 将视觉数学推理的强化学习数据构建重新定义为可验证的缩放问题，通过解耦提示难度扩展（类型感知演化）与答案可靠性验证（假设检验反证），在保持现有 GRPO 配方不变的前提下实现了数据质量和数量的同步提升。

## 摘要

> Scaling reinforcement learning for visual mathematical reasoning requires more than generating harder questions: as data volume grows, the reward labels themselves must remain reliable. Yet existing data pipelines scale supervision while trusting the labeller, and policy-side methods assume the underlying answers are already correct. We instead treat scaling as a verifiable data-construction problem and decouple two axes before any policy update: prompt difficulty, expanded by route-specific evolution operators, and answer reliability, enforced by offline hypothesis–test falsification. We instantiate this as VERIEVOL, an iterative framework with two extensible components: a type-aware evolution module that rewrites low-difficulty image-question seeds into harder, image-grounded prompts; and HTV-AGENT, a verifier that accepts an answer only after multi-source counter-evidence has failed to refute it. The resulting verified data scales in volume, extends by adding evolution routes or verifier channels, and plugs directly into existing GRPO-style RL recipes. On a five-benchmark visual-math suite, scaling evolved SFT data from 10K to 250K samples raises the mean accuracy from 35.42 to 54.73; then, with backbone, SFT initialization, and GRPO recipe held fixed, VERIEVOL adds a cumulative +3.88 over an un-evolved RL baseline, of which +1.82 comes from evolved prompts and +2.06 from the HTV-AGENT verifier. We release the prompts, data, models, code, and the full verifier trace of every sample, so that downstream work can scale and audit the pipeline rather than only inspect its outputs.
> Correspondence: li-hl23@mails.tsinghua.edu.cn, {kevinezheng, leocaxu}@tencent.com, yang.yujiu@sz.tsinghua.edu.cn
> Website: https://robertmarton.github.io/verievol
> ![](images/f07daeb3d4babf75adcdb372e0b2e6eee6f54aa2b5cb5853a36618088ad5af64.jpg)
> ![](images/5a0002c46bbb45ce7c35c27c4af530e4750763fe7faddef753e092e84485ad33.jpg)
> Figure 1 External comparison (left) and data-volume scaling (right). VERIEVOL leads three of five benchmarks and the mean against the strongest external 7B baselines; evolved SFT data (10 → 250K) and verified RL data (10 → 130K) both scale monotonically. Details in §4.2–§4.3.

Q1: 这篇论文试图解决什么问题？

论文试图解决的核心问题是：在缩放视觉数学推理的强化学习训练数据时，如何同时保证提示难度的增长和答案标签的可靠性？现有数据管道在扩大监督规模时完全信任标注者，而策略侧方法默认底层答案已经正确——这两种做法在数据量增大时都会引入不可控的噪声。论文将问题重新定义为可验证的数据构建问题，强调必须将提示难度扩展与答案可靠性保证解耦，且验证过程必须在策略更新之前完成。

Q2: 有哪些相关研究？

相关研究主要包括：(1) 多模态数学推理中的 SFT 和 RL 方法，如基于 GRPO 的配方，但这些方法通常直接使用标注数据而不验证标签正确性；(2) Evol-Instruct 类方法，通过演化操作生成更难的指令，但缺乏对答案可靠性的校验；(3) 答案验证方法，如多数投票、自洽性检查，但通常依赖模型内部分布而非外部反证证据。VERIEVOL 的独特之处在于将演化与验证作为两条独立的数据构建轴，且验证采用多方反证（hypothesis-test falsification）而非单一置信度阈值。

Q3: 论文如何解决这个问题？

VERIEVOL 包含两个核心组件：(1) 类型感知演化模块（Type-aware Evolution Module），针对低难度图文种子，利用路线特定（route-specific）的演化算子（如替换数值、增加条件、要求多步推理等）生成更难的、仍以图像为基的提示，确保每次演化保持可解性和图像依赖；(2) HTV-AGENT 验证器（Hypothesis-Test Verification Agent），在给定提示后，先由多源模型（如不同 backbone 的 VLM）生成候选答案，然后主动收集反驳证据（如通过外部计算工具或图像重新理解），只有当所有来源的反证均失败时才接受该答案作为标准答案。整个流程在策略更新前离线完成，输出标准的提示-答案-奖励元组，可直接插入现有 GRPO 流程。框架支持通过增加演化路线或验证器通道进行扩展。

Q4: 论文做了哪些实验？

实验设计验证了以下方面：(1) 数据缩放属性：固定学生模型 Qwen2.5-VL-7B-Instruct，将 SFT 数据从 10K 逐步扩展到 250K，观察平均准确率从 35.42 提升至 54.73；(2) 消融研究：在 130K RL 数据上，比较 RL-Origin（基于种子数据）、RL-Evol（仅演化）、RL-Evol+HTV（完整 VERIEVOL），平均准确率分别为 55.24、57.06、59.12，验证了演化和验证各自贡献 +1.82 和 +2.06 个点；(3) 在五个视觉数学基准（包括 MathVision、MathVista、We-Math 等）上报告分项结果，VERIEVOL 在所有数据集上一致超越基线，尤其在 MathVision_MINI 上提升高达 +5.92；(4) 使用专有模型作为数据构建教师（Seed-2.0-Pro 用于 SFT 分支，Gemini-3-flash 用于 RL 分支），学生模型保持 7B 规模。

Q5: 发现了什么实验现象？

关键实验现象包括：(1) 演化单独提升（RL-Evol vs RL-Origin）在平均指标上为 +1.82，但分项上看，演化对一些需要多步推理的基准（如 MathVision_MINI +3.94）提升显著，而对另一些基准提升较小，说明演化路线对不同难度类型的覆盖可能不均衡；(2) HTV-AGENT 验证器在演化基础上进一步带来 +2.06 提升，且在所有基准上均为正增益，表明即使演化后的答案也可能存在系统性错误，验证器能有效过滤噪声；(3) 缩放效应明显，SFT 数据从 10K 到 250K 的曲线接近线性上升，且没有饱和迹象，提示数据规模仍有扩展空间；(4) 当验证器通道（如引入更多反证来源）增加时，可靠性进一步提升，且不同通道的贡献互补而非冗余。

Q6: 有什么可以进一步探索的点？

未来可探索的方向包括：(1) 添加更多演化路线（如涉及图表、几何构造、多模态对比）以覆盖更多题型；(2) 扩展 HTV-AGENT 的验证器通道，例如集成程序执行验证、图像重绘检查或外部知识库验证；(3) 将数据阶段验证与策略侧对齐（如 reward shaping）结合，探索两阶段协同优化；(4) 在更大尺度（如 7B→70B 学生模型、百万级数据）上验证框架的扩展性；(5) 将 VERIEVOL 迁移至其他多模态推理任务（如视觉问答、科学图表理解）；(6) 探索更高效的反证策略，避免大量调用专有模型带来的计算成本。

Q7: 总结一下论文的主要内容

VERIEVOL 提出了一个可验证的多模态数学推理数据构建框架，核心思想是将数据缩放问题解耦为提示难度扩展和答案可靠性验证两个独立阶段。类型感知演化模块通过路线特定算子提升提示难度，HTV-AGENT 验证器通过多方反证确保答案正确性。在 Qwen2.5-VL-7B-Instruct 学生模型上的实验表明，该框架在 130K RL 数据上比未演化基线提升 +3.88 个点，且 SFT 数据缩放从 10K 到 250K 带来近 20 个点的增益。论文强调可组合性：数据阶段验证与策略侧优化正交，可通过添加演化路线或验证器通道持续扩展。所有产出均已开源，便于后续研究复现和审计。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：论文聚焦多模态数学推理的 RL 数据缩放问题，与您画像中的 agent 方向（权重 0.10）有一定交叉，因其构建的验证数据可直接用于策略训练

## 基本信息

- 作者：Haoling Li, Kai Zheng, Jie Wu, Can Xu, Qingfeng Sun, Han Hu, Yujiu Yang
- 机构：Tsinghua University, Tencent
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.CV, cs.LG
- 日期：2026-06-23
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23543`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 PDF 检索证据中的摘要、结论和实验部分片段，核心事实与数值均来自检索匹配的区块。
