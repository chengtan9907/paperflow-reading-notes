---
user_id: "cheng tan"
paper_id: 4070
arxiv_id: "2607.13527"
title: "VGIF-Score: Interpretable and Diagnostic Evaluation of Spatio-Temporal Instruction Following in Video Generation"
publish_date: "2026-07-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.13527.pdf"
pdf_url: "https://arxiv.org/pdf/2607.13527"
abs_url: "https://arxiv.org/abs/2607.13527"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-17T01:02:08"
---
# VGIF-Score: Interpretable and Diagnostic Evaluation of Spatio-Temporal Instruction Following in Video Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：video generation · evaluation · instruction following · spatio-temporal dependency

## 一句话总结

提出VGIF-Score，一个可解释的诊断框架，通过结合时空依赖图（ST-DAG）和自动评分（AutoRubric）来评估视频生成中的指令遵循。

## 摘要

> 本文提出VGIF-Score，一种可解释的诊断评估框架，用于评估视频生成模型是否准确遵循复杂的时空指令。该框架结合两个互补分支：基于时空依赖图（ST-DAG）的客观完成分支和基于指令条件化自动评分（AutoRubric）的主观满意度分支。客观分支将指令转化为依赖图并生成QA对，通过视觉语言模型（VLM）回答；主观分支评估整体感知效果。作者还构建了VGIF-Bench基准，包含200个结构复杂且经过人工验证的指令，覆盖多个场景。在多个最新视频生成模型上的实验表明，VGIF-Score不仅能给出评分，还能诊断具体哪些时空约束失败，揭示了现有模型在因果链、物体交互和时序依赖方面的缺陷。

Q1: 这篇论文试图解决什么问题？

现有视频生成评估方法主要关注视觉质量、时序一致性和整体文本-视频对齐，但忽略了对指令中复杂时空结构（如因果链、对象交互、时序顺序）的忠实执行。缺乏可解释的、诊断性的指标来帮助开发者定位具体失败模式。因此，需要一个能够分解指令结构并逐点检查的评估框架。

Q2: 有哪些相关研究？

相关研究包括：（1）视频生成评估指标，如FVD、CLIP相似度、用户研究等；（2）指令遵循评估，尤其在文本-图像领域有Davidson场景图等工作；（3）可解释评估方法，如基于场景图的指标。但尚无针对视频中时空结构化指令的系统评估。本工作填补了这个空白。

Q3: 论文如何解决这个问题？

VGIF-Score包含两个互补分支：
- 客观完成分支：首先将文本指令转换为时空依赖图（ST-DAG），节点表示对象、动作、关系，边表示时空依赖（如因果、顺序、共现）。然后基于ST-DAG自动生成一组是非问题（QA对），每个问题对应一个约束。利用VLM（如GPT-5）对生成视频回答这些问题，计算客观完成分数。
- 主观满意度分支：使用指令条件化的自动评分（AutoRubric），从整体感知上评估视频是否满足指令的意图和质量。
两个分支分数结合得到最终VGIF-Score。此外，作者构建VGIF-Bench，包含200个指令，覆盖八类时空依赖（如因果、顺序、共存、方向等），每个指令有详细标注和QA。

Q4: 论文做了哪些实验？

实验设计：使用VGIF-Bench评估多个当前先进的视频生成模型，包括Kling-V3、CogVideoX-1.5、HunyuanVideo等。计算每个模型的VGIF-Score及其子分数。进行诊断分析，展示各个模型在不同类型约束上的表现。还进行了与现有指标（如CLIP、FVD）的比较，验证相关性。

Q5: 发现了什么实验现象？

关键观察：（1）现有模型在复杂时空指令上表现不佳，尤其涉及因果链和间接触发事件时；（2）模型在简单对象存在性上几乎完美，但在时序顺序和交互约束上经常失败；（3）不同模型有不同的失败模式，例如Kling-V3在时空依赖上表现较好，但CogVideoX-1.5在某些因果链上失败；（4）VGIF-Score的细粒度诊断能够定位具体失败的约束，如“钥匙穿过环”可能成功但“雾形成环”可能失败；（5）与人类判断具有较高一致性。

Q6: 有什么可以进一步探索的点？

（1）将VGIF-Score扩展到更长的视频和更复杂的场景；（2）用于指导模型训练，如基于诊断设计损失；（3）研究自动ST-DAG构建的改进，减少对GPT的依赖；（4）结合人类反馈进一步校准；（5）应用到其他模态如3D生成。

Q7: 总结一下论文的主要内容

本文针对视频生成评估中缺乏可解释性和诊断能力的问题，提出了VGIF-Score框架。该框架通过时空依赖图（ST-DAG）将指令分解为可验证的约束，并结合客观完成和主观满意度两个分支进行综合评估。同时构建了VGIF-Bench基准，包含200个精心设计的时空指令，覆盖多种依赖类型。实验在多个主流模型上进行，展示了框架的有效性和诊断价值。主要贡献包括：提出创新的评估框架、构建新基准、提供细粒度诊断分析。局限性包括依赖VLM可能引入偏差、基准规模有限等。整体而言，该工作为视频生成评估提供了新视角和实用工具。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：提出可解释评估方法，对视频生成研究有直接帮助

## 基本信息

- 作者：Songyu Xu, Xin Wang, Qiang Chen, Xinran Wang, Muxi Diao, Yuxuan Zhang, Kongming Liang, Rui Lin, Zhanyu Ma
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-16
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.13527`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据（retrieved_evidence）和heuristic_draft，部分内容基于合理推断。evidence:true
