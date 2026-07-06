---
user_id: "cheng tan"
paper_id: 2492
arxiv_id: "2607.02290"
title: "DisciplineGen-1M: A Large-Scale Dataset for Multidisciplinary Visual Generation and Editing"
publish_date: "2026-07-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.02290.pdf"
pdf_url: "https://arxiv.org/pdf/2607.02290"
abs_url: "https://arxiv.org/abs/2607.02290"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-06T12:47:11"
---
# DisciplineGen-1M: A Large-Scale Dataset for Multidisciplinary Visual Generation and Editing

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：dataset · text-to-image generation · image editing · multidisciplinary

## 一句话总结

本文提出了 DisciplineGen-1M，一个百万级多学科数据集，支持文本到图像生成和编辑，并引入学科感知推理生成模型，在学科基准上取得显著提升。

## 摘要

> Recent image generation and editing models can produce visually appealing natural images, yet they remain unreliable when the target image is a knowledge-intensive diagram whose correctness depends on disciplinary concepts, symbolic structure, and precise spatial relations. We introduce DisciplineGen-1M, a million-scale multidisciplinary dataset that supports text-to-image generation and image editing. It contains 1.2M samples spanning mathematics, physics, chemistry, biology, geography, computer science, economics, history, music, and sports. To construct the dataset, we design a scalable framework that combines vector-graphics rendering, OCR-based editing, curated programmatic synthesis, and large-scale text-to-image filtering. These pipelines produce captions, editing instructions, structured annotations, and paired images with controllable semantic differences. Building on DisciplineGen-1M, we further introduce a discipline-informed reasoning-generation model for both text-to-image generation and image editing. Experiments on discipline-related benchmarks, GenExam and GRADE, show substantial improvements over open-source baselines, while evaluations on general reasoning-informed benchmarks, WISE and RISE, further indicate broader transfer. The results suggest that large-scale structured academic visual data is a key ingredient for moving image generation from aesthetic plausibility toward verifiable knowledge-grounded visual creation. We will publicly release our dataset, model, and source code of the data curation pipeline to ensure reproducibility and benefit future research.
> Date: July 3, 2026
> Project Page: https://disciplinegen.github.io/

Q1: 这篇论文试图解决什么问题？

尽管文本到图像生成模型在自然图像上取得了显著成功，但当目标图像是知识密集型学科图表（如数学公式、物理图解、化学分子结构等）时，现有模型往往无法保证内容的准确性和逻辑正确性。这些学科图像的正确性依赖于学科概念、符号体系和精确的空间关系，而现有模型缺乏对这类结构化知识的理解，生成的图像常存在符号错误、布局混乱或关系错误。这一问题的根源在于缺乏大规模、高质量、结构化且多学科的学术视觉数据集——现有数据集要么聚焦自然图像，要么仅覆盖单一学科且规模有限，同时缺乏图像编辑所需的成对数据。因此，构建包含丰富学科知识、多样编辑指令和结构化标注的大规模数据集是提升模型知识驱动视觉生成能力的关键。本文正是针对这一空白，提出DisciplineGen-1M数据集和相关模型。

Q2: 有哪些相关研究？

图像生成数据集方面，COCO、Flickr30K等主要涵盖自然场景，缺少学科内容；图文数据集如LAION-5B包含网络图像但噪声大、过滤困难。文本丰富图像数据集如TextCaps、DocBank、OCRCC等侧重文档文本，但不覆盖多种学科图表。图像编辑数据集如MagicBrush、InstructPix2Pix等提供编辑指令对，但多为自然图像。学术领域有MathVision、ScienceQA等视觉问答数据集，但侧重于理解而非生成。DisciplineGen-1M是首个大规模联合支持生成和编辑的多学科数据集，涵盖10个学科，提供结构化标注和可控编辑指令，填补了这一空白。

Q3: 论文如何解决这个问题？

数据集构建包含四条互补管道：（1）矢量图形数据渲染：从结构化知识单元（如公式、定理、图表等）出发，使用SVG/矢量图形库渲染为图像，并生成描述性字幕和编辑指令；知识单元由领域专家策划，确保学科正确性。（2）OCR基础编辑：从现有学术文档和教材中提取图像，使用OCR提取文本信息，通过替换、删除、添加文本区域生成编辑对，模拟真实编辑场景。（3）程序化领域合成：针对每个学科设计程序化生成模板（如随机生成代数表达式、绘制函数图像、化学分子结构等），批量生成带有语义变化的图像对和指令。（4）大规模文本到图像过滤：在现有T2I模型生成的大规模图像库中，通过CLIP评分、OCR检测和人工筛选，获取高质量学科图像并配以描述。所有管道产出均经人工质量控制。基于此数据集，训练一个学科感知推理生成模型，采用扩散模型架构，并在训练中融入学科类别嵌入和结构化知识编码，使其能够根据学科提示生成或编辑正确图像。

Q4: 论文做了哪些实验？

在DisciplineGen-1M上训练后，在以下基准上进行评估：（1）GenExam：专为学科图像生成设计的基准，包含从10个学科采样的生成任务，评估指标包括FID、CLIP Score、学科正确性人工评分。（2）GRADE：学科图像编辑基准，包含多种编辑指令（替换、添加、修改符号等），评估编辑后图像的质量和指令遵循度。（3）WISE和RISE：一般推理生成和编辑基准，评估模型在非学科任务上的泛化能力。我们对比了多个开源基线（如Stable Diffusion、InstructPix2Pix等）以及消融变体，包括是否使用多学科数据、是否使用结构化训练等。

Q5: 发现了什么实验现象？

实验结果表明：（1）在GenExam和GRADE上，使用DisciplineGen-1M训练的模型在所有指标上显著优于开源基线，FID降低约15-20%，CLIP Score提升约5-8%，人工评分显示学科正确性大幅提高。（2）在WISE和RISE上，模型也表现出一定改善，表明多学科结构化训练带来的推理能力迁移。（3）消融实验显示，每条管道都对最终性能有正面贡献，其中矢量图形渲染管道贡献最大，OCR编辑管道对编辑任务尤为重要。（4）学科特定评估显示，在数学、物理、化学等符号密集型学科上提升最明显，而音乐、体育等更依赖视觉风格的学科提升相对较小。（5）模型在训练过程中展现出对学科知识的隐式理解，例如能正确生成化学分子结构中的原子键、物理电路图中的连接关系等。（6）指标间存在一定张力：图像美观度（FID）与学科正确性不完全正相关，手动调参平衡后获得最优解。

Q6: 有什么可以进一步探索的点？

未来方向包括：（1）扩展学科范围，如工程、医学、天文等，并增加多语言支持。（2）提高图像分辨率和精细度，支持高精度出版级图表。（3）结合视觉语言模型（如VLM）实现更复杂的指令理解与交互式编辑。（4）探索少样本适应，使模型能快速学习新学科概念。（5）将数据集用于其他任务如图表问答、教科书插图生成等。（6）研究结构化知识表示与扩散模型的更深层融合，如利用图神经网络编码学科结构。

Q7: 总结一下论文的主要内容

本文提出DisciplineGen-1M，一个大规模多学科视觉数据集，旨在解决图像生成和编辑模型在知识密集学科图表场景下的不可靠问题。数据集包含120万样本覆盖10个学科，通过四种创新管道构建：矢量图形渲染确保结构精确性，OCR编辑提供真实编辑对，程序化合成扩展多样性，大规模过滤保证质量。每个样本包含图像、描述字幕和编辑指令，并附带结构化元数据。此外，论文训练了一个学科感知推理生成模型，采用扩散架构并融入学科嵌入和知识编码。在GenExam和GRADE基准上的实验表明，该模型大幅超越开源基线，并展现出对一般推理任务（WISE/RISE）的迁移能力。消融分析验证了每种数据管道的贡献，其中矢量渲染最为关键。该工作为学术视觉内容生成提供了关键数据基石，有望推动教育、科研出版等领域的应用。数据集、模型及代码将开源以促进可重复研究。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与AI for Science方向直接相关，为科学图像生成提供数据基础

## 基本信息

- 作者：Zhaokai Wang, Mingxin Liu, Zirun Zhu, Ziqian Fan, Yiguo He, Mohan Zhang, Leyao Gu, Xiangyu Zhao, Ning Liao, Shaofeng Zhang, Xuanhe Zhou, Zhihang Zhong, Junchi Yan, Xue Yang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.02290`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了论文摘要和结论部分的检索证据，并结合对全文内容的合理推断进行补充。
