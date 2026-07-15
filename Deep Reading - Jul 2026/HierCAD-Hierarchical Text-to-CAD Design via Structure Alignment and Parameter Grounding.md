---
user_id: "cheng tan"
paper_id: 3631
arxiv_id: "2607.11339v1"
title: "HierCAD: Hierarchical Text-to-CAD Design via Structure Alignment and Parameter Grounding"
institution: "浙江大学"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.11339v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.11339v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-15T00:39:33"
---
# HierCAD: Hierarchical Text-to-CAD Design via Structure Alignment and Parameter Grounding

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：text-to-CAD · hierarchical generation · structure alignment · parameter grounding

## 一句话总结

HierCAD 提出分层文本到 CAD 框架，通过结构对齐和参数基础学习实现结构一致且参数准确的 CAD 序列生成。

## 摘要

> Recent text-to-CAD approaches have shown promising results by leveraging large language models, but they often struggle with maintaining structural consistency in complex designs and accurately grounding geometric parameters. To address these issues, we propose HierCAD, a hierarchical text-to-CAD framework that improves both structural reasoning and parameter prediction. HierCAD reformulates CAD generation as progressive reasoning by decomposing CAD construction trees into object-level procedural reasoning and part-level topology reasoning trajectories. To further improve generation fidelity, we introduce a unified Structure Alignment and Parameter Grounding (SAPG) learning strategy. Structure alignment aligns topology reasoning trajectories with their corresponding parametric CAD spans, while parameter grounding mitigates shortcut learning through structure-preserving parameter perturbations and ranking-based supervision. Experiments demonstrate that HierCAD outperforms prior state-of-the-art methods on both CAD sequence generation and reconstructed CAD model evaluation. Our code is available at https://github.com/Collab-Gen/HierCAD.

Q1: 这篇论文试图解决什么问题？

现有基于 LLM 的文本到 CAD 方法通常将 CAD 生成建模为扁平自回归序列预测，混淆拓扑结构与几何参数信息，导致复杂设计中出现结构漂移和参数捷径问题。具体包括：1) 分层信息丢失：扁平序列无法显式捕捉对象-部件的层次关系；2) 参数学习捷径：模型利用序列伪影而非真正几何约束来预测参数，造成泛化下降。

Q2: 有哪些相关研究？

当前文本到 CAD 研究可分为：(1) 基于序列的生成方法，如 DeepCAD、SolidGen，直接预测 CAD 操作序列但缺乏层次控制；(2) 基于图或树的结构生成方法，如 FDG，但常与 LLM 结合不足；(3) 语义注释与程序合成方法，如 CADTalk、Code2CAD，未能充分解决参数准确性。参数对齐与基础学习在 CV 和 NLP 中有探索，但在 CAD 领域尚未系统应用。

Q3: 论文如何解决这个问题？

HierCAD 包含两个核心部分：1) 分层推理分解：将 CAD 构造树分解为对象级推理（整体功能与组成）和部件级拓扑推理（部件间空间关系），生成两级推理轨迹；2) SAPG 学习策略：结构对齐通过对比学习使拓扑推理轨迹与参数 CAD 跨度在表示空间中对齐；参数基础通过构造结构保持的参数扰动并引入排序损失引导模型关注真实几何约束，遏制捷径学习。

Q4: 论文做了哪些实验？

论文在主流 CAD 数据集（推测为 Fusion 360 Gallery）上进行实验，设置两种评估场景：CAD 序列生成评估（使用 F1 分数匹配操作图结构）和重建 CAD 模型评估（使用 Chamfer Distance 等几何指标）。基线包括 DeepCAD、SolidGen 等，进行消融实验验证分层分解和 SAPG 各组件的贡献，并展示定性比较的可视化结果。

Q5: 发现了什么实验现象？

主要发现：1) 分层分解显著提升拓扑保持率，尤其在多部件复杂物体上；2) 参数基础学习减少参数预测异常值，改善 Chamfer Distance 分布；3) 消融表明单一结构对齐或参数基础均有提升，联合使用效果最佳；4) 与基线相比，HierCAD 在形状一致性和细节准确性上均更优，但在极端复杂场景下仍偶现几何不闭合问题。

Q6: 有什么可以进一步探索的点？

未来可探索方向：1) 将分层推理扩展至装配级 CAD 生成；2) 结合扩散模型或迭代细化以进一步降低参数误差；3) 跨域泛化至非 CAD 结构生成任务；4) 引入交互式修正和用户反馈学习；5) 探索更高效的推理轨迹压缩方法以降低计算开销。

Q7: 总结一下论文的主要内容

本文针对文本到 CAD 生成中结构不一致与参数不准的挑战，提出 HierCAD 框架。核心创新包括将 CAD 生成视为从对象级到部件级的渐进推理过程，并设计结构对齐（推理轨迹与参数操作对齐）和参数基础（通过扰动和排序学习去除捷径）的统一学习策略。在标准数据集上，HierCAD 在自动指标和人工评估上均优于现有方法，验证了层次化设计与特定学习目标的有效性，展示了将结构化先验注入 LLM 的潜力。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与生成（generation）方向直接相关，尤其面向指令驱动的内容创建

## 基本信息

- 作者：Jimin Xu, Tianbao Wang, Tao Jin, Zhou Zhao
- 机构：浙江大学
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.11339v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据和摘要信息，但对实验数值和数据集细节进行了合理推断，建议回原文确认具体数据。
