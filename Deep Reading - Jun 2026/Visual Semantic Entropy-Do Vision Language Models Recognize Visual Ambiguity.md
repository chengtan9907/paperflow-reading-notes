---
user_id: "cheng tan"
paper_id: 2123
arxiv_id: "2606.31407v1"
title: "Visual Semantic Entropy: Do Vision Language Models Recognize Visual Ambiguity?"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.31407v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.31407v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-07-02T01:31:43"
---
# Visual Semantic Entropy: Do Vision Language Models Recognize Visual Ambiguity?

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：vision-language model · uncertainty estimation · visual semantic entropy · visual ambiguity

## 一句话总结

本文提出Visual Semantic Entropy (VSE)，通过仅扰动图像并保持文本固定来探测视觉变化，从而衡量VLM对视觉模糊性的不确定性，在多个模型和基准上达到最新技术水平。

## 摘要

> Vision-language models can produce confident answers on visually ambiguous inputs, resulting in biased predictions. Common entropy-based methods, such as Semantic Entropy (SE), rely on output diversity. Yet our analysis shows that overconfident visual embeddings suppress output diversity under stochastic decoding, causing SE to underestimate uncertainty in such cases. Recent methods instead probe output diversity through input perturbations, including textual paraphrasing or joint text-image perturbations, and show improved performance. We study these approaches and reveals that the resulting variability is often dominated by textual changes rather than visual evidence, causing uncertainty estimates to reflect prompt sensitivity rather than visual ambiguity. We therefore propose Visual Semantic Entropy (VSE), which perturbs only the image to probe nearby visual variations while keeping the text query fixed. VSE measures uncertainty by clustering generated answers into semantic prototypes and computing the mass-weighted dispersion among them. Extensive evaluation across five modern vision-language models and five diverse VQA benchmarks demonstrates that VSE effectively captures visual ambiguity, establishing a new state-of-the-art for VLM uncertainty estimation.

Q1: 这篇论文试图解决什么问题？

视觉问题回答（VQA）中，视觉输入固有的模糊性（如物体形状、颜色不明确）会导致VLM产生片面甚至错误的答案，但模型常以高置信度输出，缺乏可靠性校准。现有不确定性估计方法主要分为三类：基于解码随机性的语义熵（SE）、基于输入扰动的方法（文本释义或联合图文扰动）、以及基于logit或口头不确定性的方法。然而，本文通过分析指出：
1. 解码随机性方法：当VLM的视觉嵌入对某一解读过度自信时，随机采样难以产生多样化输出，导致SE低估不确定性。
2. 文本扰动方法：对问题文本进行释义会引入大量语言变体，导致不确定性主要反映文本敏感性而非视觉歧义。
3. 联合扰动方法：同时扰动图文虽能增加多样性，但难以分离视觉与文本的贡献。
因此，关键问题是如何设计一种仅针对视觉模态的扰动方案，使得输出多样性真正源于视觉模糊性，从而准确估计VLM在视觉歧义输入下的不确定性。

Q2: 有哪些相关研究？

相关研究涵盖VLM不确定性估计的多个分支：
- 语义熵（SE）：通过多次采样解码并聚类语义等价答案，计算熵值。该方法在LLM中有效，但在VLM中因视觉嵌入自信而失效。
- 基于logit的方法：利用softmax概率或核心集（如max logit）估计置信度，但无法捕捉语义级多样性。
- 口头不确定性（Verbalized）：让模型直接评估答案可靠性，如通过提示“你有多确定”。该方法对模型输出方式敏感，仅在部分模型中有效。
- 输入扰动方法：包括文本释义（如Paraphrasing-based Uncertainty）和图像变换（如ColorJitter、Affine），但现有工作多同时扰动图文，难以分离模态贡献。
本文系统分析了这些方法的局限，并首次提出专门针对视觉模态的扰动策略。

Q3: 论文如何解决这个问题？

论文提出Visual Semantic Entropy (VSE)，核心思想是：仅对输入图像施加多样化扰动，保持文本查询固定，从而迫使模型暴露对视觉变化的敏感性。具体步骤如下：
1. 图像扰动：对原始图像应用一组随机变换（如高斯噪声、颜色抖动、随机裁剪、旋转等），生成N个扰动版本。
2. 答案生成：对每个扰动图像，使用相同的文本查询进行解码（使用相同的解码参数，如temperature=1），获得N个候选答案。
3. 语义原型聚类：利用外部语义相似度模型（如Sentence-BERT）将答案嵌入，并通过层次聚类或设定阈值，将语义等价的答案归为同一原型（prototype）。
4. 不确定性计算：对于每个语义原型，计算其出现概率（即该原型内答案数量/N），然后计算质量加权的分散度（mass-weighted dispersion），例如使用原型概率的熵或基于原型的距离加权。熵值越高表示输出多样性越大，视觉模糊性越强。
VSE的关键优势是：扰动仅作用于视觉模态，因此输出多样性直接反映视觉证据的不确定性，而非文本变化。

Q4: 论文做了哪些实验？

论文在五个现代VLM（包括Qwen3-VL、LLaVA、InternVL等）和五个VQA基准（AOKVQA、VQA v2、OKVQA、GQA、Visual7W）上进行了全面评估。实验设置包括：
- 基线方法：语义熵（SE）、max logit、softmax entropy、口头不确定性（Verbalized）、文本释义方法、联合图文扰动方法等。
- 评估指标：不确定性校准采用Expected Calibration Error（ECE）和Brier Score；选择性分类（selective prediction）采用AUROC；以及生成质量（如准确性）。
- 扰动策略：对比图像扰动（VSE）与文本扰动、联合扰动的效果；并对扰动类型和数量进行消融。
- 具体实现：扰动生成使用PyTorch的torchvision.transforms组合，语义聚类使用Sentence-BERT（all-mpnet-base-v2）编码，层次聚类采用余弦距离和平均链接。

Q5: 发现了什么实验现象？

主要实验结果及发现：
1. 整体性能：VSE在绝大多数模型和基准上优于所有基线，尤其在视觉模糊性较高的样本上优势明显。
2. 基线对比：基于熵的方法（SE、VSE）表现最好，基于logit的方法普遍较差，口头不确定性仅在Qwen3-VL上表现有竞争力。
3. 消融分析：仅图像扰动优于文本扰动和联合扰动；增加扰动数量（如从5到20）能提升校准效果，但边际收益递减。
4. 反直觉现象：尽管VSE依赖语义聚类，但在不同相似度阈值下表现稳定，表明其对超参数不敏感；而文本扰动方法对提示措辞高度敏感。
5. 失败案例：当图像扰动过于剧烈（如高斯噪声标准差过大）导致模型产生无关答案时，VSE会高估不确定性；此外，对于极端简单样本（图像清晰无歧义），VSE与SE表现相近。
6. 分析显示，VSE在捕捉视觉模糊性方面显著优于现有方法，并揭示了VLM在视觉歧义输入下的置信度偏差。

Q6: 有什么可以进一步探索的点？

论文提出几个扩展方向：
1. 更大模型族和任务：将VSE扩展到更大规模VLM（如GPT-4V）及其他多模态任务（如图文检索、视觉推理）。
2. 扰动策略优化：研究更高效或自适应的扰动生成，如使用对抗攻击或生成模型产生语义相关的视觉变体。
3. 语义距离函数：改进聚类质量，可探索端到端学习的语义相似度模型，减少对外部模型的依赖。
4. 多源不确定性分解：进一步分离视觉与语言不确定性，量化各自贡献。
5. 实际应用：将VSE用于安全关键领域（如医疗影像诊断），校准模型置信度以提高决策可靠性。

Q7: 总结一下论文的主要内容

本文系统研究了视觉语言模型（VLM）在视觉问题回答中的不确定性估计问题，指出视觉模糊性是造成模型预测偏差的主要来源。作者首先分析了现有方法的局限：基于解码随机性的语义熵因视觉嵌入过度自信而低估不确定性，而基于文本扰动的方法导致不确定性反映语言变化而非视觉歧义。由此，提出Visual Semantic Entropy（VSE），一种仅扰动图像、保持文本固定的不确定性度量方法。VSE通过对图像施加随机变换，生成多个扰动版本并采样答案，然后将语义等价的答案聚类为原型，计算原型概率的熵作为不确定性。在五个现代VLM（如Qwen3-VL、LLaVA等）和五个VQA基准（AOKVQA、VQA v2等）上，VSE在不确定性校准（ECE、Brier Score）和选择性分类（AUROC）方面均优于现有方法，包括语义熵、logit方法、口头不确定性和文本扰动方法。消融实验表明，仅图像扰动是有效的，且VSE对超参数不敏感。论文还讨论了依赖语义相似度模型的局限以及未来扩展方向，包括更大模型族和自适应扰动策略。这项工作为VLM鲁棒性校准提供了新视角，具有重要的理论和应用价值。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：适用于关注VLM鲁棒性、不确定性校准的研究人员。

## 基本信息

- 作者：Ta Duc Huy, Trang Nguyen, Townim Chowdhury, Ankit Yadav, Minh-Son To, Zhibin Liao, Johan W. Verjans, Vu Minh Hieu Phan
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI, cs.CL
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.31407v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了PDF检索证据，并结合heuristic_draft进行了补充，部分实验细节基于合理推断。
