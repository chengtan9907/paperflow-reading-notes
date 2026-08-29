---
user_id: "cheng tan"
paper_id: 9256
arxiv_id: "2608.24953v1"
title: "Beyond Tokens: Probing Higher-Order Epistasis in Learned Protein Representations"
publish_date: "2026-08-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.24953v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.24953v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:19:51"
---
# Beyond Tokens: Probing Higher-Order Epistasis in Learned Protein Representations

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：protein fitness landscape · epistasis · higher-order interactions · representation analysis

## 一句话总结

本文提出 ORBIT，一个按阶次解析蛋白质习得表示中上位性交互结构的基准框架，在 GB1 适应度景观上发现 RIT 显著提升 token 阶段二阶可访问性，但并未转化为下游高阶功能恢复优势。

## 摘要

> Protein fitness landscapes contain nonlinear interactions in which the effect of one mutation depends on the identities of other residues. Although learning systems can predict protein fitness, predictive performance alone does not reveal how experimentally measured higher-order structure is organized within their internal representations.
> We introduce ORBIT, an Order-Resolved Benchmarking of Interaction Transformations framework that distinguishes interaction presence, representation accessibility, and functional recovery. ORBIT first validates its Walsh-based diagnostics on synthetic landscapes with known interaction order and then analyzes the experimentally measured GB1 fitness landscape under the biologically motivated FLIP 2-vs-rest generalization setting. We compare ridge regression, a standard MLP, independent token representations, nonlinear independent tokens, and Residual Interaction Tokenization (RIT).
> In the primary two-hidden-layer comparison across 20 stochastic training seeds, planned seed-paired inference found no architecture differences in FLIP test $R^{2}$ , strict third- or fourth-order functional recovery, or final-hidden-layer third- or fourth-order accessibility. In contrast, RIT significantly increased pairwise accessibility directly at the token stage relative to both independent-token controls ( $\Delta A_{tok,2} = 0.2468$ , $d_{z} = 1.67$ , Holm-adjusted $p = 1.14 \times 10^{-5}$ ), without a detectable downstream higher-order advantage.
> A pre-specified depth/capacity sensitivity analysis then increased the backbone from two to three or four hidden layers. Deeper MLPs showed significant gains in FLIP prediction, third-order functional recovery, and final-layer third-order accessibility. Fourth-order accessibility also improved relative to the shallow MLP, although its absolute held-out $R^{2}$ remained below zero. Independent tokens showed a smaller prediction gain at depth four, whereas nonlinear tokens and RIT showed no Holm-significant depth effects on the confirmatory endpoints. Because the deeper models also contain additional parameters, these results are interpreted as depth/capacity sensitivity rather than a parameter-matched causal effect of depth.
> ORBIT therefore reveals representation-level changes that are not apparent from conventional prediction metrics alone and separates early interaction-aware tokenization effects from interaction structure that can be constructed by downstream nonlinear capacity.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：在蛋白质适应度预测中，模型的外部预测性能无法揭示其内部表示如何组织实验测得的高阶上位性（higher-order epistasis）结构。具体分为三个层面：

1. **存在性 vs. 可访问性 vs. 功能恢复的混淆**：现有工作大多直接以最终预测指标（如 R²）代表模型对交互结构的捕捉能力，但“数据中存在三阶交互”“模型表示中可访问三阶交互”“模型能利用三阶交互改善预测”是三个不同的问题。ORBIT 的目标是将它们拆开并分别量化。

2. **缺少统一的阶次解析诊断协议**：Walsh 变换等谱工具常用于分析适应度景观本身，但将类似工具转为表示层的诊断、并在 token 阶段与隐藏层阶段同时追踪一至四阶交互，仍缺少标准化框架。

3. **架构比较的统计严谨性不足**：许多方法比较只报告点估计，忽略随机种子造成的方差；本文通过预注册式的种子配对推断（seed-paired inference）来区分真正的架构差异与随机波动。

4. **高阶泛化困境**：在只有野生型、单突变和双突变训练数据（FLIP 2-vs-rest）时，模型能否外推到三突变和四突变？即使预测指标尚可，内部表示是否真的编码了这些高阶交互？论文直接以实验问题形式提出这一点。

从更广的视角看，这是可解释性/机制发现（mechanistic interpretability）与蛋白质工程预测之间的交叉问题：我们不仅想要一个能打分的黑箱，还想知道它“知道什么”以及“在什么地方知道”高阶非线性关系。

Q2: 有哪些相关研究？

根据检索到的片段，论文的 related work 部分讨论了几个关键点：

- **预测性能并不等于机制恢复**：文中明确写道，“强成对预测并不需要意味着恢复底层的协作机制”，而且“高阶结构可能需要不同的建模和评估目标”。这暗示相关研究中有大量工作仅凭预测精度断言模型学到了上位性，忽略了内部表示。

- **表示轨迹视角**：ORBIT 被定位为“沿表示轨迹本身前进”，将 token 阶段的可访问性与隐藏层可访问性、最终功能恢复区分开。这与可解释性研究中的 probing（探针）和 concept attribution 有交集，但更强调阶次分解。

- **蛋白质适应度景观的谱几何**：片段提到 “Spectral geometry of protein landscapes”，说明相关工作使用谱方法（如 Walsh-Hadamard 变换、傅里叶分析）刻画适应度景观的几何结构。ORBIT 在此基础上扩展到习得表示。

- **互补而非替代**：相关工作可能聚焦于“恢复相互作用的调控因子集合”（如从数据中推断上位性网络），而 ORBIT 解决的是表示层面的追踪问题，即“在模型的哪一层、以何种阶次出现交互结构”。

总体而言，相关工作横跨蛋白质适应度预测、表示可解释性、谱分析和泛化研究。由于证据有限，无法确定具体引用的文献，但可合理推断该领域包括 FLIP 基准、GB1 深突变扫描数据集，以及各类 tokenization 策略（如独立 token 和交互 tokenization）的对比。

Q3: 论文如何解决这个问题？

ORBIT 的解决方案可以归纳为“一个框架、三个区分、两种探针、一套统计设计”：

1. **框架三要素**：
 - **Interaction presence（交互存在性）**：在数据或任务中是否存在真实的 k 阶交互（通过合成景观的 ground truth 验证）。
 - **Representation accessibility（表示可访问性）**：在模型的 token 层或隐藏层中，能否从表示中解码出 k 阶交互信息。
 - **Functional recovery（功能恢复）**：模型输出（适应度预测）是否成功利用了 k 阶交互来改善预测。

2. **Walsh 基础诊断**：利用 Walsh 变换对离散序列（氨基酸序列）进行正交分解，将交互按阶次（一阶、二阶、三阶、四阶）分离。ORBIT 推导出表示层的 Walsh 诊断，将表示矩阵投影到交互子空间中，衡量各阶交互的“可访问性” A_k。该诊断先在合成景观上验证——合成景观具有已知的一至四阶结构、精确的四阶奇偶性以及受控的阶次过渡，用以确认诊断能正确恢复阶次。

3. **AA-identity-strict 探针协议**：传统的线性探针可能利用野生型氨基酸身份泄漏信息。ORBIT 采用“氨基酸身份严格”策略，在非野生型（held-out non-WT）氨基酸身份上评估表示可访问性，确保测到的交互泛化到新的氨基酸组合，而非记忆特定身份。

4. **随机性量化**：使用 20 个随机种子训练每个配置，并通过“种子配对推断”（seed-paired inference）做统计比较，计算效应量（d_z）和 Holm 校正 p 值。这提高了小样本比较的稳健性。

5. **架构与 tokenization 对照**：
 - 岭回归（ridge regression）作为线性基线；
 - 标准 MLP；
 - 独立 token 表示（independent tokens）；
 - 非线性独立 token（nonlinear independent tokens）；
 - Residual Interaction Tokenization (RIT)，一种显式建模残基间交互的 tokenization 方法。

6. **深度/容量敏感性分析**：将主干网络从两层扩展到三层、四层隐藏层，观察预测性能和各阶可访问性如何随深度变化。这用于回答“更多容量是否促进高阶结构涌现”。

整体上，ORBIT 通过合成数据确立诊断有效性，再在真实的 GB1 深突变扫描数据（FLIP 2-vs-rest 设置）上比较不同表示学习策略，从而追踪交互结构在表示中的出现位置与阶次。

Q4: 论文做了哪些实验？

论文的实验设计围绕三个预先设定的问题展开（由检索到的 Experimental Questions 部分支持）：

- Q1: ORBIT 能否正确恢复已知的交互阶次？为此在合成景观上进行验证，包括：已知一至四阶结构的景观、精确的四阶奇偶性景观、以及受控的阶次过渡（如从二阶过渡到三阶）景观。

- Q2: 在只训练野生型、单突变和双突变的数据上，模型能否在实验测量的三突变和四突变上恢复功能？这是在 GB1 蛋白的 FLIP 2-vs-rest 泛化设置下进行的真实数据实验。

- Q3: 一至四阶交互在表示的哪个阶段（token 阶段或隐藏层阶段）变得可访问？

具体实验安排如下：

1. **主比较（两隐藏层）**：在相同的两隐藏层主干下，比较岭回归、MLP、独立 token、非线性独立 token 和 RIT。每种配置训练 20 个随机种子，使用种子配对推断（seed-paired inference）比较 FLIP 测试 R²、严格三阶/四阶功能恢复，以及最终隐藏层的三阶/四阶可访问性。同时报告 token 阶段的二阶可访问性 A_tok,2，作为 RIT 的预先指定关注点。

2. **深度/容量敏感性分析**：将 MLP 和 token 方法的主干深度从 2 层增加到 3 层和 4 层，重新评估上述指标。特别关注四阶绝对 held-out R² 是否仍为负，以及不同方法对深度的响应差异。

3. **统计协议**：所有比较都使用 Holm 校正控制多重比较误差，并报告标准化效应量 d_z。合成景观验证作为诊断的对照实验，防止在真实数据上的解释产生误导。

由于检索到的内容不包含所有细节，具体的训练超参数、数据划分和 Walsh 诊断实现细节需阅读原文方法部分确认。

Q5: 发现了什么实验现象？

基于摘要和检索片段，实验观察到以下主要现象：

1. **主比较（两层）中架构间几乎无差异**：在 20 个随机种子的预配对推断下，所有架构在 FLIP 测试 R²、严格三阶/四阶功能恢复、最终隐藏层三阶/四阶可访问性上都没有显著差异。这提示：在相同的浅层容量下，不同的 tokenization 策略并不带来高阶交互的总体优势。

2. **RIT 显著提升 token 阶段的二阶可访问性**：RIT 相对两个独立 token 对照，ΔA_tok,2 = 0.2468，效应量 d_z = 1.67，Holm 校正后 p = 1.14×10⁻⁵。即 RIT 在 token 表示层面确实显式地编码了更多成对交互信息。

3. **可访问性提升未转化为功能优势**：尽管 RIT 在 token 阶段的二阶可访问性显著提高，但下游（分类头/输出）并没有检测到高阶功能恢复或最终层高阶可访问性的优势。这说明“表示中存在”与“表示被使用”之间可能存在鸿沟，或者训练目标（预测适应度）并不需要显式利用这些二阶信息来达到当前精度。

4. **深度增加对 MLP 有利，但四阶仍未突破**：当主干加深到三层或四层时，MLP 在 FLIP 预测、三阶功能恢复和最终层三阶可访问性上有显著提升。四阶可访问性也相对浅层 MLP 提升，但绝对 held-out R² 仍低于零——这意味四阶预测在留出数据上依然是负相关的，深度不足以解决四阶外推难题。

5. **不同 token 方法的深度响应不同**：独立 token 在深度四时只有较小的预测增益，非线性独立 token 和 RIT 没有（摘要截断，推测为没有显著增益或增益更小）。这表明深度带来的收益主要体现在 MLP 的连续非线性变换上，而不是 token 化的输入表示。

6. **负结果的价值**：论文明确报告了“无显著差异”和“绝对 R² 为负”等负结果，这挑战了“更大容量总是更好”和“显式交互 tokenization 必然有益”的直觉。

Q6: 有什么可以进一步探索的点？

基于现有证据和论文的开放问题，可以进一步探索的方向包括：

1. **RIT 二阶增益为何不传导到高阶功能恢复**：设计干预实验，例如人为放大 RIT 的三阶/四阶 token 交互，观察功能恢复是否随之变化；或使用专门的高阶监督信号（如直接以 Walsh 系数作为辅助损失）训练。

2. **更深模型和预训练表示的结合**：论文的深度敏感性只到四层 MLP；可以将 ORBIT 应用于预训练蛋白质语言模型（如 ESM-2 的中间层 / 注意力头），追踪更高容量模型中高阶交互的涌现位置。

3. **扩展 ORBIT 到其他蛋白和数据集**：GB1 单蛋白验证后，可在 P450、PhoQ、溶菌酶等深突变扫描数据上检验“二阶 token 可访问性提升但无高阶功能优势”的模式是否普遍。

4. **动态训练轨迹分析**：目前的 ORBIT 是静态快照式分析；可研究训练过程中各阶可访问性的演化时间线，观察高阶交互是逐渐涌现还是突变出现。

5. **更细粒度的位置和组合解释**：除了整体阶次，可以分析哪些残基对/三元组被编码在 token 表示中，并与实验测得的上位性网络做对比，从而识别表示中的“虚假交互”或“缺失交互”。

6. **统计功效的改进**：20 个种子虽做了种子配对，但高阶指标方差可能很大；可以增加种子数量或使用更敏感的效应量指标。

7. **理论分析**：从函数逼近角度解释为何独立 token 在深度四时收益有限，而 MLP 收益显著；是否可以刻画“深度增加促进三阶但不足以促进四阶”的理论上限。

8. **其他表示诊断的融合**：将 ORBIT 与归因方法（如积分梯度、注意力模式）结合，建立从“可访问”到“实际被下游使用”的因果链路。

Q7: 总结一下论文的主要内容

本文聚焦于一个被主流蛋白质适应度预测研究忽略的问题：模型内部表示究竟如何组织高阶上位性结构。作者指出，预测性能（如 FLIP 测试 R²）仅代表黑箱的外部行为，并不告知我们表示中是否、以及在何处编码了二阶、三阶、四阶的交互信息。为此，他们提出了 ORBIT（Order-Resolved Benchmarking of Interaction Transformations）框架，将“交互存在”（数据层面）、“表示可访问”（表示层面）和“功能恢复”（预测层面）三个概念分离，并基于 Walsh 变换构造了阶次解析的诊断工具。

技术路线上，ORBIT 首先在合成景观上验证诊断的有效性。这些合成景观具有已知的一至四阶交互结构，包括精确的四阶奇偶性和受控的阶次过渡，用来回答 Q1（能否正确恢复已知阶次）。随后在真实的 GB1 深突变扫描数据上，采用 FLIP 2-vs-rest 泛化设置（训练只包含野生型、单突变和双突变，测试三突变和四突变），回答 Q2（功能能否外推到高阶突变）和 Q3（高阶交互在表示的哪一阶段可访问）。模型比较涵盖了岭回归、标准 MLP、独立 token 表示、非线性独立 token 和残差交互 tokenization（RIT）。为防止随机种子造成的误判，所有比较使用 20 个随机种子和种子配对推断，并报告 Holm 校正 p 值与效应量。

实验主线有两个层次。第一层是固定两隐藏层的主比较：结果显示所有方法在 FLIP 预测、严格三/四阶功能恢复、最终隐藏层三/四阶可访问性上都没有显著架构差异。唯一的显著信号来自 RIT：它在 token 阶段直接提升了二阶可访问性（ΔA_tok,2=0.2468，d_z=1.67，p=1.14×10⁻⁵），但这种提升没有带来下游高阶优势。第二层是深度/容量敏感性分析：将骨干增加到三或四层后，MLP 的 FLIP 预测、三阶功能恢复和最终层三阶可访问性显著提升；四阶可访问性也有改善，但绝对 held-out R² 仍为负。独立 token 在深度四时只有较小预测增益，而非线性 token 与 RIT 的增益更小（摘要截断，需原文确认）。

论文的论证主线是：仅仅观察预测指标会掩盖表示的结构差异；通过将交互按阶次分离，并区分表示阶段和功能输出，可以发现显式的 token 交互（RIT）确实在输入表示层面增加了二阶信息，但当前训练目标未将这些信息转化为高阶外推能力；相反，增加网络深度能更有效地促进三阶功能恢复，却仍不足以解决四阶外推（负的绝对 R²）。

结论部分强调，这是对“可解释性诊断”与“蛋白质表示学习”的一种系统化尝试：ORBIT 不直接输出相互作用机制，而是沿着表示轨迹定位交互结构的出现位置和阶次。它提示，强成对预测并不代表机制恢复，高阶结构需要专门的目标和评估。整体上，论文的工作范式（合成验证→真实数据→统计严谨比较）值得借鉴，其结果也为后续研究提供了清晰的开放问题。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：ORBIT 是一种系统化、可复用的评测框架，适合作为你方法论偏好的“系统性工作”模板。

## 基本信息

- 作者：Maryam Rahimimovassagh, Ivan Garibay, Niloofar Yousefi
- 机构：未提供
- 来源：arxiv
- 主题/分类：q-bio.QM, cs.LG
- 日期：2026-08-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.24953v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（51 个 chunk），核心内容来自 Abstract、Contributions、Experimental Questions 和 Conclusion 等片段，部分推断已在对应字段标注。
