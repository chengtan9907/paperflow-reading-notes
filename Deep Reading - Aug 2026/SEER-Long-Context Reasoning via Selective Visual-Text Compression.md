---
user_id: "cheng tan"
paper_id: 8150
arxiv_id: "2608.15962"
title: "SEER: Long-Context Reasoning via Selective Visual-Text Compression"
publish_date: "2026-08-18"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.15962.pdf"
pdf_url: "https://arxiv.org/pdf/2608.15962"
abs_url: "https://arxiv.org/abs/2608.15962"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-19T09:36:04"
---
# SEER: Long-Context Reasoning via Selective Visual-Text Compression

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：long-context reasoning · visual-text compression · selective retrieval · tool invocation

## 一句话总结

SEER 提出一种选择性视觉-文本压缩框架，通过视觉扫描挑选查询相关的图像页，仅在需要时调用工具检索原文，以在长上下文推理中同时获得视觉压缩的效率与文本检索的精度，并在 LongBench 上以 51.11% 平均准确率超过 Glyph-9B 2.33 分、超过 Qwen3-8B 3.49 分。

## 摘要

> Long-context reasoning remains computationally expensive for large language models due to the quadratic complexity of attention over text tokens. Visual-text compression offers a promising alternative by rendering text into images and processing them with vision-language models, often reducing token usage. However, existing approaches apply uniform compression regardless of query relevance, potentially sacrificing precision where detailed extraction is required. We present SEER, a framework that learns to select query-relevant images through visual scanning and retrieve textual content only where needed, combining the efficiency of visual compression with the precision of text-based reasoning. Through supervised fine-tuning on tool-interaction trajectories, SEER learns adaptive tool invocation for selection and retrieval. Experiments on long-context benchmarks show that SEER improves extraction precision through selective text retrieval while retaining average prompt-token savings relative to full-text baselines. On LongBench, SEER achieves 51.11% average accuracy, outperforming the visual-text baseline Glyph-9B by 2.33 points and Qwen3-8B by 3.49 points. Code can be accessed at https://github.com/jiaweixu98/SEER.

Q1: 这篇论文试图解决什么问题？

1. 核心矛盾：长上下文推理需要处理大量文本，但注意力机制二次复杂度导致计算成本高企；视觉-文本压缩能显著降低 token 数，但均匀压缩不区分查询相关性，对需要精确细节抽取的任务不友好，可能丢失关键信息。
2. 具体问题：现有视觉压缩方法（如 Glyph-9B 所属的视觉-文本压缩管线）对所有页面采用相同压缩策略，没有考虑问题时哪些页面真正需要文本级精度，造成“为了效率牺牲精度”的误伤。
3. 隐含难点：如何让系统自行判断哪些视觉压缩后的图像已足够回答、哪些必须恢复为原始文本？这需要模型具备查询驱动的感知能力，并在选择错误时承担额外的推理成本或精度损失。
4. 部署层面：选择机制本身可能不稳定（如 SFT 模型偶尔输出越界页面索引），这提醒我们：自动工具调用接口的鲁棒性本身就是需要解决的问题。
5. 数据层面：训练数据的构造依赖 teacher 筛选，可能系统性低估某些证据分布（如证据分散在多个页面的情况），使局限性在训练数据中就被固化。

Q2: 有哪些相关研究？

基于摘要和引言的逻辑，相关研究可分为几类（因未提供 Related Work 原文，以下按方向归纳，部分推断）：
1. 长上下文文本模型：通过扩展注意力、稀疏注意力、状态空间模型等方式直接处理长文本，如 Qwen3-8B 等外部基线代表；但计算开销仍未根本解决。
2. 视觉-文本压缩：把文本渲染成图像、交给 VLM 处理，典型代表为 Glyph-9B（Cheng et al., 2026），这类方法压缩率高但精度受限。
3. 选择性检索 / 工具调用：模型借助检索器或工具在长上下文中按需获取信息，类似 RAG 或 agent-based 方法；SEER 把选择与检索建模为工具调用来学习。
4. “全局理解 + 局部精确” 的混合范式：例如先让模型粗读全局、再对关键区域精读，SEER 的视觉扫描 + 选择性文本检索属于该范式。
5. 推理时计算自适应：根据查询难度动态分配计算资源，SEER 通过选择性压缩实现 token 节约。
注意：以上关于相关工作的具体分类仅从摘要和 fragment 推断，论文原文的 Related Work 段落可能涉及更多细节，建议核对原文。

Q3: 论文如何解决这个问题？

SEER 的整体方案是“先扫描、后选择、按需检索”的三阶段推理管线，并通过监督微调让模型学会调用工具。
1. 视觉扫描阶段：把整个文档渲染成图像后，模型使用 VLM 对全部页面做快速视觉扫描，获取全局语境与候选页面印象。
2. 选择阶段：根据用户问题，模型从扫描结果中筛选出高度相关的图像页（由 SFT 学习调用选择工具）。
3. 检索与推理阶段：仅对被选中的页面调用文本检索工具，恢复其原始文本，供 LLM 做精确抽取与推理生成最终答案。
4. 训练策略：在工具交互轨迹上做监督微调（SFT），联合优化“选择”和“推理”能力——训练数据包含哪些页面被选中、何时需要检索的轨迹，模型因而学会区分“全局理解足够”与“必须精读”的情况。
5. 关键权衡：目标函数是在不损失精度的前提下使平均 prompt token 数低于全文本基线；从实验结果看，SEER 在保持 token 节约的同时提升精度，说明选择机制有效。
6. 和基线 Glyph-9B 的差异：Glyph-9B 等视觉压缩方法对所有页面均匀压缩，SEER 则对它做查询导向的选择性压缩——这是方法上的核心区别。

Q4: 论文做了哪些实验？

由于提供的证据只包含实验部分的摘要性描述，具体实验矩阵无法穷尽呈现。可确认的实验设计包括：
1. 数据集：在 LongBench（Bai et al., 2024）上评测，这是一个包含 21 个任务、横跨 6 类任务的中英双语长上下文基准。
2. 对比基线：
 - 文本类长上下文 LLM（如 Qwen3-8B）；
 - 视觉-文本压缩方法（如 Glyph-9B，与 SEER 同属于同一基础架构家族和视觉压缩管线，被用作受控主对比）。
3. 评测指标：平均准确率（LongBench 平均），以及平均 prompt token 节约量。
4. 受控对照：Glyph-9B 与 SEER 共享基础架构和视觉压缩管线，因此可以直接归因于选择性机制的贡献；对外部基线的比较主要用于参考。
5. 消融实验位置：摘要中提到 ablation 放在 Section 6.4，关于“查询要求细致信息时往往只需要上下文的一小部分，而其余内容主要服务于全局理解”，但具体消融设计未在证据中提供。
6. 未知细节：任务级分解、选择精度、token 节约的具体比例、不同页数情况下的表现、错误案例分析等均未在提供的 fragment 中给出，需要查阅原文第 5-6 节。

Q5: 发现了什么实验现象？

从可获得的证据可以提取以下实验现象：
1. 选择性压缩带来精度提升：SEER 在 LongBench 上平均准确率 51.11%，比视觉压缩基线 Glyph-9B 高 2.33 个百分点；这说明“查询相关选择”确实挽回了一部分均匀压缩丢失的精确信息。
2. 优于同规模通用 LLM：SEER 比 Qwen3-8B 高 3.49 个百分点，表明视觉压缩 + 选择性检索的组合可以在长上下文任务中胜过直接处理原始文本的 8B 文本模型。
3. token 节约与精度并存：SEER 相对于全文本基线保留平均 prompt token 节约——这是摘要中明确宣称的效果，说明选择性机制没有导致 token 开销回退。
4. 关于证据分布的直觉：论文提到“证据分散时（视觉扫描得到的）信息变薄”，且在训练数据中该情况被低估——这暗示 SEER 在证据集中在一处时表现较好，而在证据跨页分散时可能性能下降；这是一个重要的边界条件。
5. 选择接口不稳健：SFT 模型偶会输出越界页面索引，系统选择直接丢弃而非纠正，这种错误会传播到后续检索与推理，构成部署层面的失败模式。
6. 未见公开的负结果细节：没有证据显示 SEER 在哪些具体任务上表现弱于基线，也未看到 token 节约的量化数字，这些缺口需要查原文。

Q6: 有什么可以进一步探索的点？

1. 鲁棒选择机制：改进工具调用接口，避免越界索引等错误输出，可引入结构化输出约束、自校验或纠错回路。
2. 证据分散场景：论文自认证据跨页分散时性能下降且训练数据低估该情形，未来可构造更均衡的训练集，或设计多页联合检索策略。
3. 任务泛化：SEER 目前专门面向问答任务（limitations 中明确说明），扩展到时序推理、多跳综合、摘要生成等任务需要调整训练信号与工具定义。
4. 动态阈值：当前模型学的是硬性选择（选 or 不选），可以探索根据查询复杂度和页面相关性输出软置信度，再决定是否检索，提升稳健性。
5. 与长上下文模型的混合：SEER 的选择可以级联到更强的文本模型上，或与稀疏注意力、状态空间模型组合，进一步降低计算。
6. 更细粒度的检索：目前选择粒度是“页面”，未来可细化到段落、句子，或按需只检索局部片段。
7. 训练数据生成：answer-conditioned teacher filtering 可能引入偏差，未来可以改用更细粒度的证据标注或使用更强的 teacher 生成轨迹。
8. 多模态文档：SEER 的视觉扫描天然支持含图文档，可探索在 PDF、网页、幻灯片等多模态长文档上的应用。
9. 效率与精度的帕累托分析：在多个压缩率 / 检索预算设置下测量准确率-成本曲线，找到更好的权衡点。

Q7: 总结一下论文的主要内容

SEER 论文应对的痛点是长上下文推理的两难：纯文本注意力代价高，纯视觉压缩又丢精度。作者观察到多数长文档问答只需从少数页面抽取精确信息，其余页面提供全局背景即可。于是提出 SEER——一个“选择性视觉-文本压缩”框架，其思想是将整个文档渲染成图像后先用 VLM 快速视觉扫描一遍，再由模型根据问题用工具选择出候选页面，最后只对选中的页面恢复文本并交给模型精确作答。这个流程意味着模型在同一份文档上既得到视觉压缩带来的 token 节约，又得到文本检索带来的抽取精度，两者通过工具调用来衔接。
技术路线上，SEER 把选择与检索都建模为工具调用，并在工具交互轨迹上做监督微调（SFT），让模型自己学习何时需要精确文本、何时视觉信息足够。这使得“选择”不再是启发式规则，而是模型能力的一部分。对比上，论文选择了与 SEER 共享基础架构和视觉压缩管线的 Glyph-9B 作为受控基线，又对比了文本型长上下文模型 Qwen3-8B，在 LongBench 的 21 个任务上进行评测。结果显示 SEER 以 51.11% 平均准确率超过 Glyph-9B 2.33 分、超过 Qwen3-8B 3.49 分，同时仍保持 prompt token 节约。
论文同时坦诚了四点局限：第一，部署层面选择接口不稳健，有时输出越界页面索引且未纠正；第二，证据分散时视觉扫描信息变薄，且训练数据的 answer-conditioned teacher filtering 低估了该场景；第三，SEER 目前专用于 QA，泛化性未验证；第四，关于其他场景的边界尚未充分讨论。整体上，SEER 代表了一条“以查询为中心的压缩”路线，把视觉压缩的效率和文本检索的精度统一到同一个可学习框架中，是长上下文推理中效率-精度权衡的一种新解法。
（注：本总结基于提供的 Abstract、Introduction、Experiments、Conclusion 和 Limitations 片段，具体细节请以全文为准。）

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文回答了一个核心问题：如何让长上下文推理的视觉压缩不再“均匀”而是“查询感知”。

## 基本信息

- 作者：Jiawei Xu, Zhilin Zhai, Jinrui Fang, Ruohan Xu, Mingfei Lu, Yi Zhang, Guanchu Wang, Tianlong Chen, Ying Ding
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.CV
- 日期：2026-08-18
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.15962`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Abstract、Introduction、Experiments、Conclusion 和 Limitations 片段，并结合元数据与启发式草稿进行补全；未命中全文的 Method/实验细节故相应字段有所保留。
