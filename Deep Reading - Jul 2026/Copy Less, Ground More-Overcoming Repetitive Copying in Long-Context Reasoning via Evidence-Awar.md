---
user_id: "cheng tan"
paper_id: 4969
arxiv_id: "2607.19345"
title: "Copy Less, Ground More: Overcoming Repetitive Copying in Long-Context Reasoning via Evidence-Aware Reinforcement Learning"
publish_date: "2026-07-22"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.19345.pdf"
pdf_url: "https://arxiv.org/pdf/2607.19345"
abs_url: "https://arxiv.org/abs/2607.19345"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-22T13:54:23"
---
# Copy Less, Ground More: Overcoming Repetitive Copying in Long-Context Reasoning via Evidence-Aware Reinforcement Learning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：repetitive copying · long-context reasoning · reward shaping · evidence grounding

## 一句话总结

通过识别并解决长上下文推理中模型重复复制输入文本的失败模式，提出基于证据感知奖励塑形（GEAR）的强化学习方法，有效提升模型接地能力与推理准确性。

## 摘要

> Large language models that generate step-by-step reasoning traces have achieved strong performance on complex tasks, and extending them to long-context settings has emerged as an important frontier. However, we identify a critical failure mode in this regime: \emph{repetitive copying}, where models extensively copy text from the input into their reasoning traces rather than productively solving the problem. We show that this behavior is pervasive across frontier long-context LLMs and intensifies with context length. By separating each prompt into task-relevant key evidence and irrelevant distractor context, we further show that the root cause is insufficient grounding: models copy from the prompt indiscriminately, and those that fail to focus on key evidence are far more likely to answer incorrectly. Motivated by this diagnosis, we propose GEAR (Grounding Evidence-Aware Reward), a reward shaping method that augments the accuracy signal with a grounding reward for overlap with key evidence and a distractor penalty for overlap with irrelevant context. To enable GEAR on natural-language data, we develop an automated pipeline that constructs evidence-annotated training data from arbitrary documents. We validate GEAR across multiple model scales and benchmarks, showing consistent improvements of up to +4.6 average points over standard RL with accuracy-based rewards, with larger gains at longer contexts, while also reducing repetitive copying and thinking length. Our findings suggest that, even as long-context evaluation shifts from simple retrieval toward complex reasoning, accurate grounding in relevant evidence remains an indispensable capability with substantial room for improvement.

Q1: 这篇论文试图解决什么问题？

论文核心问题在于长上下文推理场景中，LLM生成逐步推理轨迹时出现“重复复制”失败模式：模型大量复制输入文本到推理轨迹中，而非有效解决问题。这种复制不仅浪费 token 且降低准确性，甚至在纯检索任务中导致答案无法生成。研究进一步揭示根本原因是模型对证据的接地不足——模型无差别地复制提示内容，而未能针对性关注关键证据；无法聚焦关键证据的模型更易答错。该问题在多个前沿长上下文LLM中普遍存在，并随上下文长度增加而恶化。

Q2: 有哪些相关研究？

相关工作涉及长上下文LLM的推理能力、提示压缩与检索增强、以及基于强化学习的推理优化。论文指出尽管长上下文评估已从简单检索转向复杂推理，但模型仍依赖复制机制，现有方法如Prompt Compression、RAG等虽尝试减少冗余但未直接针对推理中的复制行为进行强化学习优化。奖励塑形方面，GEAR区别于仅基于准确率的信号，引入了明确的接地奖励与干扰惩罚。

Q3: 论文如何解决这个问题？

提出GEAR（Grounding Evidence-Aware Reward）奖励塑形方法，在标准准确率信号基础上增加两个额外目标：(1) grounding reward，计算推理轨迹中与关键证据的重叠 n-gram 比例；(2) distractor penalty，惩罚与干扰上下文的重叠。奖励形式为 R = α·R_acc + β·R_ground − γ·R_distract。为在自然语言数据上启用GEAR，开发了自动数据标注流水线：从任意文档中构建证据标注的长上下文QA对，通过关键词提取和句子检索自动识别关键证据与干扰上下文。训练使用GSPO算法，在3.2k长上下文样本混合集上进行强化学习。

Q4: 论文做了哪些实验？

在7个前沿长上下文LLM（包括API和开源）上评估重复复制行为的普遍性。GEAR在多个模型规模（1.5B~7B）和5个留存基准上测试，包括合成任务（Ruler）和自然语言理解（LongBench-v2、ClinicalQA、SummScreen、DocQA）。训练数据混合了合成样本和自动标注的自然语言样本。对比基线包括无监督方法（如Prompt Compression）和基于准确率奖励的标准RL。消融实验验证了奖励权重和证据标注方式的影响。案例研究展示了GEAR在合成检索和真实理解任务中如何减少复制并提高推理质量。

Q5: 发现了什么实验现象？

重复复制行为在7个前沿长上下文LLM中普遍存在，且随上下文长度增加显著加剧。模型在长上下文中大量复制输入文本，不仅降低准确率还大幅增加推理长度。GEAR训练后模型重复复制比例下降，推理长度缩短，在5个基准上平均提升+4.6分，长上下文场景增益更明显。Grounding reward和distractor penalty分别贡献必要效果；仅靠准确率奖励无法减少复制。案例显示GEAR使模型从无差别复制转向选择性引用关键证据，在需要精确检索和综合理解的任务中均有效。

Q6: 有什么可以进一步探索的点？

未来工作可探索：更精细的证据边界定义（如多证据交叉引用）；将GEAR扩展到多步推理链中可能存在顺序依赖的情况；结合检索增强或结构化知识进一步减少复制；在更大规模模型和更长的上下文（超过128K）上验证；自动标注流水线的质量优化与领域迁移；以及将接地能力作为独立维度纳入长上下文模型评估体系。

Q7: 总结一下论文的主要内容

论文系统性地识别并解决了长上下文推理中大型语言模型（LLM）的重复复制失败模式。通过分析7个前沿长上下文LLM，发现这些模型在进行逐步推理时会大量复制输入文本到输出中，且该行为随上下文长度增加而加剧，导致准确率下降和推理轨迹膨胀。进一步实验表明，重复复制的根本原因是模型对证据的接地不足：模型无差别地复制提示内容，而无法主动聚焦与任务相关的关键证据。为此，论文提出GEAR（Grounding Evidence-Aware Reward）奖励塑形方法，在标准准确率奖励基础上引入接地奖励（鼓励推理轨迹与关键证据的n-gram重叠）和干扰惩罚（惩罚与无关上下文的重叠），通过强化学习直接训练模型减少复制、增强证据聚焦。为支持自然语言场景，论文开发了自动化证据标注流水线，能从任意文档生成带证据标注的长上下文QA对。实验采用GSPO（Generalized Self-Play Policy Optimization）算法在32K上下文长度的混合数据集上训练多个规模的模型（1.5B~7B），并在5个不同类型的长上下文基准（合成检索、文档理解、临床问答等）上评测。结果一致表明GEAR优于基于准确率的强化学习基线，平均提升+4.6个百分点，同时显著减少重复复制和推理长度。消融实验验证了每项奖励组件的必要性，案例研究显示了行为从“复制”到“接地”的转变。论文指出，即使长上下文评估已转向复杂推理，准确的证据接地仍然是不可或缺且可大幅提升的能力。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：紧扣长上下文推理中常见的抄袭问题，对研究LLM推理质量、提示压缩和检索增强具有直接参考价值。

## 基本信息

- 作者：Lizhe Fang, Weizhou Shen, Tianyi Tang, Yisen Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-07-22
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.19345`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据片段与field_evidence_map锚点，结合摘要、引言、结论及方法部分进行综合撰写，未超出证据覆盖范围。
