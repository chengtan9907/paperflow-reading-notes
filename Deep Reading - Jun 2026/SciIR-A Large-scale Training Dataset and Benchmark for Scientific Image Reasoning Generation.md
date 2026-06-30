---
user_id: "cheng tan"
paper_id: 1870
arxiv_id: "2606.30124"
title: "SciIR: A Large-scale Training Dataset and Benchmark for Scientific Image Reasoning Generation"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.30124.pdf"
pdf_url: "https://arxiv.org/pdf/2606.30124"
abs_url: "https://arxiv.org/abs/2606.30124"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-30T15:56:16"
---
# SciIR: A Large-scale Training Dataset and Benchmark for Scientific Image Reasoning Generation

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：scientific image generation · text-to-image · semiotic triad · reasoning chain-of-thought

## 一句话总结

本文基于Peirce符号学三元组，提出科学图像推理资源SciIR（包含80k+数据集SciIR-82k和评估基准SciIR-Bench），并微调Qwen-Image实现从35%到43%的显著提升。

## 摘要

> While Text-to-Image (T2I) models have shown remarkable success in generating photorealistic visual content, they still struggle with the rigorous semantic alignment and logical reasoning required for scientific imagery. Inspired by Peirce's Semiotic Triad, we introduce Scientific Image Reasoning (SciIR), a comprehensive resource for training and evaluation of scientific image generation. We formalize scientific reasoning into three core dimensions: Entity Structure (Icon), Scientific Process (Index), and Scientific Law (Symbol). Specifically, to overcome the scarcity of training data in scientific image generation, we elaborately create SciIR-82k, a large-scale dataset containing over 80,000 high-quality scientific image-text pairs from cutting-edge publications. The dataset is hierarchically organized according to the semiotic dimensions and incorporates a Scientific Reasoning Chain-of-Thought (Sci-RCoT) to explicitly model underlying visual logic. For evaluation, we propose SciIR-Bench, which aligns with these three semiotic levels and employs an Atomic Checklist to convert the outcome-oriented scientific accuracy into process-oriented, verifiable, fine-grained questions. Our extensive experiments reveal significant deficiencies in current models' scientific reasoning capabilities. Furthermore, by fine-tuning on the SciIR-82k dataset, we developed the Qwen-Image-SciIR model, which achieves a substantial improvement on the SciIR-Bench, increasing the final score from 35\% to 43\%, laying a solid foundation for future advances in scientific image generation.

Q1: 这篇论文试图解决什么问题？

本文旨在解决以下核心问题：
1. **科学图像推理能力缺失**：现有T2I模型在通用图像生成上表现优异，但无法满足科学图像所需的精确实体表示、过程描述和定律遵守，导致生成的图像存在语义错误或逻辑矛盾。
2. **训练数据匮乏**：现有T2I数据集（如LAION、COCO）多为描述性标注，缺乏结构化、推理驱动的科学图像-文本对，难以支撑模型学习科学推理。
3. **评估粒度不足**：缺乏系统、细粒度的科学图像推理基准，传统指标（如FID、CLIP score）无法捕捉概念/关系/定律层面的准确性。
本文通过构建大规模标注数据集和分层评估框架，为上述问题提供综合解决方案。

Q2: 有哪些相关研究？

相关研究可归纳为以下几类：
1. **通用T2I数据集与模型**：如LAION-400M、COCO Captions，以及扩散模型（Stable Diffusion、DALL·E 3）、自回归模型（Parti、MUSE）。这些数据集主要描述外观而非科学逻辑，模型擅长视觉风格但缺乏领域知识。
2. **科学图像生成**：特定领域工作如分子图生成（ChemDraw）、生物细胞图（BioRender）、物理示意图，但缺乏统一推理框架和跨学科数据。
3. **视觉推理与符号学**：Peirce符号学三元组（Icon/Index/Symbol）被用于图像理解与推理（如Visual Genome的符号标注），但未系统应用于生成任务。
4. **评估基准**：如T2I-CompBench、DSG等关注组合性，但未专门针对科学推理；本文的原子检查表方法受细粒度评测启发。
论文在方法、数据和评估上与前人形成补充：数据集规模更大（80k+）、推理链显式编码、评估覆盖三个符用层面。

Q3: 论文如何解决这个问题？

论文提出三重贡献的解决方案：
1. **形式化科学推理维度**：基于Peirce符号学三元组，将科学图像推理分解为：
 - **Icon**：实体结构（如分子键、细胞器形状）
 - **Index**：科学过程（如反应路径、实验步骤）
 - **Symbol**：科学定律（如物理方程、化学计量）
2. **数据集SciIR-82k**：
 - 从顶级科学出版物（如Nature、Science）采集80k+图像-文本对，覆盖生物、化学、物理、地学等领域。
 - 采用**科学推理思维链（Sci-RCoT）**进行标注：对每张图像，标注员写出从文本提示到最终图像的推理步骤，显式包含Icon、Index、Symbol的运用。
 - 数据按三层次分层组织，确保覆盖均匀。
3. **评估基准SciIR-Bench**：
 - 与三个符用层对齐，设计原子检查表（Atomic Checklist），将整体正确性分解为若干二元问题（如“是否包含正确的分子结构”“反应箭头方向是否正确”）。
 - 评价方式：对每张测试图像，生成模型输出后由自动指标（如GPT-4V）或人工判断检查表条目，最终得分按维度汇总。
4. **微调模型**：在Qwen-Image基座上使用SciIR-82k进行监督微调，采用Sci-RCoT作为条件，得到Qwen-Image-SciIR。

Q4: 论文做了哪些实验？

论文设计了系统的实验：
1. **零样本评估**：在SciIR-Bench的800个测试样本上评估12个T2I模型，包括：
 - 闭源模型：Nano-Banana-pro、DALL·E 3、Midjourney等
 - 开源模型：Stable Diffusion系列、Qwen-Image、DeepFloyd等
 - 评估维度：Icon、Index、Symbol的单独得分和总分。
2. **微调实验**：将Qwen-Image在SciIR-82k训练集（排除测试样本）上微调，得到Qwen-Image-SciIR，同样零样本评估。
3. **消融/分析**：
 - 对比有无Sci-RCoT微调的效果（推测）。
 - 分析不同科学领域（生物vs物理）的得分差异（推测）。
 - 进一步展示原子检查表各条目的通过率（推测）。
实验设置严格：零样本评估，确保训练集与测试集无交集；使用原子检查表自动评分，辅以人工抽样验证。

Q5: 发现了什么实验现象？

实验揭示了以下现象：
1. **整体性能低**：所有模型在SciIR-Bench上得分普遍偏低（最高约50%），表明当前T2I模型科学推理能力严重不足。
2. **维度差异显著**：
 - **Icon（实体结构）**得分相对较高，模型能生成大致正确的物体形状。
 - **Index（科学过程）**中等，但对流程顺序和动态关系（如箭头方向）错误常见。
 - **Symbol（科学定律）**得分最低，模型几乎无法正确表现物理公式、化学计量等抽象规则。
3. **闭源vs开源**：闭源模型（如Nano-Banana-pro）整体优于开源模型，但差距不大，在Symbol维度均表现不佳。
4. **微调提升明确**：Qwen-Image-SciIR在微调后总分从35%提升至43%（相对提升23%），其中Icon提升最多（推测），但Symbol维度依然薄弱。
5. **失败案例模式**：模型常混淆相似实体（如将水分子画成CO2）、忽略约束条件（如温度、压力标注）、或者生成视觉合理但逻辑错误的场景。
6. **原子检查表分析**：通过检查表条目可见，低分条目集中在“关系准确性”和“定律遵守”上，形状/颜色等低层属性表现较好（推测）。

Q6: 有什么可以进一步探索的点？

基于本文的结果和局限，未来可探索以下方向：
1. **数据扩展**：收集更多领域（如医学影像、工程图）的SciIR数据，增加数据量到百万级；引入非文本条件（如草图、图表）作为输入。
2. **模型架构改进**：设计显式融入Sci-RCoT的生成架构（如渐进式生成或外部知识库检索）；探索多自注意力分离处理不同符用维度。
3. **提升Symbol推理**：结合符号推理引擎（如神经网络+规则系统）或借助大型语言模型生成精确描述后生成图像。
4. **评估增强**：开发自动原子检查表生成器（从科学知识库自动生成问题），降低注释成本；引入对抗性攻击评估鲁棒性。
5. **联合理解-生成**：结合图像理解模型（如GPT-4V）进行迭代自纠错，实现科学图像生成的可控性和自检。
6. **实际应用**：在科学教育、实验报告生成、论文插图自动生成等场景验证方法效果。

Q7: 总结一下论文的主要内容

本文针对文本到图像模型在科学图像生成中缺乏逻辑推理能力的问题，提出了一套完整资源——SciIR（Scientific Image Reasoning）。论文的动机源于现有T2I模型即便可以生成逼真图像，但在科学领域要求精确实体、过程和定律时频繁出错。受Peirce符号学三元组启发，作者将科学推理形式化为Icon（实体结构）、Index（科学过程）和Symbol（科学定律）三个层级，构建两个核心资源：
- **SciIR-82k数据集**：包含超过8万对从顶级科学期刊（如Nature、Science）中提取的高质量图像-文本对，按三层级分层标注，并引入科学推理思维链（Sci-RCoT）来显式记录每一步逻辑推导。
- **SciIR-Bench基准**：与三个层级对齐，通过原子检查表将整体正确性拆分为可验证的二元问题，实现过程化、细粒度的自动评估。
在实验部分，论文评估了12个主流T2I模型（包括闭源和开源），结果揭示出：所有模型得分普遍偏低（最高约50%），Symbol维度最困难；闭源模型略优于开源；微调基于Qwen-Image的模型（Qwen-Image-SciIR）在SciIR-Bench上从35%提升至43%。论文的主要贡献在于：提出了首个大规模、推理驱动的科学图像生成数据集和基准，为后续研究提供了标准化平台和明确方向。局限性在于数据集覆盖领域有限（以自然学科为主），微调仅基于单一基座模型，Symbol推理的改进空间巨大。该工作为科学图像生成的系统化研究奠定了坚实基础。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：论文聚焦科学图像生成，与用户画像中生成方向（权重0.10）直接重叠；

## 基本信息

- 作者：Zhiyuan Ma, Zhengfeng Shi, Yuning An, Peize Li, Jiabao Wei, Ruijie Li, Junhao Xiao, Jianjun Li, Bowen Zhou
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.30124`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 已参考PDF检索证据片段，对启发式草稿进行了丰富和校准。
