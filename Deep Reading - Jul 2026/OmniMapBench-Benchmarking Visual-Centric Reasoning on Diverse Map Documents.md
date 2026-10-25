---
user_id: "cheng tan"
arxiv_id: "2607.09068"
title: "OmniMapBench: Benchmarking Visual-Centric Reasoning on Diverse Map Documents"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Jul 2026/2607.09068.pdf"
pdf_url: "https://arxiv.org/pdf/2607.09068"
abs_url: "https://arxiv.org/abs/2607.09068"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-10-12T17:45:49"
---
# OmniMapBench: Benchmarking Visual-Centric Reasoning on Diverse Map Documents

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：visual-centric reasoning · map document understanding benchmark · visual dependency index · large vision-language model evaluation

## 一句话总结

OmniMapBench 是一个专为地图文档设计的视觉中心推理基准，包含 2,096 个手工标注问答对和 9 类地图文档，并提出 Visual Dependency Index (VDI) 来量化基准对不可约视觉信息的依赖程度；在 25 个主流 LVLM 上的评测显示最佳模型准确率仅为 75.03%，说明现有模型在真正的视觉接地推理上仍有显著差距。

## 摘要

> Recent advancements in LVLMs necessitate robust benchmarks for complex, visually grounded reasoning. A critical limitation is identified in many document understanding benchmarks: visual content is often reducible to text, enabling high performance without genuine visual grounding. To address this limitation, OmniMapBench is introduced to foster visual-centric reasoning for map documents. The benchmark comprises 2,096 manually annotated question-answer pairs across 1,603 map documents from nine categories. It is designed to probe a hierarchy of skills, ranging from perception to multi-step visual reasoning. To quantify benchmark properties, a simple yet effective benchmark-level metric is proposed: the Visual Dependency Index (VDI), defined as the accuracy drop when images are replaced with question-agnostic descriptions. OmniMapBench exhibits higher VDI than established benchmarks, which quantitatively validates its focus on irreducible visual reasoning. Comprehensive evaluations of 25 leading LVLMs are conducted on OmniMapBench. A significant performance gap is observed, with the top-performing model achieving only 75.03\% accuracy. This result underscores the challenges posed by OmniMapBench to current LVLMs. This work aims to catalyze progress in visual-centric reasoning for document understanding of LVLMs. The dataset and code are publicly available at https://github.com/SIGMME/OmniMapBench.

Q1: 这篇论文试图解决什么问题？

1. 核心问题：当前 LVLM 发展迅速，但文档理解基准存在系统性缺陷——大量基准中的视觉内容可以被文本转录或描述近似替代，模型依靠 OCR、文本线索或语言先验即可回答，无需真正理解图像布局、空间关系和视觉符号，导致分数虚高，无法反映真实视觉推理能力。2. 缺乏衡量‘视觉不可约性’的量化手段：现有 benchmark 没有统一的指标来回答‘这个任务到底在多大程度上必须看图像才能解决’，导致研究者难以甄别哪些基准真正考验视觉推理。3. 地图文档这一类别被忽视：地图具有高度视觉化的空间符号系统（拓扑关系、路线、图例、方向、比例），既不同于自然图像也不同于普通文档版面，且包含从感知到多步推理的丰富任务层级，是检验视觉中心推理的理想试金石，但缺少专门的高质量基准。4. 评测协议问题：要科学评估一个基准是否‘视觉中心’，需要考虑描述长度（token budget）的影响——描述越长包含信息越多，模型可能仅凭文字作答；现有工作没有系统地在不同描述预算下测量视觉依赖的衰减曲线。5. 现有 LVLM 在地图这类视觉密集型文档上的真实能力底数未知：缺乏系统性、跨模型、跨类别的评测结果，无法判断当前模型在感知、单步空间推理、多步推理各层级上的瓶颈位置。

Q2: 有哪些相关研究？

1. 文档理解基准：DocVQA 是代表性文档问答基准，但论文指出其许多问题只需阅读表格文字或版面文本即可回答，视觉依赖度低（示例中表格类和情感类问题几乎不依赖图像结构）。2. 通用多模态基准：MMBench 等全面评测多模态模型各方面能力的基准也被纳入对比，但 VDI 分析显示其在长描述下视觉依赖趋近于零（kmax=1024 时 VDI≈0.028），说明其大量题目可被文本替代。3. 图表/科学图解理解基准：AI2D 针对科学图表，同样在长描述下 VDI 接近零（0.019），表明这类基准存在类似的文本可约性。4. 空间推理与视觉推理研究：论文引用 ‘think with images’ 系列方法，这些方法训练模型分解复杂视觉查询并逐步推理，是 OmniMapBench 所要推动的能力方向（对应引言中 [52,64,65,90] 等工作）。5. 地图理解相关工作：基准覆盖从基础地形分析到虚构虚拟世界和遥感影像的多种地图风格，隐含与 remote sensing 视觉理解、GIS 场景理解的连接。6. 视觉依赖评估方法：论文提出的 VDI 属于 benchmark-level 的‘文本可约性’度量，与该思路相关但更简单直接，用‘问题无关描述替换图像 + 准确率下降’来代理不可约视觉信息量。7. LVLM 评测与能力分层：OmniMapBench 的问题设计分层为感知、单步空间推理、多步视觉推理，这与能力分层的评测哲学（如从 perception 到 reasoning 的层级）一致。

Q3: 论文如何解决这个问题？

1. 数据构建：构建包含 9 类地图文档的语料库，涵盖基础地形图、虚构虚拟世界地图、遥感影像等多种视觉风格；收集原始地图文档后由标注者生成问答对，初始候选集合为 2,882 对。2. 质量控制：对 2,882 个候选 QA 对进行严格交叉验证，由两个独立团队分别验证，只有通过全部验证的题目才进入最终集合，最终保留 2,096 个 QA 对，覆盖 1,603 张地图文档。3. 技能分层设计：问题按能力层级组织，从基础感知（如识别地标符号）到单步空间推理（如判断路线/方位），再到多步视觉推理（如结合多个视觉线索推导结论），以探测模型的能力边界。4. VDI 指标：提出 Visual Dependency Index，定义为用问题无关的图像描述替换真实图像后模型准确率的下降幅度，数值在 0 到 1 之间；接近 0 表示任务几乎可纯文本解决，接近 1 表示强烈依赖视觉输入。5. VDI 计算方法：给定 token budget kmax，用 VLM 为每张图生成最多 kmax 词的描述，然后使用固定 prompt（‘请仅根据以下图像描述回答问题’）让模型仅依据描述作答，计算准确率下降值；在不同 kmax（如 128、256、512、1024）下测量，得到视觉依赖随描述预算增大的衰减曲线。6. 参考模型消融：在 VDI 分析中引入参考模型的选择作为消融变量，检验不同描述生成器对 VDI 数值的影响，增强指标稳健性。7. 评测协议：在 25 个领先 LVLM（开源与闭源）上统一评测，同时报告 VDI 与 DSI（推测为描述充分性相关指标，原文证据不足）两个维度，并用 Figure 4、Table 2、Figure 7 呈现总体与分地图类别的结果。

Q4: 论文做了哪些实验？

1. 主评测：在 OmniMapBench 的 2,096 个 QA 对上评测 25 个主流 LVLM（含 Gemini-3.1-Pro、Qwen3.5 系列等开源/闭源模型），报告总体准确率与模型间差距。2. VDI 对比实验：在 OmniMapBench、AI2D、MMBench、DocVQA 等多个基准上计算 VDI，并在不同 token budget（kmax 从较小值到 1024）下绘制衰减曲线，比较各基准对文本替代的抵抗能力。3. 类别细分分析：按 9 种地图类型给出类别层面的性能 breakdown（Figure 7），观察模型在不同地图风格上的表现差异。4. 技能层级分析：按感知、单步空间推理、多步视觉推理等能力层级拆分结果（对应 Figure 6/表 2 所在章节的 ‘different levels’ 分析）。5. 参考模型消融：更换用于生成图像描述的参考 VLM，检验 VDI 指标对描述生成器的敏感性，评估指标稳健性。6. DSI 指标报告：论文同时报告 DSI（从检索片段看与描述相关，推测可能是 description sufficiency 一类的指标），用于交叉验证高 VDI 结论。

Q5: 发现了什么实验现象？

1. 最佳成绩有限：表现最好的 Gemini-3.1-Pro 仅达到 75.03% 准确率，25 个模型整体存在显著性能差距，说明 OmniMapBench 对当前 LVLM 构成真实挑战。2. 开源模型反超闭源：Qwen3.5 系列等领先开源模型显著超过列表中的多数闭源模型，挑战了‘闭源必然最强’的惯性认知（此点在 heuristic_draft 中明确，检索片段仅部分支持，需结合实际结果解读）。3. VDI 衰减更慢：OmniMapBench 的 VDI 随 token budget 增大而下降的速度明显慢于其他基准；在 kmax=1024 时 VDI 仍高达 0.338，为所有基准中最高；作为对比，AI2D 和 MMBench 在最大预算下 VDI 分别跌至 0.019 和 0.028，几乎降为零。4. 高预算下仍保持高视觉依赖：检索片段提到 OmniMapBench 在较大预算下饱和度保持在 0.629（kmax=1024，第二高），说明即使给足文字描述，模型仍需依赖视觉信息才能答好。5. 高 DSI 与 VDI 相互印证：较高的 DSI 与 VDI 共同验证了 OmniMapBench 抵抗文本还原、需要真正视觉中心推理的结论。6. 视觉可约性差异：DocVQA 中的表格/情感类问题极易被文字描述替代，而 OmniMapBench 的路线阻断、地标符号识别、方位判断等问题无法用纯文本描述等价还原，直观展示了‘视觉中心’与‘文本可约’任务的区别。

Q6: 有什么可以进一步探索的点？

1. 扩展地图类型与任务难度：当前 9 类地图可继续扩展至更多专业地图（地质图、海图、气象图、室内导航图等），并增加更复杂的多步推理、反事实推理和规划类任务。2. 引入时序与交互式地图理解：现有基准是静态问答，可探索动态地图、多时相遥感变化检测、交互式导航指令跟随等任务形态。3. 更细粒度的能力诊断：将 VDI 推广到问题级/类别级，识别模型在感知、空间关系、多步推理各环节的具体失败模式，并构建错误类型学。4. VDI 的推广与理论化：研究 VDI 对参考模型、描述 prompt、token 预算的敏感性，探索更理论化的‘视觉不可约性’度量（如信息论视角），并推广到非地图文档。5. 训练方法与模型改进：基于 OmniMapBench 开发视觉中心推理的训练策略，如 ‘think with images’ 类方法、显式空间推理模块、地图符号 grounding 训练，推动感知-推理一体化。6. 评测协议标准化：将 VDI 纳入文档理解基准的常规验收指标，推动社区建立‘新基准必须报告文本可约性’的规范，抑制刷分型基准。7. 跨模态鲁棒性研究：考察模型在风格化地图、虚构地图、遥感影像与真实地图之间的泛化差异，研究分布偏移下的视觉推理退化规律。8. 与智能体任务结合：地图问答可扩展为具身/智能体场景中的视觉导航、空间规划、路径决策任务，将静态问答升级为动态决策评估。

Q7: 总结一下论文的主要内容

论文围绕‘视觉中心推理（visual-centric reasoning）’这一核心命题展开，针对现有文档理解基准的普遍缺陷——视觉内容可被文本还原、模型不真正看图像也能高分——提出 OmniMapBench 基准和 VDI 指标。论证主线如下：第一，指出问题：许多 benchmark 的题目（如 DocVQA 中的表格阅读、情感判断）本质上依赖文本内容而非图像结构，导致 LVLM 的分数不能反映真实视觉理解能力。第二，选择地图作为测试域：地图是视觉信息密度极高且不可文本化的文档类型，包含拓扑关系、符号系统、路线、方位、尺度等真正需要图像才能回答的信息；基准覆盖 9 类共 1,603 张地图文档，从基础地形图、虚构虚拟世界到遥感影像。第三，质量控制：初始 2,882 个候选 QA 对经两个独立团队的严格交叉验证，最终保留 2,096 对，确保正确性和质量。第四，能力分层：题目按感知、单步空间推理、多步视觉推理分层，形成从低到高的能力探测谱系。第五，方法贡献：提出 VDI——用问题无关描述替换图像后准确率的下降幅度——作为 benchmark 层面的文本可约性度量，并设计了带 token budget 的系统评测协议（Algorithm 1），让模型在不同描述长度下作答，绘制视觉依赖衰减曲线。第六，实证发现：OmniMapBench 的 VDI 衰减显著慢于 AI2D、MMBench 等基准，在 kmax=1024 时仍达 0.338（所有基准最高），DSI 也验证了高视觉依赖；25 个 LVLM 的最佳成绩仅 75.03%（Gemini-3.1-Pro），开源 Qwen3.5 系列表现超过多数闭源模型，显示当前模型在视觉中心推理上仍有明显短板。第七，意义与展望：数据集、指标与协议共同构成评估地图文档视觉中心推理的原则性框架，旨在推动 LVLM 从‘用文本线索答题’走向‘真正感知-推理整合’。整体而言，这是一篇以基准建设+元评测指标为核心的系统性工作，兼具数据集贡献（2,096 QA / 1,603 图 / 9 类）、方法贡献（VDI）与实证贡献（25 模型全面评测）。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：对做 LVLM 评测与基准建设的研究者：本文的 VDI 指标是可直接复用的 benchmark 级‘文本可约性’度量，适合作为新基准的验收工具。

## 基本信息

- 作者：Yang Chen, Yunwen Li, Yufan Shen, Minghao Liu, Tianyu Zheng, Bin Fu, Qunshu Lin, Zhi Yu, Botian Shi
- 机构：未提供
- 来源：未知
- 主题/分类：未知
- 日期：2026-07-13
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.09068`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Introduction/Discussion/Conclusion 证据片段与 heuristic_draft，并优先采用检索证据；由于 Method 与 Results 章节证据覆盖有限，部分细节已标注为合理推断或需回原文核对。
