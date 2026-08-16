---
user_id: "cheng tan"
paper_id: 7410
arxiv_id: "2608.09292v1"
title: "Beyond the Capability Boundary: Zeroth-Order Optimization for Self-Evolving LLM Agents"
publish_date: "2026-08-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.09292v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.09292v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-12T01:22:31"
---
# Beyond the Capability Boundary: Zeroth-Order Optimization for Self-Evolving LLM Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：zeroth-order optimization · self-evolving agents · llm agents · lora perturbation

## 一句话总结

该论文提出一种零阶自进化框架（Zeroth-Order Self-Evolution），通过扰动 LLM 的 LoRA 参数估计梯度并更新参数，使智能体能够在无需轨迹标注的情况下，在困难样本上学习并突破自身能力边界，并在多个深度研究基准上获得更多成功轨迹和更优性能。

## 摘要

> Self-evolving methods improve the capabilities of LLM agents by sampling trajectories from the underlying LLMs and learning from these trajectories. However, these methods struggle to learn beyond the inherent capability boundary of the agents, since the agents cannot sample correct trajectories on difficult examples for further improvements. In this paper, we propose a zeroth-order self-evolution framework that enables agents to learn beyond their capability boundary by perturbing LLM parameters to adapt to difficult examples without any trajectory annotations. Specifically, we perturb LoRA parameters of LLMs, run the agent, compute the losses under the perturbed and original parameters, and use the loss difference to estimate gradients and further update the LoRA parameters. We sample trajectories using the updated LLMs for supervised fine-tuning to break through the capability boundary of the agents, forming a closed self-evolution loop. We introduce a parallel perturbation inference mechanism and an adaptive lookup mechanism to reduce time consumption in zeroth-order optimization, with an answer perplexity loss that provides smooth and stable zeroth-order loss values. Experiments on multiple deep research benchmarks show that our method obtains substantially more successful trajectories and consistently outperforms strong baselines, especially on difficult examples. The code and released artifacts are available at https://github.com/hidk1911/ZOForLLMAgents.

Q1: 这篇论文试图解决什么问题？

该论文试图解决的核心问题是：自我进化的 LLM 智能体难以超越其自身固有的能力边界。具体而言，自进化方法通常依赖从模型自身采样轨迹并进行学习，但当一个样本的难度超过智能体当前能力时，模型无法生成正确或可用的轨迹，导致没有可学习的正样本，从而陷入能力提升的瓶颈。这种能力边界的存在使得智能体无法通过自身生成的数据获得超过其分布外难度的知识。

传统解决思路包括：(1) 依赖外部标注或更强模型提供监督，但这引入了额外成本和标注瓶颈；(2) 使用更复杂的奖励筛选，但依然受限于采样分布。论文指出，根本问题在于采样轨迹的成功率在困难样本上趋近于零，因此无法为进化提供学习信号。

论文提出一种新的视角：不再要求智能体先能采样正确轨迹，而是直接调整模型参数以适配困难样本。通过零阶优化（Zeroth-Order Optimization, ZOO），在参数空间中进行扰动搜索，利用前后损失差得到梯度的近似估计，从而能在无标注轨迹的情况下更新模型，使其在困难样本上获得更高的回答概率。该方法本质上绕过了“先采样后学习”的依赖，将自进化从轨迹空间扩展到参数空间。

此外，论文还面临实际工程挑战：零阶优化通常需要大量前向传播，计算开销高。因此需要设计高效机制来降低时间消耗，并设计合适的损失函数以保证优化稳定性。

论文将问题定位在深度研究（Deep Research）任务上，这是一个复杂的多步信息检索与推理任务，对智能体的能力边界敏感，适合验证方法的有效性。

Q2: 有哪些相关研究？

相关研究主要分布在三个方向：

1. **LLM 智能体与深度研究任务**：论文引用了 Yao et al. (2023)、Schick et al. (2023) 和 Qin et al. (2024) 等工作，这些工作表明 LLM 智能体在复杂信息检索和推理任务中展现出强大潜力，例如 Deep Research 任务要求智能体进行多轮检索、信息整合和推理。深度研究任务是目前评估智能体复杂能力的重要基准。

2. **自我进化（Self-Evolving / Self-Improvement）方法**：自我进化方法通过从模型自身采样轨迹并用于训练来提升能力，例如自我训练、自蒸馏、迭代微调等。这类方法的核心假设是模型可以从自身输出中筛选高质量样本进行学习，但在困难样本上采样成功率低，导致无法突破能力边界。相关工作通常依赖奖励模型或外部筛选来过滤轨迹，但依然受限于采样分布。

3. **零阶优化（Zeroth-Order Optimization）**：零阶优化是一类不依赖梯度反向传播、仅通过函数值估计梯度的优化方法，在强化学习、黑盒优化中广泛应用。在 LLM 领域，已有工作尝试通过扰动参数或输入来估计梯度，但将其应用于智能体自我进化的研究较少。论文将零阶优化与 LoRA 参数扰动结合，是对该方向的新探索。

4. **LoRA 参数高效微调**：LoRA 通过低秩分解减少可训练参数，广泛用于 LLM 的轻量级适配。论文选择扰动 LoRA 参数而非全量参数，既降低了计算成本，又与常见的微调实践兼容。

5. **多模态智能体**：论文在局限部分提到未来的多模态扩展，并引用 Gao et al. (2025) 等，说明该领域已有相关进展。

需要注意的是，由于检索证据有限，相关工作的详细对比信息并不完整，上述内容部分是基于论文引用的合理推断。

Q3: 论文如何解决这个问题？

论文的核心方法是零阶自进化框架，具体技术路线如下：

1. **参数扰动与梯度估计**：对于每个困难样本，在 LLM 的 LoRA 参数上施加随机扰动（perturbation），得到扰动后的参数。分别在原始参数和扰动参数下运行智能体（或直接计算损失），获得两个损失值。利用零阶优化的经典技巧——损失差除以扰动幅度——来估计梯度方向，进而更新 LoRA 参数。这种方式不需要任何轨迹标注，只需要一个标量损失。

2. **损失设计：答案困惑度损失**：为了提供平滑且稳定的零阶损失值，论文设计了答案困惑度损失（answer perplexity loss）。具体做法是：在采样到一条推理轨迹后，以该轨迹为条件，计算参考答案的负对数似然，将其作为标量损失。相比稀疏的最终答案奖励，这种损失包含了轨迹信息，能提供更密集、更平滑的信号，有利于零阶梯度估计。

3. **闭环自进化流程**：更新 LoRA 参数后，使用更新后的 LLM 重新采样轨迹。由于参数已向困难样本适配，采样到正确轨迹的概率显著提升。然后，用这些成功轨迹进行监督微调（SFT），进一步强化模型能力。这就形成了一个闭环：零阶更新提升采样能力 → 收集成功轨迹 → SFT 强化 → 再次零阶更新，如此循环，逐步突破能力边界。

4. **并行扰动推断机制**：零阶优化需要多次前向计算，论文设计了并行扰动推断机制，将多个扰动参数的推理过程并行化，从而降低时间开销。合理推断这可能是通过 batched computation 或异步执行实现。

5. **自适应查找机制**：为了进一步减少时间消耗，论文引入自适应查找（adaptive lookup）机制。具体细节在证据中未明确，合理推测是动态调整扰动次数或查找合适的扰动尺度，以平衡估计精度和计算成本。

6. **与现有方法对比的差异**：传统自进化方法依赖采样成功轨迹，而本文方法直接优化参数以适应困难样本，再反过来促进轨迹采样，从而突破了能力边界。

由于检索证据主要来自摘要和引言，文中明确提到的技术细节仅包括上述机制；更具体的公式和超参数未提供，需查阅全文。

Q4: 论文做了哪些实验？

论文的实验设置和结果主要来自摘要和结论片段，具体信息有限。已知的实验要素包括：

1. **任务与基准**：使用了多个深度研究（Deep Research）基准。具体基准名称未在提供的证据中出现，需查阅原文。深度研究任务要求智能体进行多步骤检索、推理和答案生成。

2. **对比基线**：与强基线方法进行比较。摘要中未列出具体基线名称，但合理推断包括标准的自我进化方法、零阶优化变体或 SFT 基线。

3. **评测指标**：主要指标是成功轨迹的数量（即智能体在基准上完成任务的轨迹数），以及整体性能（可能包括准确率或任务完成率）。论文强调“在困难样本上”表现突出，说明实验对样本难度进行了分层分析。

4. **实验结果**：实验表明，所提出的方法获得了“显著更多”的成功轨迹，并“一致优于”强基线，尤其在困难样本上性能提升明显。这验证了方法能突破能力边界的核心主张。

5. **消融/机制分析**：从摘要看，提到了并行扰动推断机制和自适应查找机制用于降低时间消耗，但未见具体消融结果。答案困惑度损失的作用也未在证据中给出单独验证。

6. **资源与代码**：论文提供了代码和发布物链接（https://github.com/hidk1911/ZOForLLMAgents），便于复现。

由于证据不足，具体的实验数字、基准名称、基线详情和消融表均缺失，需要阅读全文获取。

Q5: 发现了什么实验现象？

根据现有证据，可提取的实验观察如下：

1. **方法在困难样本上表现突出**：论文明确提到“尤其在困难样本上”，表明该方法对能力边界附近样本的改进效果显著。这可能是因为零阶优化能从参数层面直接适配困难样本，而不是依赖采样。

2. **成功轨迹数量显著增加**：相比基线，方法获得了“substantially more successful trajectories”，这直接支持了突破能力边界的假设——模型通过参数更新后能够采样更多正确轨迹，进而形成良性循环。

3. **一致优于强基线**：在多个深度研究基准上，方法都优于基线，且这种优势是“consistently”的，说明方法具有跨基准的泛化性。

4. **可能存在的反直觉现象**：零阶优化通常被认为在高维参数空间中效率低，但论文通过 LoRA 参数扰动和高效机制使其在智能体进化中有效，这可能是一个反直觉的发现。另外，借助答案困惑度损失在无标注条件下也能获得平滑梯度，说明损失设计对零阶优化的稳定性至关重要。

5. **负结果或局限性**：论文在局限部分承认仅验证了 LLM 智能体，未扩展到多模态；这暗示在多模态场景下可能面临额外挑战。但未提供具体失败案例。

6. **指标间张力**：零阶优化可能引入额外的时间开销，论文通过并行扰动和自适应查找来缓解，但未见具体的时间对比数据。这可能是效率与性能之间的权衡。

这些观察中，前三点直接有摘要支持，后两点属于合理推断。具体数值、趋势和消融对比需要查阅原文。

Q6: 有什么可以进一步探索的点？

基于论文内容和局限性，可以提出以下进一步探索的方向：

1. **扩展到多模态智能体**：论文在局限中明确指出目前仅验证了 LLM 智能体，未来计划将其推广到多模态智能体（引用 Gao et al., 2025）。多模态场景下，轨迹形式更加多样，损失设计需要适应不同模态信号，零阶优化也可能面临更高的计算挑战。

2. **更复杂的智能体架构**：当前方法基于 LoRA 参数扰动，对于更大规模的模型或更复杂的 agent 架构（如使用外部工具、内存机制），零阶优化的有效性需要进一步验证。

3. **改进零阶优化效率**：虽然论文提出了并行扰动推断和自适应查找，但仍可探索更高级的零阶优化策略，如基于历史梯度的自适应步长、更优的扰动分布、或与贝叶斯优化结合。

4. **损失函数创新**：答案困惑度损失是一个起点，未来可以设计更细粒度的过程奖励或对比损失，进一步改善零阶损失的平滑性。

5. **能力边界的量化与分析**：论文从经验上展示了突破边界，但尚未提供理论分析。未来可以研究能力边界的几何性质，以及零阶优化在何种条件下能稳定突破边界。

6. **泛化到其他领域**：用户画像偏好生物科学应用。可以探索将方法用于科学发现任务中的智能体，例如分子设计、文献挖掘等，这些场景同样存在困难样本和轨迹标注稀缺的问题。

7. **闭环自进化的长期行为**：研究多次进化循环后的稳定性、遗忘问题以及样本利用效率。

8. **与强化学习的结合**：可以比较或融合基于策略梯度的强化学习与零阶自进化，探索哪种方式在何种资源限制下更优。

Q7: 总结一下论文的主要内容

本文研究 LLM 智能体的自我进化能力，指出传统自进化方法受限于能力边界——当任务难度超过智能体当前能力时，无法采样到正确轨迹，导致无法改进。针对这一问题，作者提出一种零阶自进化框架（Zeroth-Order Self-Evolution），核心思想是从参数空间入手，通过扰动 LoRA 参数并利用损失差估计梯度，直接更新模型参数以适配困难样本。

技术路线上，方法包括：对 LoRA 参数进行随机扰动，运行智能体得到扰动前后的损失；使用答案困惑度损失（以推理轨迹为条件计算参考答案的负对数似然）作为零阶优化的标量目标；通过并行扰动推断和自适应查找机制降低计算开销；更新参数后重新采样轨迹，并用成功轨迹进行监督微调，形成闭环进化。

在实验方面，论文在多个深度研究基准上验证了方法有效性，主要发现是：相比强基线，方法获得了显著更多的成功轨迹，尤其在困难样本上性能提升明显，且优势一致。这表明零阶优化能够使智能体突破固有边界，从参数层面获取原本无法采样到的学习信号。

论文还提供了代码仓库以便复现。整体上，本文的创新在于将零阶优化引入智能体自我进化，绕开了轨迹采样的限制，为提升智能体上限提供了新范式。但论文也存在局限：目前仅在 LLM 智能体上验证，未扩展到多模态；零阶优化的计算效率仍需进一步优化。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文核心方向为“agent”（权重 0.10），与用户画像中的智能体方向直接相关。

## 基本信息

- 作者：Bingzhen Liu, Xiaomeng Fan, Yuwei Wu, Zhi Gao, Mingyang Gao, Chuanhao Li, Yunde Jia
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.CL
- 日期：2026-08-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.09292v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（abstract、introduction、conclusion、limitations 片段），并结合启发式草稿进行了中文精读报告编写；部分细节因证据不足已标注为合理推断。
