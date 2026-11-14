---
user_id: "cheng tan"
paper_id: 21465
arxiv_id: "2611.15844v1"
title: "Open Vision Banana: A Fully Open Recipe for Generative Visual Perception"
publish_date: "2026-11-11"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.15844v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.15844v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-14T17:41:13"
---
# Open Vision Banana: A Fully Open Recipe for Generative Visual Perception

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：generative visual perception · image editing · vision foundation models · semantic segmentation

## 一句话总结

Open Vision Banana 提出一套完全开放、可复现的配方，将 4B/9B 预训练图像生成器转化为统一的多任务视觉感知系统，并用颜色图例集成这一测试时扩展方法显著提升分割表现。

## 摘要

> Recent work suggests that pretrained image generators encode real-world understanding beyond image synthesis. Vision Banana exploits this by framing vision tasks as conditional image editing. However, its data, weights, code, and implementation details are not public. We present Open Vision Banana (OVB), a fully open and reproducible recipe for converting pretrained generators into unified multi-task perception systems. Our analyses identify the design choices that matter most for generalization, including color encoding, prompt design, parameter adaptation, and task balancing. These choices interact, so individually helpful choices can hurt when combined with others. Starting from 4B and 9B parameter open-weight base models and using 2.1M-3.8M training pairs from public datasets, OVB surpasses Vision Banana's reported Cityscapes zero-shot performance (73.9 vs. 69.9 mIoU) and rivals state-of-the-art affine-invariant depth estimators. Its non-zero-shot variants come within five points of Vision Banana on referring and reasoning segmentation but remain further behind on instance segmentation. All models largely retain the base model's image-editing abilities. With legend ensembling, we introduce a test-time scaling method for generator-based perception. It segments an image under several class-color legends and fuses the results by majority vote, correcting unstable color-class bindings and improving mIoU by up to 8.7 points on Cityscapes and 15.0 points on COCO-Object without retraining. We release the dataset construction and processing pipeline, the training and evaluation code, as well as the final, intermediate, and ablation checkpoints. Project website: https://openvisionbanana.github.io

Q1: 这篇论文试图解决什么问题？

【核心问题】
这篇论文试图解决的问题可以分成三层。第一层是范式验证：近期工作提出，预训练图像生成器在合成能力之外还编码了语义、几何和世界知识，因此可以把分割、深度、法向等感知任务统一改写为“条件图像编辑”——输入图像加任务提示，输出一张经过编辑的图像，再从图像中读回预测。Vision Banana 已展示这条路线可行，但其数据、权重、代码和实现细节不公开，导致无法判断哪些设计真正关键、结果是否可复现、误差来自范式还是工程细节。

【配方缺失】
第二层是配方缺失：把生成器改造成多任务感知系统涉及颜色编码、提示模板、参数适配、任务混合比例、是否保留生成数据等一系列选择；论文明确指出这些选择彼此交互，单独看有效的改动组合后可能有害，因此需要一个系统的、可复现的实验分析来给出稳定配方。换句话说，论文不是只问“生成器能不能做感知”，而是问“在开放资源下，怎样组合这些设计才真正泛化”。

【输出接口稳定性】
第三层是输出接口的稳定性：生成式感知用颜色或灰度图作为符号接口，模型可能把类别绑定到错误颜色，或在图例变化时输出不一致；论文试图用 legend ensembling 这种测试时扩展方法纠正“颜色—类别绑定”不稳定，从而在不重训练的情况下提升精度。

【现有缺口】
从摘要与片段看，现有缺口包括：Vision Banana 闭源，开放生成器能否达到同等泛化未知；任务专用判别模型很强但碎片化，难以用一个模型统一覆盖；生成式通用模型在实例分割等细粒度任务上是否可靠未知；测试时扩展在生成式感知中如何定义、收益多大、成本多高，也缺少系统研究。

【隐含假设】
论文的路线依赖若干假设：图像生成器编码了足够好的视觉表示；条件图像编辑可以承载稠密预测；颜色图例是足够表达力的输出接口；公开数据混合可以近似闭源数据；基座的编辑能力与感知能力可以共存。这些假设在语义分割和深度上可能成立，但在实例分割、开放词汇、细粒度类别和分布外场景中可能失效。

【边界与风险】
颜色数量限制类别数；实例分割需要区分同类实例，颜色接口更困难；多数投票依赖图例置换后预测可对齐；推理成本随集成次数线性增长；论文比较的是报告数字而非受控同算力对比。这些边界决定了 OVB 更像一个可复现的强基线，而不是全面替代任务专用模型的通用解。

Q2: 有哪些相关研究？

【生成器作为视觉学习器】
从摘要与检索片段可确认，论文的相关工作至少覆盖以下几条线。第一，生成器作为视觉学习器：RINO、DICEPTION、ZS Vision Learners 等工作探索冻结或预训练生成模型的特征用于感知；Marigold、Lotus 等把生成先验用于稠密几何预测。这条线的核心问题是：生成式预训练到底学到了什么，能否在没有任务专用头的情况下做稠密预测。

【生成式通用感知】
第二，生成式通用感知：Vision Banana 把视觉任务写成条件图像编辑，SenseNova-Vision 也被列为生成器基通用器；这些系统通常闭源或数据/训练细节不完整。OVB 的定位不是另起炉灶，而是把这条路线开放化、可复现化，并补齐设计选择分析。

【任务专用专家】
第三，任务专用专家：语义分割、指代/推理分割、实例分割、深度、法向各有强 baseline；论文表格中的 Best specialist 即此类。OVB 与这些专家的比较揭示了统一生成式模型的优势（任务接口统一、可利用生成先验）与短板（实例分割、细粒度边界）。

【统一多任务与图像编辑】
第四，统一多任务模型与指令编辑：用统一提示处理多任务，同时保留基座图像编辑能力。OVB 的“所有模型基本保留编辑能力”这一结果，直接关联到多任务训练是否会造成生成先验的灾难性遗忘。

【测试时扩展】
第五，测试时扩展：legend ensembling 与 LLM 的自洽性/多数投票同源，但在生成式感知中以“类别—颜色图例”置换为扰动轴。相关研究还包括 prompt ensemble、seed ensemble、test-time augmentation 等；OVB 的贡献是把这些思想落到颜色—类别绑定这一具体失败模式上。

【开放复现】
第六，开放复现：释放数据流水线、代码与检查点。由于提供的是片段而非完整 Related Work，具体引用范围和公平比较需回原文核对；上述归类是基于摘要、片段和该领域常见脉络的合理整理。

Q3: 论文如何解决这个问题？

【总体框架】
OVB 的总体思路是保留预训练图像生成/编辑模型作为骨干，把每个感知任务都变成“给输入图像加提示，让模型输出一张编辑后的图像”的条件生成问题。模型不新增任务专用头，而是复用生成器的图像到图像能力，把预测结果编码在输出图像的像素颜色或灰度中。

【输入与提示】
具体来说：输入由原图、任务说明和类别—颜色图例组成；提示模板从约十种等概率采样的模板中抽取，任务标签如 SEMANTIC_SEGMENTATION 被显式写入，图例把每个类别映射到一个颜色。片段中的提示形式包括“按语义类别给输入图像上色”“只画列出的类别”等；这种模板随机化旨在避免模型对固定措辞过拟合。

【颜色编码与读回】
语义分割时，模型被要求按图例给像素上色，输出图像中像素颜色即预测类别；深度任务要求输出场景保持的灰度反深度图；法向任务输出法向图。读回阶段把颜色映射回类别、把灰度映射回深度/法向。颜色编码因此不是小事：颜色空间、类别—颜色距离、图例长度和读回规则都会直接影响精度。

【训练配方】
训练时使用公开数据集构建 2.1M–3.8M 图像—标注对，混合多个感知任务，并可能混入基座的生成/编辑数据以保留编辑能力。论文系统比较颜色编码、提示设计、参数适配（如冻结嵌入层 vs 全 transformer 微调）和任务平衡；关键发现是这些选择存在交互，不能把单因素消融的最优解直接相加。

【Legend ensembling】
推理时提出 legend ensembling：对同一图像用多个不同的类别—颜色图例分别分割，把每个输出按图例解码回类别，再逐像素多数投票；这相当于用测试时计算纠正颜色—类别绑定的不稳定性。片段显示 Cityscapes 使用 20 次 pass、SA-Co/Gold 使用 11 次 pass，具体次数和融合规则需回原文核对。

【开放释放】
论文释放数据构建与处理流水线、训练/评测代码，以及最终、中间和消融检查点。这让 OVB 不只是“另一个模型”，而是一个可复现的实验平台；但基座模型的具体名称、损失函数、优化器与训练超参在提供材料中不完整，需回原文确认。

Q4: 论文做了哪些实验？

【模型与数据】
论文的实验可以按“模型/数据—任务/指标—对比—消融—测试时扩展”来读。模型与数据方面，从 4B 和 9B 两个开放权重基座出发，使用 2.1M–3.8M 公开数据训练对；不同模型/配置的数据量不同。

【任务与基准】
任务与基准方面，语义分割包括 Cityscapes 零样本与 COCO-Object-80；还有指代与推理分割、实例分割（SA-Co/Gold 等）、仿射不变深度、法向，以及基座图像编辑能力的保留。指标包括 mIoU、cIoU、gIoU、cgF1、δ1、角度误差等；表格片段中这些指标混排，具体对应需回原文。

【主要对比】
主要对比对象包括 Vision Banana、开放生成器通用器（ZS Vision Learners、RINO-Zero、DICEPTION、SenseNova-Vision 等）以及任务专用专家/深度估计器。论文既报告零样本结果，也报告非零样本变体，以区分“泛化能力”和“目标数据适配后的上限”。

【关键结果】
关键结果：OVB 9B 零样本 Cityscapes 73.9 mIoU，超过 Vision Banana 的 69.9；OVB 4B 为 71.3；非零样本配方分别再提升 5.1 和 1.8 个点，指代/推理分割距 Vision Banana 不足 5 个点，实例分割仍明显落后；深度可媲美先进仿射不变估计器；图像编辑能力基本保留。

【消融与测试时扩展】
消融覆盖颜色编码、提示模板、参数适配、任务平衡、生成数据比例、图例剪枝等；片段显示生成数据比例对法向和深度的影响非单调，移除生成数据在法向上误差最低但深度变差；仅冻结嵌入层时精度最高（需核对）。测试时扩展方面，legend ensembling 在 Cityscapes 提升至多 8.7 mIoU、COCO-Object 提升至多 15.0 mIoU，无需重训练；代价是多次前向。

【证据边界】
需要强调，提供的是摘要和检索片段，不是完整实验章节；表格行列错位明显，部分数值不能可靠归因。上述实验清单是阅读框架，具体数据集版本、训练轮数、硬件、基座模型和公平性设置需回原文核对。

Q5: 发现了什么实验现象？

【设计选择的交互】
最重要的现象是“设计选择交互”：颜色编码、提示模板、参数适配和任务平衡并非独立增益，单独有效的选择组合后可能互相抵消。这提示单因素消融会高估某些技巧，也说明生成式感知的配方需要联合搜索/因子实验，而不是把已有 trick 堆起来。

【颜色—类别绑定的不稳定性】
模型有时不按图例颜色输出，或在图例置换后改变预测；legend ensembling 能纠正这一点，说明瓶颈部分在符号绑定而非视觉特征本身。这是一个值得注意的失败模式：生成器可能“看见了”物体，但没有稳定地把类别与提示中的颜色对应起来。

【测试时扩展的收益与成本】
多次图例分割加多数投票在 Cityscapes 上最高 +8.7 mIoU、COCO-Object 上 +15.0 mIoU；COCO 类别更多、绑定更容易出错，收益更大（合理推断）。片段显示 Cityscapes 用 20 passes、SA-Co/Gold 用 11 passes，说明 pass 数可按基准调整；但推理成本随 pass 数线性增长，论文未给出实时性分析。

【生成数据比例的非单调性】
移除生成数据在法向估计上误差最低，但深度变差；增加比例又带来其他权衡。这揭示了“保留编辑能力”和“感知专精”之间的张力，不能用单一比例通吃；也说明生成数据不仅是正则项，还可能改变不同几何任务之间的相对偏好。

【参数适配】
片段暗示仅冻结嵌入层时精度最高，全 transformer 微调的对照也在表中；这说明适配容量和灾难性遗忘之间存在平衡。若该结论成立，实际部署可以选择更轻量的适配方式，但具体配置和数值需回表核对。

【尺度与零样本/非零样本】
表格片段中 4B 零样本 71.3、9B 73.9，非零样本 4B 76.4、9B 75.7；若数值对应无误，小模型在非零样本下反超，说明数据配方/任务平衡可能比参数量更关键。这是一个非单调 scaling 现象，值得进一步验证。

【指标张力与证据噪声】
深度 δ1 可很高（97–99）但平均误差仍有 13–20，阈值指标饱和会掩盖稠密误差；法向角度误差与深度指标也未必同向。检索片段来自表格，行列错位明显，部分数字不能可靠归因；应把上述现象当作阅读线索而非最终结论。

Q6: 有什么可以进一步探索的点？

【任务扩展】
第一，把颜色图例接口推广到实例/全景/视频/3D：实例分割需要区分同类实例，单纯类别颜色不够，可探索实例 ID 颜色、边界图、分层图例、视频时序一致性。第二，开放词汇与细粒度类别：研究图例长度、颜色距离、类别层次结构对绑定稳定性的影响。

【读回与不确定性】
第三，改进读回与不确定性：学习式颜色解码器、概率颜色映射、校准、失败检测；当前颜色匹配是手工的，量化误差和颜色混淆会直接变成标签错误。第四，研究颜色—类别绑定机制：分析注意力是否真正读取图例、是否存在颜色先验捷径、图例长度/顺序/颜色距离的影响；这对可解释性和鲁棒性都关键。

【测试时扩展系统化】
第五，测试时扩展的系统化：除图例置换外，联合提示模板、随机种子、任务分解、自洽性、验证器、自适应 pass 数、早停和级联；研究收益随 pass 数的 scaling curve 与饱和点。第六，效率：把多次 legend ensemble 蒸馏成单次前向，或缓存/共享注意力；研究能否用少量 pass 达到大部分收益。

【数据与训练配方】
第七，数据混合与任务平衡：用因子实验或贝叶斯优化搜索感知数据、生成数据、任务比例；研究非单调现象的成因，提出动态加权/课程学习。第八，参数适配与生成能力保持：比较 LoRA、adapter、部分冻结、模型合并；目标是在不牺牲编辑能力的前提下最大化感知，避免灾难性遗忘。

【评测与开放复现】
第九，开放评测与复现：统一数据许可、评测协议、失败案例报告；与闭源 Vision Banana 做同算力/同数据受控比较。第十，跨领域迁移：遥感、医学影像、显微镜、科学成像中的类别颜色图例和稠密预测；但这些领域对颜色语义、类别不平衡和域偏移更敏感，需重新验证。

【与 agent 结合】
第十一，与 agent 结合：把生成式感知作为多模态 agent 的视觉工具，输出可读图像供后续推理/规划；研究工具调用成本、失败恢复和多步编辑。第十二，安全与可靠性：生成式输出可能 hallucinate，需研究对抗图例、分布外类别、置信度与弃权机制。

Q7: 总结一下论文的主要内容

【一句话定位】
Open Vision Banana 是一篇“开放配方+系统分析+测试时扩展”的工作，而不是单纯提出一个新模型。它的起点是近期关于生成式视觉感知的观察：预训练图像生成器在合成图像之外，似乎编码了可用于分割、深度、法向等任务的现实世界知识；Vision Banana 已经把视觉任务改写为条件图像编辑，并展示了可行性，但其数据、权重、代码和实现细节不公开。OVB 的目标是把这条路线变成可复现的公共基线：用 4B/9B 开放权重生成器、2.1M–3.8M 公开数据对，训练一个统一的多任务感知系统，并系统回答“哪些设计选择真正影响泛化”。

【方法主线】
方法上，OVB 把每个任务都变成“输入图像+任务提示→输出编辑图像”。语义分割时，提示包含类别—颜色图例，要求模型按类别给像素上色；深度时输出场景保持的灰度反深度图；法向时输出法向图。提示模板从十种模板中等概率采样，任务标签显式写入，图例把类别映射到颜色。训练配方涉及颜色编码、提示设计、参数适配、任务平衡以及是否混入生成数据。论文特别强调这些选择彼此交互：单独消融有效，不代表组合后仍然有效。推理阶段提出 legend ensembling：用多个不同的类别—颜色图例分别分割同一图像，解码后逐像素多数投票，以纠正不稳定的颜色—类别绑定；这是一种无需重训练的测试时扩展。

【实验主线】
实验上，OVB 在 Cityscapes 零样本语义分割达到 73.9 mIoU（9B）和 71.3 mIoU（4B），超过 Vision Banana 报告的 69.9；非零样本配方分别再提升 5.1 和 1.8 个点。它在指代/推理分割上距 Vision Banana 不足 5 个点，但实例分割仍明显落后；深度可媲美先进仿射不变估计器；图像编辑能力基本保留。消融显示生成数据比例对法向和深度的影响非单调，移除生成数据在法向上误差最低但深度变差；参数适配中仅冻结嵌入层可能最优（片段证据，需核对）。Legend ensembling 在 Cityscapes 上最高 +8.7 mIoU、COCO-Object 上 +15.0 mIoU，代价是多次前向；片段显示 Cityscapes 用 20 passes、SA-Co/Gold 用 11 passes。

【贡献与边界】
贡献可以概括为：第一，给出完全开放的数据构建、训练、评测与检查点，降低生成式感知的复现门槛；第二，系统分析颜色、提示、适配、任务平衡及其交互，给出可操作的配方经验；第三，用 legend ensembling 把测试时计算引入生成式感知，证明输出接口的不稳定性可以被推理时集成部分修正；第四，展示生成器基通用器在多个感知任务上的潜力与边界。边界也很清楚：实例分割仍是短板，非零样本并非全面 SOTA，颜色图例限制类别表达，集成增加推理成本，评测范围尚未覆盖视频/3D/医学/安全校准等场景；与闭源 Vision Banana 的比较是报告数字对比，而非同数据同算力受控实验。总体看，这篇论文的价值更在于“开放可复现的配方与失败模式分析”，而不是单一指标刷新。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：如果你关注 agent：OVB 把感知统一成“图像编辑”工具接口，可作为多模态 agent 的视觉前端；legend ensembling 也提供了测试时计算换精度的案例。

## 基本信息

- 作者：Arpit Jadon, Valentinos Pariza, Jona Ruthardt, Ryousuke Yamada, A. Sophia Koepke, Andrei Bursuc, Yuki M. Asano
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-11-11
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.15844v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要依据摘要和 PDF 语义检索命中的 Method/结果片段，已剔除明显表格噪声；对无法可靠对应的数值与配置均标注了需回原文核对。
