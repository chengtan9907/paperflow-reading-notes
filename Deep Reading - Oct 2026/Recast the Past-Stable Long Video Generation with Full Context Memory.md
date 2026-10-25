---
user_id: "cheng tan"
paper_id: 17022
arxiv_id: "2610.27986"
title: "Recast the Past: Stable Long Video Generation with Full Context Memory"
publish_date: "2026-10-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.27986.pdf"
pdf_url: "https://arxiv.org/pdf/2610.27986"
abs_url: "https://arxiv.org/abs/2610.27986"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-21T11:45:32"
---
# Recast the Past: Stable Long Video Generation with Full Context Memory

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：long video generation · autoregressive video model · video memory · diffusion transformer

## 一句话总结

本文提出 ReCam，通过将累积视频历史编码为高维记忆 token 并利用原生自注意力进行检索，配合双指针（dual-pointer）去噪策略，实现 30 秒以上稳定长视频生成，在 VBench-Long 上以 80.1 总分领先。

## 摘要

> [reference generation] ... The person is already present at the start, performs one simple upper-body
> action, then exits laterally while the fixed camera holds on the same unchanged environment ...
> retention analysis:
> (visible throughout [Shot 1]): fully preserved - preserve the referenced face, hairstyle,
> clothing, body proportions, and identity while allowing new motion.
> detailed description:
> [Shot 1] At 0.00 seconds, is already present and facing the camera. ...
> From 0.00 to 1.00 seconds, the person smiles naturally and gives one clear, decisive one-handed wave.
> From 1.00 to 1.50 seconds, the person immediately settles and turns naturally into a slight three-quarter
> view to begin leaving.
> From 1.50 to 5.00 seconds, the person takes decisive, continuous lateral steps toward screen right and
> leaves the frame ...
> By 5.00 seconds the person is completely out of frame; from 5.00 to 15.00 seconds the fixed camera holds on
> only the unchanged empty scene and background layout from .
> ...
> (b) Initially absent
> At the beginning of the target video, the visible environment, lighting, colors, and camera composition
> from are fully referenced; is outside the frame and the first frame is an empty
> scene.
> subject definitions:
> is a Black man with short hair in a white T-shirt and blue trousers, whose identity, face,
> hair, body proportions, and clothing come from . is the complete visual reference
> for and the scene. Preserve ...

Q1: 这篇论文试图解决什么问题？

1. 核心问题：自回归视频模型要维持长期记忆，但完整保留历史帧在 10 秒到 1 分钟的视频范围内计算和内存开销过大；现有压缩/检索方法会丢失历史证据，导致场景重访、离屏主体重现时出现不一致。
2. 两类现有设计维度：论文把已有方法归纳为“如何检索历史信息”与“如何保存历史信息”两个维度。选帧或稀疏化注意力虽然降低 conditioning 成本，但选择结果未必覆盖所需历史证据。
3. 已有方法的缺陷：场景重访或主体再次出现时，模型往往无法恢复之前看到的场景/主体信息；这说明局部生成可行但长期记忆不可靠。
4. 关键洞察：对历史的“粗读”（coarse read）不必然牺牲细粒度信息——可以把局部时空内容打包成较大的读取单元，同时保留全历史覆盖，让视频模型自己在高维 DiT 隐空间里决定如何编码和利用历史。
5. 自回归生成本身的误差传播：复用上一段生成的帧作为上下文时，早期生成错误会被逐步放大；需要一个针对 context 与 target 帧分别设计的噪声调度来缓解。

Q2: 有哪些相关研究？

1. 自回归视频生成与流式/交互式生成：论文引用了世界模型（Parker-Holder et al., 2025; Sun et al., 2025; Team et al., 2026; Wang et al., 2026c; Gao et al., 2026a）、具身智能（Ye et al., 2026; Kim et al., 2026; Li et al., 2026a）以及创意内容生成（Gao et al., 2025; Wan et al., 2025; Teng et al., 2025）等应用，说明这些场景把需求从局部短片段转向长时间记忆与状态维持。
2. 长视频生成的上下文压缩方法：已有方法通过选择历史帧或稀疏化注意力来降低 conditioning 开销（Yu et al., 2025; Cai et al., 2026; Jia et al., 2026），但检索/选择结果不一定覆盖全部历史证据。ReCam 与这些方法的关键区别在于：让模型通过原生自注意力自己学习历史的表示与检索，而不是预先设计选取规则。
3. Wan2.2-I2V-A14B：ReCam 从该 high-noise expert 模型初始化，属于对现有 I2V 扩散模型做长视频记忆增强的工作。
4. 评测体系：使用 VBench-Long I2V 套件（Huang et al., 2024）、R2M-Bench（Gu et al., 2026）以及 SpatialVID-HQ（Wang et al., 2026a），分别覆盖长时生成质量、静态场景重访和相机控制训练。
5. RoPE 变体：方法中使用了 PRoPE（Li et al., 2026b）做相机轨迹注入（fused-PRoPE），属于对位置编码扩展的利用。

Q3: 论文如何解决这个问题？

1. 整体框架：ReCam = 历史记忆 token 化 + 原生自注意力检索 + 双指针去噪。历史 latent 被转换为记忆 token，当前 chunk 通过自注意力同时接触历史与当前内容；文本交叉注意力只作用于当前 chunk。
2. 历史粗读（Recasting the history）：设历史 latent 帧为 F_H ∈ R^{C×T×H×W}，划分为局部时空块 {B_i}，每块 B_i ∈ R^{C×τ×h×w} 展平为向量 vec(B_i) ∈ R^{Cτhw}，由轻量 MLP f_φ 投影为记忆 token m_i = f_φ(vec(B_i))。这样显著减少历史 token 数，同时保留全历史覆盖；每个块在 DiT 高维隐空间中表示，模型可自行学习编码与利用局部细节。
3. 首帧锚点：第一帧保持为 clean visual anchor（干净视觉锚点），确保场景基线与身份信息的稳定性。
4. 双指针去噪：对 recent context（近期上下文帧）与 target frames（当前待生成帧）分别设置噪声调度。recent context 在一段调度中保持较高噪声水平，从而限制复用帧中错误的影响范围；当去噪接近结束时恢复 clean context。这样同时规避“noisy context 引入额外去噪状态”与“clean context 单指针误差传播”的问题。
5. 相机轨迹注入：在原始 DiT RoPE 之后、self-attention 之前应用 PRoPE，形成 fused-PRoPE，不增加可学习参数；训练与推理时只使用轨迹信息。
6. 训练策略：从 Wan2.2-I2V-A14B 的 high-noise expert 初始化，生成 480p 16fps 视频，每个自回归步添加 40 个像素帧；全局 batch size 64，三阶段各 10,000 步，最大历史长度分别为 10/20/30 秒；使用 AdamW（学习率 10^-5、10^-6、10^-6）。

Q4: 论文做了哪些实验？

1. 数据：内部 200 万条通用视频（5–60 秒，多数来自网络，覆盖多种真实场景），不使用人工构造的记忆标签（如 FOV overlap 重访片段）；静态场景重访额外在 SpatialVID-HQ 上训练以获得相机控制能力。
2. 长时生成评测：使用官方 VBench-Long I2V 套件，评测 30 秒 480p 视频，覆盖一致性、运动、视觉质量六个维度。
3. 静态场景重访评测：使用 R2M-Bench 的 100 个参考场景，测试相机返回时是否能恢复之前观察过的内容；用 MemoryGain（MG）和 Normalized Memory Ratio（NMR）校准重访一致性。
4. 离屏主体重现评测：评测协议覆盖主体再次出现时的一致性，报告四个一致性指标，比较首尾两个两秒窗口的变化（Δ = last − first，负值表示退化）。
5. 消融与趋势实验：历史长度 scaling（10/30/45/60 秒）、训练损失曲线对比（clean context、single-pointer denoising、dual-pointer denoising），以及 VBench 指标在首尾窗口的变化。
6. Baseline 对比：所有方法在相同条件下生成 30 秒 480p 视频，使用 VBench-Long 与重访/重现协议进行系统比较。

Q5: 发现了什么实验现象？

1. 总体结果：ReCam 在 VBench-Long 上取得最高总分 80.1，且离屏主体重现的四项一致性指标全部领先，说明历史记忆 token 化+自注意力检索确实改善了长期一致性。
2. 场景重访：在 R2M-Bench 上具有竞争力（但并非全面第一，论文表述为“保持竞争力”），说明静态场景重访仍是难点，且相机控制训练是必要补充。
3. 消融发现（合理推断，基于题目给出的片段文字）：历史长度从 30s 扩展到 60s 时仍有收益或至少不退化，说明 ReCam 的 token 压缩机制可以支撑更长历史；训练损失随步数下降并接近 clean-context loss。
4. 双指针 vs 单指针：dual-pointer denoising 与 clean context 的最终损失接近，且都低于 single-pointer denoising；在 VBench 首尾窗口变化上 dual-pointer 在四项指标上表现最好。这说明对 context 加噪的“单指针”方案会损害长期一致性，而双指针方案既不引入 noisy-context 的额外状态、又能抑制误差传播。
5. 反直觉点：把上下文加噪反而比不加噪（单指针 clean context）更差；而双指针把噪声主要调度在 recent context 上却能接近 clean context 的性能并超过单指针——噪声调度不是越干净越好，关键是错误传播的控制。

Q6: 有什么可以进一步探索的点？

1. 领域外记忆可靠性：论文明确承认 out-of-domain 设置下的可靠记忆使用仍是开放挑战；未来可探索更严苛的重访场景。
2. 更多下游应用：世界模型、具身智能、创意内容生成等场景的长时记忆需求不同，ReCam 的通用历史记忆机制可被进一步适配。
3. 从 in-context demonstrations 学习：论文提出未来工作探索从上下文演示中学习，可能走向少样本/指令化的长视频记忆控制。
4. 场景重访的进一步提升：当前 R2M-Bench 上只是“竞争力”而非绝对领先，可在记忆表示或相机轨迹建模上继续改进。
5. 更长的历史跨度与更高分辨率：当前训练历史上限为 30 秒，推理评测为 30 秒；可探索 1 分钟以上乃至无限流式视频的记忆。
6. 记忆 token 的可解释性与紧凑性：块大小 τ×h×w、MLP 容量、token 数等超参对记忆粒度与质量的影响值得系统研究；也可探索自适应分块或层级记忆。

Q7: 总结一下论文的主要内容

论文针对自回归视频模型的长时记忆问题提出 ReCam。背景是：世界模型、具身智能和创意生成等应用需要模型在数十秒到一分钟的生成过程中维持一致的状态，而完整保留历史帧在计算和内存上不可承受；已有方法通过选帧或稀疏注意力压缩历史，但可能丢失关键证据，导致场景重访与离屏主体重现不一致。ReCam 的出发点是让视频模型自己学习如何表示和检索历史：把累积的历史 latent 帧按局部时空块粗读，展平后经轻量 MLP 投影到 DiT 高维隐空间形成记忆 token，保留全历史覆盖的同时大幅减少 token 数；历史记忆与当前 chunk 通过原生自注意力连接，文本交叉注意力只用于当前 chunk；第一帧保持为干净视觉锚点。针对自回归 rollout 的误差传播，论文提出双指针去噪：recent context 与 target frames 使用不同噪声调度，对 recent context 保留较高噪声以限制复用帧错误的影响，去噪结束时恢复 clean context；消融显示 dual-pointer 在最终损失上接近 clean-context 且低于 single-pointer，VBench 首尾窗口变化上四项指标最优。此外通过 fused-PRoPE 在原始 RoPE 后注入相机轨迹，不增加可学习参数。训练从 Wan2.2-I2V-A14B 的 high-noise expert 初始化，在内部 200 万条 5–60 秒通用视频上三阶段训练（最大历史 10/20/30 秒，每阶段 10,000 步），静态场景重访额外用 SpatialVID-HQ 训练相机控制；评测使用 VBench-Long（30 秒 480p，六维度）、R2M-Bench（100 场景，MemoryGain/NMR）与离屏主体重现协议。结果显示 ReCam 在 VBench-Long 上达到 80.1 的最高分，离屏主体重现四项一致性指标全面领先，场景重访保持竞争力。局限是领域外记忆可靠性仍待解决；未来方向包括更严苛重访场景、更多下游应用和从上下文演示中学习。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的生成方向（权重 0.10）直接相关：长视频生成、扩散 Transformer、自回归 rollout。

## 基本信息

- 作者：Yukuo Ma, Cong Liu, Junke Wang, Junqi Liu, Haibin Huang, Zuxuan Wu, Xuelong Li
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-10-21
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.27986`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（introduction/method/results/conclusion 片段）与 field_evidence_map，并结合 heuristic_draft 做了补全和校准；对无直接证据的细节（如历史长度 scaling 的具体档位）做了合理推断标注。
