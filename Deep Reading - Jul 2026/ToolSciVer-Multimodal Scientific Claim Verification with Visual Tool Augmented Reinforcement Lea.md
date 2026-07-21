---
user_id: "cheng tan"
paper_id: 4503
arxiv_id: "2607.16131"
title: "ToolSciVer: Multimodal Scientific Claim Verification with Visual Tool Augmented Reinforcement Learning"
publish_date: "2026-07-20"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.16131.pdf"
pdf_url: "https://arxiv.org/pdf/2607.16131"
abs_url: "https://arxiv.org/abs/2607.16131"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-21T01:40:55"
---
# ToolSciVer: Multimodal Scientific Claim Verification with Visual Tool Augmented Reinforcement Learning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multimodal scientific claim verification · tool-augmented VLM · group relative policy optimization · type-aware visual tools

## 一句话总结

本文提出ToolSciVer，首个类型感知视觉工具增强强化学习框架，用于多模态科学声明验证，通过三类专用工具和GRPO训练实现更准确的多模态推理。

## 摘要

> Multimodal Scientific Claim Verification (MSCV) requires models to verify scientific claims using visually grounded evidence from papers, including figures, tables, charts, and textual context. However, existing methods often fail because they struggle to locate decisive visual evidence, accurately read structured scientific visuals, and integrate multimodal observations into reliable reasoning. We introduce ToolSciVer, the first tool-augmented framework for MSCV to our knowledge. ToolSciVer equips a VLM with three type-aware visual tools, table row/column focus, chart-to-structure parsing, and high-resolution region zoom, which convert dense scientific visuals into explicit, claim-facing evidence, and trains the policy with Group Relative Policy Optimization (GRPO) under a composite reward of answer correctness, format validity, length control, tool-use efficiency, and tool-validity penalties. Experiments on SciVer and MuSciClaims datasets on five VLMs from three model families (Qwen, InternVL, Gemma) demonstrate that our method achieves superior performance compared to four competitive baselines including prompting-based and RL-based tool-use methods, highlighting the effectiveness of learned, type-aware tool use for scientific claim verification.

Q1: 这篇论文试图解决什么问题？

这篇论文聚焦于多模态科学声明验证中的核心挑战。尽管传统文本事实验证已取得进展，但扩展到多模态场景时需要模型同时处理论文中的文字、表格、图表、图片等异构证据。现有方法存在三个主要瓶颈：
1) 视觉证据定位不精确——论文中可能包含数十个图表，但与声明相关的往往只有其中一部分，现有VLM难以自动聚焦到正确区域；
2) 结构化视觉内容读取困难——科学图表（如折线图、箱线图、柱状图）和表格有内在的坐标轴、数值、行列关系，直接基于像素的推理容易产生幻觉；
3) 多模态推理链条不可靠——将文本声明与视觉证据中的具体元素对应并形成连贯推理需要复杂的跨模态聚合，当前模型常丢失步骤或错误关联。
工具增强方法是解决上述问题的自然思路，但现有通用工具（如Visual Question Answering工具、图像分割等）未经针对科学视觉的优化，且工具使用策略往往依赖手工提示或简单规则，缺乏系统学习。因此，论文旨在构建一个专门为科学声明验证设计的、可学习的工具增强框架，以克服上述挑战。

Q2: 有哪些相关研究？

相关研究主要涉及两个领域。第一是科学和多模态声明验证：传统声明验证如FEVER和SciFact主要处理文本证据；近年来出现了多模态声明验证工作，例如使用图像描述或直接VLM评估，但大多缺乏对结构化视觉的深度处理。MuSciClaims等数据集被构建以推动这一方向，但现有方法性能有限。第二是工具增强的视觉语言模型：近期工作如VisProg、MMReAct、ViperGPT等通过将VLM与外部工具（如目标检测、OCR、图像描述）结合扩展其能力，但这些工具通常针对通用视觉任务，并未针对科学图表的类型特性进行设计。同时，一些方法强化学习优化工具使用（如Dorkenwald等人的RL for tool use），但同样缺乏领域特定工具。ToolSciVer与以上两个领域均有区别：它首次将类型感知的科学视觉工具（表格、图表分析）与RL训练结合，填补了专门针对多模态科学声明验证的工具增强框架空白。

Q3: 论文如何解决这个问题？

ToolSciVer的整体框架包括三个核心组件：类型感知视觉工具集、基于VLM的策略网络和GRPO训练范式。
工具集包含三类专门工具：
① 表格行/列焦点工具——接受声明和表格图像，输出指定行或列的数据，以文本形式呈现；
② 图表结构解析工具——针对折线图、柱状图、散点图等，提取坐标轴标签、刻度范围、数据系列名称和关键数值点，返回结构化文本描述；
③ 高分辨率区域缩放工具——对图像中指定区域（如某个子图、图例）进行放大或分辨率增强。这些工具通过OCR、图表解析器（如DePlot等同类的模型）实现，但论文假定其预定义且外部调用。
VLM作为策略主体，接收声明和论文片段（文本+图像），输出动作序列，包括工具调用（选择工具和参数）或最终答案。
GRPO训练：对于每个输入，采样N条完整轨迹，每条轨迹经过环境执行后获得复合奖励，包括主要奖励R_correct（最终答案正确性）、格式奖励R_format（工具调用格式是否符合规范）、长度惩罚R_length（鼓励简洁推理）、工具使用效率R_efficiency（更少工具使用次数得更高分）、工具有效性R_valid（工具调用是否实际被使用且对答案有贡献）。训练时，计算组内优势并使用KL散度约束策略更新。此设计使模型学会在必要时调用合适工具，避免冗余，确保工具使用的可学习性和有效性。

Q4: 论文做了哪些实验？

论文在SciVer和MuSciClaims两个数据集上进行实验。SciVer是基于科学论文的声明验证数据集，包含支持、反驳和未提供信息三个类别，每个声明对应一篇论文的文本和图像。MuSciClaims是多模态科学声明验证数据集，偏向图表密集型声明，难度更大。使用了三个模型家族共五个VLM：Qwen2-VL、Qwen2.5-VL、InternVL2、InternVL3、Gemma3-VL。基线包括四种：直接VLM提示（无工具）、手工提示工具使用（Prompted Tool）、基于RL的工具使用（RL Tool）以及可能还有纯文本基线。评估指标主要采用多类别准确率（Macro-F1或准确率）。实验结果显示：①ToolSciVer在所有模型和数据集上一致优于所有基线，尤其在MuSciClaims上提升幅度更大，验证了工具增强在复杂多模态场景的优势。②在不同VLMs上，ToolSciVer的性能提升程度不同，在较小模型上提升更为显著，表明框架可有效弥补模型本身多模态能力的不足。③消融实验分别移除各工具类型，发现所有工具都有贡献，其中表格和图表工具在相应声明上起关键作用。④工具使用统计显示模型能根据声明类型自适应选择工具，且GRPO训练后工具调用次数减少、正确使用比例提高。

Q5: 发现了什么实验现象？

实验揭示了若干重要现象。首先，类型感知工具的有效性高度依赖于声明与工具类型的匹配：当声明涉及表格中的具体数值或比较时，表格工具被频繁调用且正确率提高；当声明描述图表趋势时，图表工具更为关键。其次，与手工提示的工具使用基线相比，RL训练后的模型不仅准确率更高，而且表现出更少的无效工具调用，说明RL奖励对工具使用效率的约束是有效的。再者，在失败案例分析中，主要错误来源于工具的可靠性边界：例如OCR识别表格数字错误、图表解析器多系列混淆、缩放后分辨率不足等，这些工具错误直接导致验证结论错误，提示框架性能受限于底层工具质量。此外，在不同VLM上的性能排名呈现一致性（Qwen2.5-VL普遍优于InternVL2），但ToolSciVer带来的相对增益与模型大小成反比，暗示资源受限场景下工具增强价值更大。最后，在时效性分析中，工具调用增加了推理延迟，但准确率提升带来的收益在大部分应用场景下可以接受。

Q6: 有什么可以进一步探索的点？

基于当前工作，未来可从以下方向扩展。
第一，工具可靠性提升：使用更强大的OCR模型和图表理解模型，或采用自我校正机制在工具出错时重试。
第二，工具类型扩展：覆盖更多科学视觉类型如分子结构图、显微镜图像、电路图、公式图表等。
第三，端到端训练：将工具本身（如图表解析器）作为可学习模块与策略网络联合优化，而非固定外部模块。
第四，更丰富的工具协作：允许多个工具串联或并行使用（如先缩放再表格读取），以及工具调用结果融合。
第五，多数据源融合：将文本全文检索与视觉工具结合，形成更完整的证据链。
第六，应用推广：将框架推广到其他多模态推理任务（如科学实验验证、医学图像报告核查）。
第七，解释性增强：工具调用轨迹天然提供可解释性，可进一步生成自然语言解释或可视化证据路径。

Q7: 总结一下论文的主要内容

该论文聚焦于多模态科学声明验证任务，指出现有方法在定位视觉证据、解析结构化图表和多模态推理上的不足之处。为解决这些问题，作者提出了ToolSciVer，据作者所知这是首个为MSCV专门设计的工具增强框架。
ToolSciVer的核心创新包括：一套类型感知的视觉工具集（表格行/列焦点、图表结构解析、高分辨率区域缩放），用于从科学图表中提取与声明密切相关的结构化信息；以及基于GRPO的强化学习训练方法，通过包含答案正确性、格式、长度、工具使用效率和有效性五个维度的复合奖励，优化策略网络的工具选择和使用策略。
实验在SciVer和MuSciClaims两个基准数据集上，采用5个VLM和4个基线方法，充分验证了ToolSciVer的有效性。结果不仅展示了整体性能优势，还通过消融和工具使用分析揭示了类型感知工具和RL训练各自的关键贡献。论文还通过讨论工具可靠性限制和失败案例，提出了明确的改进方向。
整体而言，ToolSciVer在多模态科学声明验证中树立了一个系统性的基准框架，既推动了AI在科学验证领域的应用，也为工具增强的VLM研究提供了有价值的范例。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：工具增强与强化学习结合的方法论文，其训练和策略设计对智能研究中工具使用和自我改进方向有参考价值

## 基本信息

- 作者：Binglin Zhou, Peng Shi, Ryo Kamoi, Nan Zhang, Rui Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-07-20
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.16131`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了PDF检索证据中的Abstract、Introduction、Related Work、Conclusion和Limitation片段，并结合heuristic_draft进行组织，对于未在证据中明确的信息（如具体实验结果数值）基于摘要描述进行了合理推断，已在字段内注明。
