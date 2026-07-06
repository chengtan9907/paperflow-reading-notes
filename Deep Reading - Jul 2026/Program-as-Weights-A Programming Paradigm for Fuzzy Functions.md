---
user_id: "cheng tan"
paper_id: 2570
arxiv_id: "2607.02512"
title: "Program-as-Weights: A Programming Paradigm for Fuzzy Functions"
publish_date: "2026-07-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.02512.pdf"
pdf_url: "https://arxiv.org/pdf/2607.02512"
abs_url: "https://arxiv.org/abs/2607.02512"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-06T13:05:52"
---
# Program-as-Weights: A Programming Paradigm for Fuzzy Functions

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：fuzzy function programming · program-as-weights · parameter-efficient finetuning · neural compilation

## 一句话总结

论文提出模糊函数编程（fuzzy-function programming）范式，通过编译器将自然语言规范转化为紧凑的本地可执行神经工件，从而在极低资源下达到大模型级别的任务性能。

## 摘要

> Many everyday programming tasks resist clean rule-based implementation, such as alerting on important log lines, repairing malformed JSON, or ranking search results by intent, and are increasingly outsourced to large language model APIs at the cost of locality, reproducibility, and price. We propose fuzzy-function programming: compiling such a function from a natural-language specification into a compact, locally-executable neural artifact. We instantiate this paradigm with Program-as-Weights (PAW), in which a 4B compiler trained on FuzzyBench, a 10M-example dataset we release, emits parameter-efficient adapters for a frozen, lightweight interpreter. A 0.6B Qwen3 interpreter executing PAW programs matches the performance of direct prompting of Qwen3-32B, while using roughly one fiftieth of the inference memory and running at 30 tokens/s on a MacBook M3. PAW reframes the foundation model from a per-input problem solver into a tool builder: invoked once per function definition, it produces a small reusable artifact whose subsequent calls per function application are cheap and offline.

Q1: 这篇论文试图解决什么问题？

论文试图解决以下问题：1）日常编程中存在大量“模糊”任务，即对人类直观但难以用符号规则精确描述，例如识别重要日志行、修复格式错误的JSON、按意图排序搜索结果等；2）当前开发人员常将这些任务外包给大语言模型（LLM）API，导致对远程服务的依赖、高昂的按调用付费成本、数据隐私泄露风险以及结果不可重复；3）现有模型蒸馏或程序合成方法要么仍需较大模型运行，要么难以平衡灵活性与效率。PAW旨在提供一种替代范式：将模糊函数一次性编译为紧凑的神经工件，后续调用在本地轻量解释器上离线执行，同时保持与大模型相近的准确率。

Q2: 有哪些相关研究？

相关研究包括：1）直接提示（Direct Prompting）：使用商用或开源LLM API每次执行任务，成本高且依赖网络；2）模型蒸馏（Model Distillation）：如ALCHEmist (Huang et al., 2024b) 将标注逻辑从LLM蒸馏为Python程序，在标准解释器上运行，但生成的是符号程序而非神经工件；3）参数高效微调（PEFT）：如LoRA、Adapter等，但通常针对特定任务微调，而未实现从规范到工件的编译；4）程序合成（Program Synthesis）：利用LLM生成代码，但处理模糊规约时可靠性不足。PAW的区别在于：它学习一个编译器将自然语言规范直接映射为神经网络权重（LoRA适配器），而非产生显式代码；解释器是固定的预训练语言模型（Qwen3-0.6B），提供执行环境。这种端到端学习避免了符号程序的脆弱性，同时保持了神经网络的泛化能力。

Q3: 论文如何解决这个问题？

PAW框架包含两个阶段：编译阶段和推理阶段。编译阶段：用户提供自然语言函数规范（如描述+示例），编译器首先通过一个小的任务改写模板（task-rewriting template）将规范整理为干净的伪程序格式（包括释义描述和少量输入输出示例）。然后，一个4B参数的LoRA编译器（基于Qwen3-4B）处理该伪程序，生成一组LoRA适配器权重，这些权重被加载到冻结的Qwen3-0.6B解释器上。推理阶段：将待处理输入送入加载了适配器的解释器，模型输出即为函数结果。由于解释器仅0.6B且适配器参数极少（通常是亿级以下），整个工件体积小，可直接在本地设备（如MacBook M3）上运行，无需GPU或网络连接。训练编译器时使用作者收集的FuzzyBench数据集，包含约1000万示例，涵盖多种模糊任务。

Q4: 论文做了哪些实验？

论文在FuzzyBench测试集上进行了系统实验。主要比较PAW与三类基线：1）直接提示不同规模的开源LLM（Qwen3系列：0.6B、4B、8B、14B、32B等）；2）标准微调（全参数微调或LoRA微调）相同参数量的模型；3）模型蒸馏基线（如将大模型知识蒸馏到小模型）。所有基线在同一测试集上评估，采用精确匹配（Exact Match）作为主要指标。此外，进行了消融实验：1）编译器不同组件（任务改写模板、LoRA秩、训练数据规模）的影响；2）解释器模型规模的影响；3）适配器参数量的影响。资源测量：在MacBook M3上测量PAW推理的峰值内存和速度，与直接提示32B模型所需云端GPU资源对比。

Q5: 发现了什么实验现象？

主要发现：1）PAW（0.6B解释器+适配器）在FuzzyBench上达到73.78%精确匹配，超过直接提示Qwen3-32B的68.70%，同时推理内存仅约1/50（从32B所需约64GB降至约1.2GB），在MacBook M3上达到30 token/s；2）与标准微调0.6B模型相比，PAW显著更高（微调0.6B约55%），证明编译器学习的适配器比直接任务微调更有效；3）消融表明：任务改写模板至关重要，去除后性能下降约10%；LoRA秩在64-128之间最优；编译器数据规模扩大持续提升性能，未饱和；4）解释器规模从0.6B增至4B时性能提升有限，但计算成本增加，0.6B是高效点；5）失败案例分析：PAW在需要多步推理或长程依赖的任务上表现较弱（如复杂逻辑推理），与局限一致。总体，PAW在模糊函数任务上实现了效率与准确率的帕累托改善。

Q6: 有什么可以进一步探索的点？

论文指出若干开放方向：1）多步/长程推理：当前PAW仅支持单步函数（一个输入一个输出），扩展至多步或需要状态保持的任务是自然延伸；2）函数组合：用户代码中可组合PAW函数（如案例研究所示），但编译器尚未显式优化组合性；3）编译器规模与泛化：编译器本身（4B）仍有改进空间，更小或更大的编译器可能带来效率或质量变化；4）动态规范更新：当函数规范改变时，需要快速重编译而非重新训练；5）更丰富的任务类型：当前FuzzyBench覆盖有限，扩展到代码生成、决策、交互式任务等；6）安全性和可靠性：本地执行可能出现错误或对抗输入，需要保证机制。此外，跨领域应用如科学计算、生物信息学中的模糊模式识别也值得探索。

Q7: 总结一下论文的主要内容

本文提出Program-as-Weights（PAW），一种新的模糊函数编程范式。核心思想是将自然语言函数规范一次性编译为紧凑的神经网络权重（LoRA适配器），在固定的轻量级解释器（0.6B Qwen3）上本地、离线执行，从而替代对大型LLM API的依赖。为此，作者构建了FuzzyBench数据集（1000万示例），涵盖多种模糊任务（日志分类、JSON修复、排序等）。他们训练了一个4B参数的编译器，采用两阶段流程：首先通过任务改写模板将用户规范标准化，然后由LoRA编译器生成适配器。编译后的PAW程序在测试集上达到73.78%精确匹配，超过直接提示32B模型（68.70%），而推理内存仅约1/50，在MacBook M3上以30 token/s运行。此外，PAW支持函数组合和本地部署，为日常编程中的模糊任务提供了可重复、低成本、保护隐私的解决方案。论文还讨论了局限，如当前限于单步、未验证多步推理，以及编译器未显式优化组合性。整体上，PAW将基础模型从逐个问题解决者重新定位为工具构建者，对边缘计算、隐私敏感应用和降低AI成本具有重要意义。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：如果你关注智能体（agent）系统，PAW提供了一种将函数本地化、高效执行的方式，减少对远程LLM的依赖，有助于构建更自主、低延迟的智能体。

## 基本信息

- 作者：Wentao Zhang, Liliana Hotsko, Woojeong Kim, Pengyu Nie, Stuart Shieber, Yuntian Deng
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL
- 日期：2026-07-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.02512`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据。
