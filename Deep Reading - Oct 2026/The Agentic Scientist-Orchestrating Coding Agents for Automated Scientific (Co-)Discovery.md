---
user_id: "cheng tan"
paper_id: 16151
arxiv_id: "2610.18933v1"
title: "The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.18933v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.18933v1"
generation_provider: "heuristic"
generation_model: "PaperFlow template"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-18T02:10:38"
---
# The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery

> ★★★★☆ 推荐阅读 · 模型 heuristic/PaperFlow template

🏷 关键词：agentic scientist · agents automated · automated scientific · co- discovery · coding agents · program · training · evaluation

## 一句话总结

We study a learned evolution strategy whose learned outputs are zero-initialized bounded multiplicative residuals on a separable CMA-ES core, so the strategy is exactly the…

## 摘要

> Learned optimizers are usually reported by the margin they win by, not by the
> margin their hand-designed scaffolding already wins by. We study a learned
> evolution strategy whose learned outputs are zero-initialized bounded multiplicative
> residuals on a separable CMA-ES core, so the strategy is exactly the classical Figure 1: The object of study. Three small learned modules emit bounded multiplicative residuals
> algorithm at initialization. Its 364 meta-parameters are meta-evolved only on on a separable CMA-ES core (recombination weights, the step size and mean step, and the per-
> 2–10-dimensional analytic functions, frozen, and the same optimizer is rebuilt coordinate axis scales). Zero-initialized output layers make every residual exactly 1 at initialization,
> for a 704-dimensional Brax Ant policy and scored by best-so-far area under the so the strategy starts as, and can be forced back to, the classical algorithm. The meta-parameters are
> curve over 2,000 generations, normalized so the initial policy is 0 and a same-seed evolved only on 2–10-dimensional analytic tasks, frozen, and the same optimizer is then rebuilt for a
> CMA-ES reference is 1. The strategy scores 1.191 and beats the reference on all 704-dimensional control policy and scored against a pinned same-seed CMA-ES reference.
> eight evaluation seeds. However, switching the learned modulation off—which
> provably reduces the same code to separable CMA-ES plus the fixed sampling
> machinery—still scores 1.107, so the anchor supplies 55.8% of the above-reference machinery, and the same evaluation harness. The gap between that control and the full method is the
> margin. The learned residual is real (+0.083 paired, 8/8 seeds) but conditional: part meta-learning is responsible for; everything below it is scaffolding.
> it shrinks with budget and reverses sign on final solution quality. Meta-learning Our contributions are as follows. (i) We describe a 364-parameter anchored residual evolution strategy succeeded in-distribution; transfer is where the gain was lost. whose learned modules are permutation-equivariant in the population and shared across coordinates,
> so a single weight vector serves a 16-member population in 2–10 dimensions during meta-training
> 1 INTRODUCTION andreadinga 256-membera generationpopulationcounter or ina budget.704 dimensions(ii) We reportat deployment,the decompositionwith noratherlearnedthanfeaturethe headline:or gate
> against a pinned same-seed CMA-ES reference at 1.000, the identity-gate anchor reaches 1.107
> Black-box optimizers for continuous problems are still overwhelmingly hand-designed, and the and the full method 1.191, so the anchor accounts for 55.8% of the entire above-reference margin.
> design space is large enough that new heuristics appear faster than they can be compared (Osaba et al., (iii) We show the surviving learned increment is genuine but conditional. It is positive on all eight
> 2021; Houssein et al., 2022). Meta-black-box optimization proposes to replace that manual search: seeds, yet its share of the above-reference excess falls from 86.5% at generation 1,000 to 44.2% at
> parameterize the update rule of an evolutionary algorithm with a small neural network, meta-train it generation 2,000, and it reverses sign on final solution quality, where the no-learning anchor finishes
> on a distribution of cheap objective functions, and hope that the resulting rule generalizes (Lange 220.7 return higher. (iv) We localize the failure: meta-training worked, cutting final log regret by
> et al., 2023; Gaier et al., 2020; Zhan et al., 2023). The appeal is that the inner tasks can be cheap 50.8% on its training functions and 28.4% on held-out ones. What did not survive was the transfer of
> analytic functions while the deployment task is expensive—for instance a neural control policy, that gain from at most ten dimensions to a 704-dimensional control task.
> whose parameter space is high-dimensional and awkwardly conditioned enough to frustrate even
> methods with access to far more structure, such as Markov chain Monte Carlo samplers for Bayesian
> neural networks (Papamarkou et al., 2022). 2 RELATED WORK
> This creates an attribution problem, and it is structural rather than incidental. Learned update rules Meta-black-box optimization treats the update Discovering optimizers instead of designing them. are almost never learned from nothing: they are parameterized deviations from an update that already rule of an evolutionary algorithm as a differentiable-free parameterized object and searches over it works. Outside evolutionary computation, meta-learned optimizers such as Meta-SGD keep gradient with an outer optimizer. Attention-based genetic algorithms discovered this way generalize across descent as their backbone, learning a per-parameter vector that rescales and redirects its step (Li problem classes without per-task retuning (Lange et al., 2023); related work learns the search et al., 2017); inside it, learned evolutionary operators sit on top of the selection, recombination, and representation rather than the update (Gaier et al., 2020), and broader surveys situate these methods adaptation machinery of the algorithm they modify (Lange et al., 2023). A learned rule built on a within learning-aided evolutionary computation (Zhan et al., 2023) and meta-learning generally competent classical algorithm therefore inherits that algorithm’s performance for free. If it then beats (Vanschoren, 2019). The impulse is not specific to evolutionary computation: gradient-based meta- an external baseline, the reported margin conflates two very different quantities: what meta-learning learning also learns the update rule itself, for instance by meta-learning a per-parameter step vector on contributed, and what the scaffolding already contributed. Separating them requires a comparison the top of stochastic gradient descent (Li et al., 2017). In both families the learned object is a modification external baseline cannot provide—the same code with the learned part switched off. The question of a working algorithm, which is precisely what makes attribution hard. Our setting follows the does a learned optimizer how much of its measured this paper asks is therefore not transfer, but evolutionary template—outer evolutionary search over inner-strategy parameters, frozen transfer—but advantage is the optimizer it was anchored to. is instrumented so that the classical contribution can be subtracted out.
> We make that question answerable by construction. We study a learned evolution strategy in
> g(v) = 2tanh(3v) every learned output is a zero-initialized bounded multiplicative residual, The mechanisms our learned gates modulate Adaptation inside classical evolutionary algorithms. which12, 2], ∈[ applied to a component of a separable CMA-ES core. Because the final layer of each network is are exactly the ones that classical algorithms already adapt by hand: recombination weights, step
> initialized
> the fixed sampling to zero, g(·)machinery.= 1 exactlyThis atmakesinitializationan exactandnullthecontrolstrategyavailable:is separableforcingCMA-ESevery gatewrappedto 1 forin size,accumulatedand covariance.are surveyedThe evolution-strategyby Emmerich et al.family(2018).and CMA-ES,the derandomizedits best-knownadaptationmember,mechanismscouplesit
> the whole run recovers the classical algorithm within the same code path, the same fixed sampling cumulative step-size adaptation with rank-one and rank-µ covariance updates (Hansen, 2016); its
> 1 2
> separable variant restricts the covariance to a diagonal and remains competitive in high dimension, Anchor, and make the anchor exactly recoverable. Every learned output is a bounded multiplicative
> particularly under low effective dimensionality (Yamaguchi et al., 2022); scaling population-based residual
> g(v) = exp ln search to hundreds or thousands of variables is a distinct problem with its own literature (Omidvar
> et al., 2022; Yang et al., 2018). A long line of work hand-designs the online adaptation of operators applied to a quantity that separable CMA-ES 2 · tanh(γv)already∈[computes.12, 2], Theγ =final3, dense layer of every(2)
> and control parameters (Elsayed et al., 2017), of the selection rule itself (R´unarsson & Yao, 2000), or module has zero-initialized kernel and bias, so v = 0 and g(0) = 1 exactly at initialization: the
> of an adaptive grid maintained over the search space (Rostami & Shenfield, 2016). Transferring tuned anchored core is the classical algorithm, not an approximation of it. The outer search therefore
> CMA-ES parameters between problems has itself been studied (Thomaser et al., 2023), and CMA-ES explores only dimensionless multiplicative deformations of a known-good algorithm, and the worst
> is routinely extended with surrogates or randomized parameters for applications (Hamdani et al., 2019; case is bounded—no gate can move a quantity by more than a factor of two per generation. It also
> Sauerland et al., 2023). Our residual gates can be read as a learned, bounded, g evidence-conditioned makes the decisive experiment trivial: pinning
> version of these adaptation rules that degrades gracefully to the unmodified algorithm. construction, separable CMA-ES plus the fixed machinery≡1 for a wholeof Sectionrun yields3.4, anda configurationshares every thatotheris,lineby
> of code with the full method.
> Because a strategy must accept any popu- Architectures for population-agnostic conditioning.
> Meta-training runs for 50 inner generations in 2–10 dimensions; Condition on evidence, not on time. lation size and any dimension, the component that consumes the population is naturally expressed
> 2,000 deployment runs for generations in 704. A feature carrying a generation index, a budget fraction, as a permutation-equivariant set function rather than a fixed-width vector; permutation-invariant
> or a horizon would risk letting the outer search fit a schedule tied to the 50-generation meta-training attention architectures formalize this (Lee et al., 2018). Components that consume per-coordinate
> regime; we therefore exclude such features by design rather than by measurement, as no time- or scalar summaries can instead be shared across coordinates, which is the route we take for two
> conditioned configuration was run. Every feature we use is dimensionless—normalized ranks, cosine of our three modules. Amortized conditioning of a predictor on a variable-size context set is the
> alignments, path-length ratios against their neutral expectations, success frequencies—and no learned same construction used by neural processes and their descendants (Gordon et al., 2019; Jha et al.,
> feature or gate reads a counter or a budget. The one counter in the algorithm is the evidence-driven 2022), and by closely related amortized-inference transformers (M¨uller et al., 2021), and the practical
> stall counter of Section 3.4, which resets on improvement rather than tracking elapsed time. importance of normalizing such inputs is well documented (Huang et al., 2020).
> Where a primary source we rely on conceptually is not cited directly, we name it in prose and, where
> a survey or tutorial covers the same material, cite that instead (Emmerich et al., 2018; Hansen, 2016). 3.3 THE ANCHORED STRATEGY
> This applies to the original separable CMA-ES of Ros and Hansen, cumulative step-size adaptation = Candidates are drawn in the standard separable reparameterization xi Search distribution.
> of Hansen and Ostermeier, the theory of mirrored sampling, the BBOB/COCO noiseless testbed, and m + σ d scalar step size σ, and per-coordinate axis scales ID), >0.
> the Brax physics engine. O(D) The representation ⊙zi, withiszidiagonal,∼N(0, so state and compute are rather than O(D2), which d ∈RDis what
> keeps the strategy cheap at D = 704 with a population of 256; the restriction follows the evidence
> 3 METHOD that a diagonal covariance remains competitive in high dimension, particularly under low effective
> dimensionality (Yamaguchi et al., 2022). We treat this as a design assumption rather than a demon-
> 3.1 PROBLEM SETUP AND SUCCESS CRITERION stratedin this study,necessity:and theno full-covariance,large-scale literaturelimited-memory,contains otherortractablelow-rankhigh-dimensionalcomparator was runsearchat thismethodsscale
> (Omidvar et al., 2022; Yang et al., 2018). The one covariance-representation comparator we do run θ A candidate strategy is an ask/tell search distribution over carrying a vector of learned RD
> 0.360 moves in the opposite direction—pinning the axis scales to 1—and the diagonal beats it by 1 meta-parameters, with The evaluator is fixed and has two parts. In the first, ≤dim paired (Section 4.4). θ is meta-evolved on cheap analytic θ ≤1024.functions. In the second, θ is frozen, digest-authenticated,
> and the same strategy is rebuilt for a continuous-control policy search, which is what produces the
> reported score. The control task never updates θ or any outer-search state, so the second part is a pure Three learned modules (364 parameters). All modules use a hidden width of 8 and a zero-
> out-of-distribution transfer test. initialized output layer. (a) A recombination residual (201 parameters) is a permutation-equivariant
> 6 set encoder over the population in the Deep-Sets style (Lee et al., 2018): a ReLU layer
> The control task is an episodic reinforcement-learning problem whose objective is the return ac- →8 16 per member, a mean-pooled context broadcast back and concatenated, a ReLU layer, and
> cumulated over a policy rollout (Kaelbling et al., 1996), but here the optimizer observes only that →8 8 an output. Its output gates the CMA recombination weights, followed by wi i episode-level return, and the problem is solved by direct search over policy parameters rather than →1 ←wcma g(·), 9 9 (b) renormalization. (98 parameters) map scalar features through ReLU Global gates
> ant by value or gradient estimation. Concretely it is a Brax v1 environment with legacy-spring 8 (c) and to two scalars that modulate the step-size update and the mean step. A →8dimension
> 87 8 + 8 = 704 dynamics and contact-force observations, optimized with a tanh-linear policy over 6 6 8 (65→2 parameters) maps per-coordinate features through ReLU and applied gate × 2,000 parameters, initialized at the origin, with 500-step episodes, a population of 256, and genera- →8 →1, identically to every coordinate, and modulates the exponent of the diagonal covariance update. Total:
> tions. For each of eight fixed evaluation seeds we take the mean of the best-so-far return curve over 201 + 98 + 65 = 364 D meta-parameters. No weight shape depends on or on the population size N;
> all generations—the anytime area under the curve—and average over seeds. Writing and Aalg, Acma N the set module is equivariant in and the dimension gate is shared across coordinates, which is what
> for that quantity under the candidate, under a pinned same-seed CMA-ES reference, and under Ainit allows one frozen weight vector to serve both regimes.
> the untrained initial policy, the reported score is
> Aalg The six member features are a centered rank, a Gaussian-quantile transform of that s = −Ainit
> Acma rank, a centered improvement indicator, a normalized radial term a normalized
> The reference is pinned: −Ainitpopulation, so256, 2,000s = 0generations,at the initialinitialpolicy,standards = 1deviationat CMA-ES.0.05, the same(1) Features. base-weight term, and the cosine alignment between the member’s tanh(∥zi∥2/Dnoise and the −1),step-size evolution
> eight seeds, the same episode budget. The comparison is therefore iso-budget and same-seed, and path. The nine global features are the CSA signal
> s > 1 means the candidate accumulates more best-so-far area than a strong classical optimizer under log elite radial term, the spread of d, a path alignment,tanh(∥pσ∥/χDand four success−1), aratesrank-one(threepathexponentialratio, an
> identical conditions. Note that Equation 1 rewards anytime progress: a strategy can win on s while moving averages at different rates plus the current-generation improvement fraction), each mapped
> finishing at a worse solution, a distinction that turns out to matter. to the[−1,rank-µ1]. Theand rank-onesix per-coordinateterms minusfeaturesone, andare tanha broadcasttransformsglobalofsignal.pσ, of pc/d,Every offeaturecenteredis scale-freelog d, of
> 3.2 DESIGN RATIONALE: MAKE THE CLASSICAL CONTRIBUTION SUBTRACTABLE by construction, which also keeps the network inputs in a well-conditioned range (Huang et al.,
> 2020); conditioning a fixed parameter vector on a variable-size evidence set in this way is the same
> Two design commitments follow from the attribution question. amortization used by conditional neural processes (Gordon et al., 2019; Jha et al., 2022).
> 3 4
> Learned CMA-ES Paper (pages 1–4 of 13)
> 44
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> m σ The mean moves by with the weighted step; the step size follows (ablation). Mirroring is kept but Equation 3 is yw ywUpdates. • No pair-consistent path normalization log←m+gmean +cumulative step-size adaptation, and the axis scales replaced by the textbook Hypothesis: this mis-specifies the step-size controller and gσ µeff.
> follow the separable rank-one plus rank-µ σ ←logratio,σ raised (cσ/dσ)(∥pσ∥/χDto an exponent modulated−1); by the dimension produces a landscape-independent step-size collapse.
> gate, followed by a geometric-mean renormalization that keeps (ablation). The restart predicate is forced false; the counter and stall re-expansion
> σbetween and d does not drift. With all gates at 1 these are precisely σ · d theinvariantseparableso theCMA-ESgauge freedomupdates • Noincumbent tracking remain but are inert. Hypothesis: the late-run record-setting comes from
> (Hansen, 2016). restart-driven re-exploration once the step size has been ground down.
> 3.4 FIXED DISTRIBUTIONAL MACHINERY 3.6 EXPERIMENTAL PROTOCOL
> Three mechanisms sit outside the learned part and are shared by every configuration below. Meta-training. The outer optimizer is CMA-ES with population 256, run for 1,500 generations.
> 128 Each outer generation samples fresh analytic tasks: a function drawn from ten BBOB-styleMirrored sampling. The first rows are drawn independently; each remaining row ⌈N/2⌉noise 2 training functions, a dimension drawn uniformly from to 10, a shifted and rotated optimum, andis either the exact antithetic mirror of a drawn row or an independent draw, selected by a gate
> [0, additive Gaussian noise with standard deviation drawn from 0.1]. Each candidate runs its innerclip(1 0, 1) s where is the slowest success moving average. Every row is marginally
> 16 50 strategy on every task with population for generations, starting at standard deviation 1.0. A under either branch, and the gate reads no clock: when the population is improving often, ID)−Ns/m,
> within candidate’s loss on a task is its best observed clean objective; losses are standardized each taskN(0,mirroring is spent sparingly; when improvements dry up, the draw becomes fully antithetic and the
> 128 median across the candidate population and then aggregated by the over the tasks, so no singleodd component of the weighted noise sum cancels.
> task or scale can dominate. Non-finite or invalid tasks receive a large fixed penalty. The outer mean
> Pair-consistent path normalization. Mirroring changes the neutral variance of the weighted noise after 1,500 generations is the selected weight vector.
> sum, because a mirrored pair contributes (wa −wb) rather than two independent draws. We thereforenormalize both evolution paths by Freeze and transfer. The selected weights are serialized, hashed, and re-loaded under a runtime
> identity check that binds the source hash, the configuration digest, and the evaluator hash. The
> w2 −1 , (3) strategy is then rebuilt for the 704-dimensional control problem with the deployment population. No µpatheff unpaired
> further optimization of the weights occurs, and no outer state is carried across.
> capped at N, instead of the textbook the textbook constant with a mirrored = Pµeffpairs(wa= −wb)2i w2i )−1.+ UsingP
> draw biases which Each configuration is evaluated on the same eight fixed seeds (10,000–10,007). We drives a monotone, landscape-independent Reporting.
> step-size decay;∥pσ∥/χDSectionbelow4.4 measuresits neutralexactlyvalue,(Pthat failure. report the score of Equation 1 together with the standard error over those eight seeds, per-seed win
> counts against the pinned reference, and paired per-seed differences against the proposed configu-Capped stall re-expansion. A counter tracks generations without an epoch-best improvement, with ration; a paired difference is the honest statistic here because all configurations share seeds, initial
> 120 + 30D/N D = N =patience (202.5 at 704, 256). On firing, the mean is re-anchored on the conditions, and budget. We also report an exact sign test over the eight seeds, but always alongside
> σ min(3σ,monotone incumbent, the paths are reset, and so the step size can only σ0)), n = the win count, because the two-sided sign-test p-value at 8 is identical for a configuration that ←max(σ,grow and only up to its initial value. The monotone incumbent is never discarded.
> 8/8 2,000 wins and one that loses 0/8. All configurations completed the full generations on the same
> 8-device hardware allocation within comparable wall-clock times, so no comparison is confounded
> 3.5 COMPARED CONFIGURATIONS by truncated runs. Following common practice in metaheuristic experimentation (Osaba et al., 2021),
> we fix budget, seeds, and the reference implementation across all configurations rather than tuning
> Seven configurations were evaluated under the identical fixed evaluator, budget, seeds, and scoring each separately.
> rule. Each row is a single, source-level change against the proposed strategy unless stated otherwise.
> • Proposed. The anchored residual strategy above, 364 meta-parameters, weights frozen after 4 RESULTS
> meta-training.
> • Identity-gate anchor (decisive control). A single module constant makes the gate function 4.1 THE HEADLINE, AND WHAT IT DECOMPOSES INTO
> return 1 unconditionally, so all three learned modules become the identity. The remaining proposed strategy scores (mean standard error over eight seeds) and exceeds the
> algorithm is separable CMA-ES plus the fixed machinery of the previous subsection; d = the Thepinned same-seed CMA-ES reference1.191±0.019 on 8/8 seeds±(sign test p = 0.0078; paired Cohen’s 4.16).
> meta-parameters have no effect on any update. This is the paper’s central comparison. 417.5 [356.7, In raw units it improves the mean best-so-far return by (95% bootstrap interval 486.7],
> in return units) and reaches the reference’s final return in 828 generations, a The axis scales are pinned to 1, deleting the diagonal • Isotropic step-size-only baseline.
> Read on its own, this is a successful analytic-to-control transfer of a learned optimizer.2.42× budget saving. covariance entirely and leaving λ)-ES with cumulative step-size adaptation, the a (µ/µw,
> D = 704 learned recombination residual, and the learned scalar gates. Hypothesis: at with The identity-gate control refuses that reading. Switching the learned modulation off—which, as
> 256 samples the diagonal estimate is badly under-sampled, so learned diagonal covariance verified at the source level, makes every gate exactly 1 and reduces the strategy to separable CMA-ES
> transferred from a 2–10-dimensional training distribution might be inert or actively harmful. 1.107 1.000 plus the fixed machinery—still scores (Figure 2a). Since the reference is by
> 55.8% definition, the anchor alone supplies of ±the0.028 entire above-reference margin. The paired learning • A self-contained port of a standard learned-evolution- Unanchored learned strategy.
> +0.083 8/8 increment over the same eight seeds is 0.016, positive on seeds (Figure 2b). The 142 strategy formulation with freshly initialized parameters and no classical anchor, meta- ± 1.000 correct summary of the run is therefore a decomposition, not a headline: pinned CMA-ES trained and transferred under the same evaluator. It differs from the proposed strategy in → 1.107 identity-gate anchor method 1.191. parameter count and in architecture simultaneously, so we use it only as motivation for →full anchoring, not as a controlled measurement of it. Two further cautions apply to the anchor row itself. Its own advantage over the reference is 7/8 seeds
> • No mirrored sampling (ablation). The mirrored draw is replaced by an unmirrored i.i.d. with sign-test p = 0.070, which is not significant at the conventional level; and because configuration
> Gaussian block, with the path normalization consistently reverting to µeff. Hypothesis: selection during development used this same eight-seed protocol, 1.191 should be read as the score
> mirroring trades slightly slower early progress for a better-conditioned distribution. of a selected configuration rather than an unbiased estimate of the method.
> 5 6
> Figure 2: The score decomposes into a large scaffolding term and a small learned term. (a) Normalized Figure 3: The advantage is conditional on both budget and metric. (a) Anytime normalized score
> area under the best-so-far curve at 2,000 generations, with standard errors over eight seeds; the dashed as a function of generation for four configurations, with the pinned reference at 1. The two learned
> line is the pinned same-seed CMA-ES reference, which is a normalization constant rather than a anchored configurations start above the reference and decay toward it, while the identity-gate anchor
> measured bar. (b) Paired per-seed differences of the proposed configuration minus each comparator, starts below it and rises through it; the unanchored learned strategy improves but never reaches it.
> annotated with the number of seeds on which the proposed configuration is ahead. Among the four (b) Mean best-so-far return for the proposed configuration, the identity-gate anchor, and the pinned
> comparators plotted here, removing the pair-consistent path normalization is the largest effect; the reference. The anchor is behind for most of the run, passes the proposed configuration at generation
> isotropic and unanchored configurations of Table 1, which replace the covariance representation 1216, and finishes 220.7 ahead.
> and the architecture rather than removing one mechanism, differ by more. Removing the stall
> re-expansion is a null (3/8 seeds).
> (−28.4%),the identity-gaterelativecontrolto thetheseidentity-gatetwo differencesanchorareevaluatedexactly on0.000exactlyby construction,the same taskswhich(Figureis the4).sanityFor
> 4.2 THE LEARNED INCREMENT IS BUDGET- AND AXIS-DEPENDENT check that the anchor comparison is what it claims to be. The learned weights are also far from their
> initialization: their Euclidean distance from the anchor point is 20.62, versus 0.0021 for the control
> Figure 3 shows why the decomposition, and not the single number, is the defensible summary. whose gates are inert.
> Budget. The two configurations approach the reference from opposite sides. The proposed configu- The in-distribution gain is nonetheless narrow. Of the eleven instrumented functions, three regress
> ration starts far above it and decays toward it as the budget grows, from an anytime score of 1.663 under the frozen weights, and a single training function—the ill-conditioned attractive sector—
> at generation 250 to 1.191 at 2,000. The identity-gate anchor starts below the reference at 0.897, supplies 88.0% of the mean training-set improvement. On a separate 400-generation analytic probe
> 500crosses it between generations and 1,000, and keeps rising to 1.107 at 2,000, moving further run at deployment scale, the frozen weights are worse than
> above the reference over exactly the window in which the proposed configuration is falling toward it. regret). Meta-training thus found a real but concentrated improvement the anchor (−1.10on its versusown distribution,−2.24 final andlog
> The share of the above-reference excess attributable to the learned part therefore falls monotonically, that improvement did not broaden into the transfer regime.
> from 86.5% at generation 1,000 to 64.8% at 1,500 and 44.2% at 2,000, while the anchor’s share
> rises correspondingly. Extrapolating the trend is not warranted, but within the measured window the
> learned contribution is not a fixed property of the strategy—it is a property of the strategy at a given 4.4 WHICH COMPONENTS CARRY THE SCORE
> budget.
> Metric axis. The score of Equation 1 integrates the whole best-so-far curve and therefore rewards Table 1 gives the full picture. Three findings stand out.
> fast early progress. On final solution quality the ordering inverts. The proposed configuration finishes The diagonal covariance is essential, not inert. Pinning the axis scales to 1 costs 0.360 paired
> at 3488.5 mean best return, the identity-gate anchor at 3709.2: the anchor ends 220.7 higher, and its (8/8 seeds) and drops the configuration below the reference to 0.830. The concern that a diagonal
> return curve overtakes the proposed one at generation 1216 and never falls back (Figure 3b). Per-seed estimate from 256 samples in 704 dimensions would be pure noise is not borne out: even a badly
> final returns were logged only against the pinned reference, which both configurations beat on most under-sampled diagonal, carrying learned and anchored structure, is worth far more than the learned
> seeds (7/8 for the anchor, 8/8 for the proposed configuration), so this reversal rests on the mean gap recombination residual and scalar gates that remain without it.
> and the crossover rather than on a paired seed count. The learned modulation buys a faster approach Removing the pair- normalization is a correctness requirement, not a tuning choice.and a worse destination. A study that reported only the anytime score, or only the final return,
> 0.294 would Pathconsistent normalization of Equation 3 while keeping the mirrored draw costs paired (8/8have reported opposite conclusions from the same eight runs. seeds), the largest of the fixed-machinery ablations. Two configurations in Table 1 differ by more—the
> isotropic baseline at 0.360 and the unanchored strategy at 0.765—but both replace a representation or
> 4.3 META-LEARNING WORKED; THE TRANSFER IS WHAT FELL SHORT an architecture wholesale rather than removing a single mechanism, so they are not comparable effect
> sizes. The mechanism is visible in the trajectory: that configuration’s return freezes at 2527.8 from
> It would be easy to conclude that the outer search simply failed to learn anything. It did not. Evaluated roughly generation 500 onward, exactly the signature of a step-size controller receiving a persistently
> on its own training distribution with the same frozen weights, the strategy cuts final log regret from biased neutral signal. This is best read as a measurement of self-inflicted mis-specification rather than
> 0.766 to 0.376 on the training functions (−50.8%) and from 1.553 to 1.112 on held-out functions as the contribution of a component—which is precisely why we report the honest mirroring ablation
> 7 8
> Learned CMA-ES Paper (pages 5–8 of 13)
> 45
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> 4.5 ANCHORING AS MOTIVATION, NOT AS A CONTROLLED MEASUREMENT
> The unanchored learned strategy scores 0.426 and never reaches the reference on any seed, ending at
> 2007.5 return against the reference’s 3256.9. The contrast with 1.191 is the intuition that motivated
> anchoring in the first place, and Figure 3a shows it is not a slow-start artifact: that configuration’s
> anytime score rises monotonically and still ends far below 1. We deliberately decline to read the 0.765
> gap as the causal effect of anchoring. That configuration differs from the proposed one in parameter
> count (142 versus 364), in module architecture, and in feature design at once; the parameter-matched
> unanchored comparison that would identify the effect was not run. What the run does establish
> cleanly is the opposite direction of the same question—that the anchor by itself already accounts for
> most of what the anchored system achieves.
> 5 LIMITATIONS AND THREATS TO VALIDITY
> One meta-training run per configuration. Every configuration was meta-evolved once. The
> eight seeds are evaluation seeds inside that single run, so every standard error we report excludes
> meta-training-seed variance, which is plausibly the larger source. Reported dispersion is therefore a
> Figure 4: Meta-training succeeded on its own distribution. Final log regret of the frozen weights lower bound on the true uncertainty.
> against the identity-gate anchor on the analytic meta-training functions and on held-out analytic Every configuration, including Comparisons confound mechanism with meta-training outcome.functions (lower is better). The same frozen weights buy only a +0.083 paired increment on the the identity-gate control, was meta-evolved in its own separate run, and three of them—the isotropic704-dimensional control transfer, and lose on final solution quality, which places the shortfall in step-size-only baseline and the no-mirroring and no-path-normalization ablations—landed at weightstransfer rather than in meta-learning. measurably different from the proposed configuration’s: the recorded distance from the anchor
> point is 20.62 for the proposed configuration and 19.12, 13.50 and 9.90 for those three. For them,
> the reported delta mixes “this mechanism was removed” with “a different single meta-training run
> Table 1: All seven configurations under the identical evaluator, budget, seeds, and scoring rule. Score happened.” The no-stall-re-expansion ablation is the exception among the configurations whose gates
> is the normalized anytime area under the best-so-far curve at 2,000 generations (pinned same-seed are active: its recorded distance is 20.62, matching the proposed configuration’s to full precision,
> CMA-ES = 1), with standard error over eight seeds. ∆is the paired per-seed difference of the so for that row the confound does not apply and the delta is closest to a clean mechanism removal.
> proposed configuration minus the row, with the number of seeds on which the proposed configuration We note this as an observation about the recorded weights and do not use it to argue anything about
> is ahead. Wins is seeds above the pinned reference. Gcma is generations to match the reference’s final the other rows. The identity-gate control is immune to the confound by construction, because its
> return; a dash marks configurations that never reached it within 2,000 generations. meta-parameters provably cannot affect any update—which is another reason it, rather than the
> ablation table, carries the paper’s claim.
> Configuration Score ∆(paired) Wins Final return Gcma One environment, one policy class, one budget regime. All transfer evidence comes from a single
> D = Brax Ant task with a tanh-linear policy at 704. We have shown the conclusion is sensitive to 1.191 0.019 Proposed (anchored residual) — 8/8 3488.5 828 ± 1.185 0.023 0.006 0.026 the budget and to the metric; we have not shown it is stable across environments, policy classes, or No stall re-expansion (3/8) 8/8 3461.1 839 ± ± 1.107 0.028 0.083 0.016 Identity-gate anchor (8/8) 7/8 3709.2 1042 population sizes. The 2,000-generation window also sits close to where the two curves cross, so the ± ± 1.094 0.022 0.097 0.024 No mirrored sampling (8/8) 8/8 3286.9 1509 sign of the “learning helps” conclusion is not robust to a modest change in horizon. ± ± 0.897 0.021 0.294 0.026 No pair-consistent normalization (8/8) 0/8 2527.8 — ± ± The eight-seed protocol used to report the headline was also used to select Selection and reporting. 0.830 0.024 0.360 0.019 Isotropic step-size only (8/8) 0/8 2362.5 — ± ± 0.020 0.765 0.021 the configuration during development, so the reported score carries a selection bias of unquantified Unanchored learned strategy 0.426 (8/8) 0/8 2007.5 — ± ± n = 8 size. The sign-test p-value we quote is direction-agnostic at and is only interpretable next to 1.000 Pinned CMA-ES reference — — 3256.9 2000
> the win count. Finally, the analytic probes and the control task are different measurement families, 0.000 Initial policy — — 497.4 —
> and we have kept them on separate axes rather than merging them into a single figure.
> Interpretability of the learned part. Bounded multiplicative residuals around an identity make
> the learned component unusually legible for a learned optimizer—its worst-case effect is a factor
> what two on a named quantity—but we have not extracted a human-readable account of theseparately. Removing mirroring itself, with the normalization consistently reverted, costs only 0.097 364of parameters encode, and the gate diagnostics available in this study were recorded only for thepaired—the same order as the +0.083 learning increment. configuration in which the gates are inert. Learned components embedded in otherwise transparent
> The stall re-expansion is a null. Deleting it changes the score by 0.006 paired, with the proposed algorithms remain black boxes in the sense studied by the explainability literature (Guidotti et al.,
> configuration ahead on only 3/8 seeds and the paired standard error (0.026) four times the effect; 2018; Arrieta et al., 2020), and the anchored design mitigates the risk rather than removing it.
> the per-seed differences straddle zero, from
> generations at D = 704, N = 256, and on the−0.136separateto +0.084.400-generationThe mechanism’sanalytic probe—runpatience is 202.5at the
> same D = 704, N = 256 scale—the stall counter peaks at 7 and the re-expansion never fires there. 6 CONCLUSION
> We did not instrument firings over the 2,000-generation control runs, and we cannot infer that the
> mechanism was dormant there: the longest recorded stretch without a strict best-so-far improvement We asked how much of a frozen-transferred learned evolution strategy’s advantage is produced by
> on those runs averages 275 generations and reaches 578, above the patience threshold. The negative meta-learning and how much by the classical algorithm it is anchored to, and we built the strategy
> result therefore rests on the paired delta alone—whether or not the re-expansion fired on Ant, deleting so that the question has an exact answer. Against a pinned same-seed CMA-ES reference at 1.000,
> it does not move the score. the full method scores 1.191 and the identity-gate anchor—the same code with every learned gate
> 9 10
> provably pinned to one—scores 1.107. The anchor supplies 55.8% of the above-reference margin. Hamid Hamdani, Boucha¨ıb Radi, and Abdelkhalak El Hami. Optimization of solder joints in
> The remaining increment is real and consistent across seeds, but it shrinks as the budget grows and embedded mechatronic systems via kriging-assisted cma-es algorithm. International Journal
> reverses sign on final solution quality. for Simulation and Multidisciplinary Design Optimization, 10:A3, 2019. ISSN 1779-6288. doi:
> The negative result is specific and, we think, more useful than the positive one. Meta-training on cheap 10.1051/smdo/2019002. URL http://dx.doi.org/10.1051/smdo/2019002.
> analytic functions clearly worked: it halved final log regret in-distribution and improved held-out Nikolaus Hansen. The cma evolution strategy: A tutorial, 2016. URL https://arxiv.org/
> analytic functions by nearly thirty percent. What did not survive was the transport of that gain to a abs/1604.00772.
> 704-dimensional control policy, where it degraded into a budget-dependent head start. Anchoring is
> the design choice that bounds how far that failure can propagate—no gate can move a quantity by Essam H. Houssein, Mohamed H. Hassan, Mohamed A. Mahdy, and Salah Kamel. Development and
> more than a factor of two in a generation, and the classical algorithm remains exactly recoverable application of equilibrium optimizer for optimal power flow calculation of power system. Applied
> as a control. It does not guarantee that the anchored system matches that algorithm, and here it Intelligence, 53(6):7232–7253, 2022. ISSN 1573-7497. doi: 10.1007/s10489-022-03796-7. URL
> did not: the full method finished 220.7 return below the identity-gate anchor. The one unanchored http://dx.doi.org/10.1007/s10489-022-03796-7.
> configuration we ran under the same protocol collapsed to 0.426, but it also differs in parameter
> count and architecture, so we report it as motivation rather than as a measurement of what anchoring Lei Huang, Jie Qin, Yi Zhou, Fan Zhu, Li Liu, and Ling Shao. Normalization techniques in training
> contributes—an identification this study does not provide. dnns: Methodology, analysis and application, 2020. URL https://arxiv.org/abs/2009.
> 12836.
> The methodological point generalizes beyond evolution strategies, and beyond this benchmark.
> Whenever a learned component is layered onto a competent classical procedure—and gradient-free Saurav Jha, Dong Gong, Xuesong Wang, Richard E. Turner, and Lina Yao. The neural process
> optimizers of this family are deployed that way in settings from engineering design to variational family: Survey, applications and perspectives, 2022. URL https://arxiv.org/abs/2209.
> quantum algorithms (Cerezo et al., 2021; Hamdani et al., 2019)—the honest experiment is the one 00517.
> that can switch the learned part off exactly, on the same code path, and report the two numbers side
> by side. When such a control is cheap to build, as it is for zero-initialized bounded residuals, there is Leslie Pack Kaelbling, Michael L. Littman, and Andrew Moore. Reinforcement learning: A survey.
> little excuse for reporting only the headline. Journal of Artificial Intelligence Research, 4:237–285, May 1996. ISSN 1076-9757. doi: 10.1613/
> jair.301. URL http://dx.doi.org/10.1613/jair.301.
> REFERENCES Robert Tjarko Lange, Tom Schaul, Yutian Chen, Chris Lu, Tom Zahavy, Valentin Dalibard, and
> Sebastian Flennerhag. Discovering attention-based genetic algorithms via meta-black-box opti-
> Alejandro Barredo Arrieta, Natalia D´ıaz-Rodr´ıguez, Javier Del Ser, Adrien Bennetot, Siham Tabik, Proceedings of the Genetic and Evolutionary Computation mization. In Conference, GECCO ’23,
> Alberto Barbado, Salvador Garc´ıa, Sergio Gil-L´opez, Daniel Molina, Richard Benjamins, Raja pp. 929–937. ACM, 2023. doi: 10.1145/3583131.3590496. URL http://dx.doi.org/10.
> Chatila, and Francisco Herrera. Explainable artificial intelligence (xai): Concepts, taxonomies, 1145/3583131.3590496.
> Information opportunities and challenges toward responsible ai. Fusion, 58:82–115, 2020. ISSN
> 1566-2535. doi: 10.1016/j.inffus.2019.12.012. URL http://dx.doi.org/10.1016/j. Juho Lee, Yoonho Lee, Jungtaek Kim, Adam R. Kosiorek, Seungjin Choi, and Yee Whye Teh. Set
> inffus.2019.12.012. transformer: A framework for attention-based permutation-invariant neural networks, 2018. URL
> M. Cerezo, Andrew Arrasmith, Ryan Babbush, Simon C. Benjamin, Suguru Endo, Keisuke Fujii, https://arxiv.org/abs/1810.00825.
> Jarrod R. McClean, Kosuke Mitarai, Xiao Yuan, Lukasz Cincio, and Patrick J. Coles. Vari- Zhenguo Li, Fengwei Zhou, Fei Chen, and Hang Li. Meta-sgd: Learning to learn quickly for few-shot
> ational quantum algorithms. Nature Reviews Physics, 3(9):625–644, August 2021. ISSN learning, 2017. URL https://arxiv.org/abs/1707.09835.
> 2522-5820. doi: 10.1038/s42254-021-00348-9. URL http://dx.doi.org/10.1038/
> s42254-021-00348-9. Samuel M¨uller, Noah Hollmann, Sebastian Pineda Arango, Josif Grabocka, and Frank Hutter.
> Transformers can do bayesian inference, 2021. URL https://arxiv.org/abs/2112.
> Saber Elsayed, Ruhul Sarker, Carlos A. Coello Coello, and Tapabrata Ray. Adaptation of operators 10510.
> and continuous control parameters in differential evolution for constrained optimization. Soft
> Computing, 22(19):6595–6616, 2017. ISSN 1433-7479. doi: 10.1007/s00500-017-2712-6. URL Mohammad Nabi Omidvar, Xiaodong Li, and Xin Yao. A review of population-based metaheuristics
> http://dx.doi.org/10.1007/s00500-017-2712-6. for large-scale black-box global optimization—part i. IEEE Transactions on Evolutionary Compu-
> tation, 26(5):802–822, October 2022. ISSN 1941-0026. doi: 10.1109/tevc.2021.3130838. URL
> Michael Emmerich, Ofer M. Shir, and Hao Wang. Evolution Strategies, pp. 89–119. Springer http://dx.doi.org/10.1109/tevc.2021.3130838.
> International Publishing, 2018. ISBN 9783319071244. doi: 10.1007/978-3-319-07124-4 13. URL
> http://dx.doi.org/10.1007/978-3-319-07124-4_13. Eneko Osaba, Esther Villar-Rodr´ıguez, Javier Del Ser, Antonio J. Nebro, Daniel Molina, Antonio
> LaTorre, Ponnuthurai Nagaratnam Suganthan, Carlos A. Coello Coello, and Francisco Herrera.
> Adam Gaier, Alexander Asteroth, and Jean-Baptiste Mouret. Discovering representations for black- A tutorial on the design, experimentation and application of metaheuristic algorithms to real-
> box optimization. In Proceedings of the 2020 Genetic and Evolutionary Computation Conference, world optimization problems. Swarm and Evolutionary Computation, 64:100888, 2021. ISSN
> GECCO ’20, pp. 103–111. ACM, 2020. doi: 10.1145/3377930.3390221. URL http://dx. 2210-6502. doi: 10.1016/j.swevo.2021.100888. URL http://dx.doi.org/10.1016/j.
> doi.org/10.1145/3377930.3390221. swevo.2021.100888.
> Jonathan Gordon, Wessel P. Bruinsma, Andrew Y. K. Foong, James Requeima, Yann Dubois, and Theodore Papamarkou, Jacob Hinkle, M. Todd Young, and David E. Womble. Challenges in markov
> Richard E. Turner. Convolutional conditional neural processes, 2019. URL https://www. chain monte carlo for bayesian neural networks. Statistical Science, 37(3), August 2022. ISSN
> repository.cam.ac.uk/handle/1810/300991. 0883-4237. doi: 10.1214/21-sts840. URL http://dx.doi.org/10.1214/21-sts840.
> Riccardo Guidotti, Anna Monreale, Salvatore Ruggieri, Franco Turini, Fosca Giannotti, and Dino Shahin Rostami and Alex Shenfield. A multi-tier adaptive grid algorithm for the evolutionary
> Pedreschi. A survey of methods for explaining black box models. ACM Computing Surveys, 51(5): multi-objective optimisation of complex problems. Soft Computing, 21(17):4963–4979, 2016.
> 1–42, August 2018. ISSN 1557-7341. doi: 10.1145/3236009. URL http://dx.doi.org/ ISSN 1433-7479. doi: 10.1007/s00500-016-2227-6. URL http://dx.doi.org/10.1007/
> 10.1145/3236009. s00500-016-2227-6.
> 11 12
> Learned CMA-ES Paper (pages 9–12 of 13)
> 46
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> Thomas Philip R´unarsson and Xin Yao. Stochastic ranking for constrained evolutionary optimization.
> IEEE Transactions on Evolutionary Computation, 4(3):284–294, 2000. ISSN 1089-778X. doi:
> 10.1109/4235.873238. URL http://dx.doi.org/10.1109/4235.873238.
> Volkmar Sauerland, Claudine von Hallern, Iris Kriest, and Julia Getzlaff. A cma-es algorithm
> allowing for random parameters in model calibration. Journal of Advances in Modeling Earth
> Systems, 15(8), August 2023. ISSN 1942-2466. doi: 10.1029/2022ms003390. URL http:
> //dx.doi.org/10.1029/2022ms003390.
> Andr´e Thomaser, Marc-Eric Vogt, Anna V. Kononova, and Thomas B¨ack. Transfer of Multi-
> objectively Tuned CMA-ES Parameters to a Vehicle Dynamics Problem, pp. 546–560. Springer
> Nature Switzerland, 2023. ISBN 9783031272509. doi: 10.1007/978-3-031-27250-9 39. URL
> http://dx.doi.org/10.1007/978-3-031-27250-9_39.
> Joaquin Vanschoren. Meta-Learning, pp. 35–61. Springer International Publishing, 2019. ISBN
> 9783030053185. doi: 10.1007/978-3-030-05318-5 2. URL http://dx.doi.org/10.
> 1007/978-3-030-05318-5_2.
> Teppei Yamaguchi, Kento Uchida, and Shinichi Shirakawa. Improvement of sep-cma-es for opti-
> mization of high-dimensional functions with low effective dimensionality. In 2022 IEEE Sympo-
> sium Series on Computational Intelligence (SSCI), pp. 1659–1668. IEEE, December 2022. doi:
> 10.1109/ssci51031.2022.10022244. URL http://dx.doi.org/10.1109/ssci51031.
> 2022.10022244.
> Qiang Yang, Wei–Neng Chen, Jeremiah D. Deng, Yun Li, Tianlong Gu, and Jun Zhang. A level-
> based learning swarm optimizer for large-scale optimization. IEEE Transactions on Evolutionary
> Computation, 22(4):578–594, August 2018. ISSN 1941-0026. doi: 10.1109/tevc.2017.2743016.
> URL http://dx.doi.org/10.1109/tevc.2017.2743016.
> Zhi-Hui Zhan, Jian-Yu Li, Sam Kwong, and Jun Zhang. Learning-aided evolution for optimization.
> IEEE Transactions on Evolutionary Computation, 27(6):1794–1808, December 2023. ISSN
> 1941-0026. doi: 10.1109/tevc.2022.3232776. URL http://dx.doi.org/10.1109/tevc.
> 2022.3232776.
> 13
> Learned CMA-ES Paper (page 13 of 13)
> 47
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> Review
> {
> "Summary": "The paper presents an anchored residual evolution strategy that augments a separable CMA-ES
> core with three small neural gate modules whose final layers are zero-initialized. This anchoring ensures
> exact recovery of classical CMA-ES during initialization or when gates are forced to identity, enabling a
> clean decomposition of performance gains. The authors meta-evolve 364 parameters on 2–10 D analytic
> functions, freeze them, and transfer to a 704 D Brax Ant policy search. Against a pinned same-seed CMA-ES
> baseline (normalized to 1.0), the full method scores 1.191× while the anchor alone scores 1.107×, showing
> 55.8% of the margin stems from the classical backbone. The learned residual adds an early improvement that
> shrinks with budget and even reverses on final return. Detailed ablations isolate the roles of mirroring,
> path normalization, covariance gating, and stall re-expansion, and in-distribution tests confirm genuine
> meta-learning gains on analytic tasks.",
> "Strengths": [
> "Innovative anchored-residual design that exactly recovers CMA-ES at initialization to isolate learned
> contributions.",
> "Rigorous performance decomposition quantifying classical vs. learned gains under the same code path and
> seeds.",
> "Comprehensive ablations (covariance gating, mirroring, path normalization, stall re-expansion) with
> controlled budgets and seeds.",
> "Budget- and metric-dependent analysis showing early learned benefits but late reversal, highlighting
> evaluation pitfalls.",
> "Clear, well-organized writing and reproducible protocol with transparent reporting of negative or
> conditional results."
> ],
> "Weaknesses": [
> "Empirical evaluation is limited to a single high-dimensional control task (Brax Ant) and one policy
> class; generality is untested.",
> "Each configuration was meta-trained only once, so meta-training seed variability and hyperparameter
> sensitivity are not quantified.",
> "The unanchored baseline differs in architecture and parameter count, confounding the isolation of
> anchoring effects.",
> "No intermediate-dimension transfers, comparisons to low-rank/full-covariance ES, or alternative
> learned-optimizer frameworks.",
> "Learned gate modules remain largely a black box with no fine-grained interpretability of what the 364
> parameters encode."
> ],
> "Originality": 3,
> "Quality": 3,
> "Clarity": 4,
> "Significance": 3,
> "Questions": [
> "Can you report performance variance over multiple independent meta-training runs to account for seed
> effects?",
> "Have you evaluated transfer on any other high-dimensional tasks or different policy architectures to
> test generality?",
> "Is it possible to construct a parameter-matched unanchored baseline (same architecture/count) for a
> cleaner anchoring ablation?",
> "Have you tried intermediate-dimension transfers (e.g., 50–100 D) to assess how the learned residual
> scales with problem size?",
> "Can you provide diagnostics on the learned gates (e.g., distributions over generations) to shed light
> on their functional role?"
> ],
> "Limitations": [
> "Scope restricted to one ES variant (separable CMA-ES), one RL environment, and one budget regime;
> limits external validity.",
> "Single-run meta-training underestimates overall uncertainty and may introduce selection bias in
> reported gains.",
> "Ablation setups retrained under new seeds confound mechanism impact with training noise.",
> "Anchoring control relies on multiplicative zero-initialized gates, which may not generalize to all
> learned-optimizer architectures.",
> "Transfer improvement is modest and budget-sensitive, highlighting the need for further architectural or
> domain expansions."
> ],
> "Ethical Concerns": false,
> "Soundness": 3,
> "Presentation": 4,
> "Contribution": 3,
> "Overall": 6,
> "Confidence": 4,
> "Decision": "Accept"
> }
> 48
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> C.3. Error Diffusion ALIFE Paper
> 2. For the classiﬁcation setting, three domain-speciﬁc in- dom feedback. Building on this framework, our work ex-
> novations: layer-speciﬁc sigmoid widths, batch-centered tends Error Diffusion to multi-class classiﬁcation and rein-
> class error, and asymmetric initialization, whose cross- forcement learning.
> Diffusing Blame: Task-Dependent Credit Assignment taskshift qualitativelyablation revealswiththattaskcredit-assignmentdifﬁculty. bottlenecks Reinforcement learning without backpropagation.
> in Biologically Plausible Dual-Stream Networks Evolution Strategies (ES) provide a gradient-free alternative 3. Cross-domain evaluation shows that ED-PPO achieves
> to policy gradient methods (Salimans et al., 2017), but scale returns comparable to backpropagation-based and DFA-
> poorly with parameter count. Proximal Policy Optimization Yutaro Yamada, Luca Grillotti, Rujikorn Charakorn, Sebastian Risi, David Ha, Robert Tjarko Lange based PPO on continuous control tasks such as Ant, Hu-
> (PPO) is a widely used policy gradient method (Schulman manoid, and HalfCheetah. On the more complex Craftax
> et al., 2017) that typically relies on backpropagation for Sakana AI, Tokyo, Japan task, ED-PPO achieves performance comparable to DFA.
> gradient computation. Biologically plausible alternatives yutaro.yamada.y@gmail.com, robert@sakana.ai
> for RL credit assignment have been proposed—reward-
> Related Work modulated Hebbian rules can solve decision-making tasks
> by gating synaptic updates with a global reward signal (Dale, 1935; Eccles, 1976; Strata and Harvey, 1999). This Abstract The weight trans- Biologically plausible learning rules. (Pfeiffer et al., 2010), and Izhikevich (2007) showed that constraint shapes cortical computation through balanced ex- port problem has motivated a family of alternatives to back- Biological neural circuits obey Dale’s principle: each neu- linking spike-timing-dependent plasticity with dopamine citation and inhibition (Markram et al., 2004). Artiﬁcial neu- ron’s synapses are uniformly excitatory or inhibitory. Arti- propagation (Crick, 1989; Bengio et al., 2015). Feedback signaling addresses the distal reward problem—but these ral networks, by contrast, freely assign positive and negative ﬁcial networks that respect this constraint must coordinate Alignment uses ﬁxed random backward weights (Lillicrap methods have not been demonstrated with deep networks separate excitatory and inhibitory populations, fundamen- weights to any connection, a simpliﬁcation that enables efﬁ- et al., 2016), and Liao et al. (2016) showed that approxi- tally changing how credit is assigned during learning. Sev- on continuous-control benchmarks. We replace backpropa- cient credit assignment via backpropagation but bears little mate weight symmetry can support learning in deeper net- eral biologically plausible learning rules avoid backpropa- gation in PPO’s gradient computation with Error Diffusion, resemblance to biological learning. Training deep networks works. DFA projects output errors directly to hidden lay- gation’s weight transport requirement, but it has been dif- evaluating on Brax physics tasks (Freeman et al., 2021) and with backpropagation requires the backward pass to use ex- ﬁcult to achieve strong performance under Dale’s princi- ers (Nøkland, 2016), and Launay et al. (2020) demonstrated Craftax (Matthews et al., 2024). weight trans- act transposes of forward weight matrices, the ple beyond MNIST. Error Diffusion (ED) was originally that DFA scales to modern architectures, including trans-
> proposed in a dual-stream excitatory/inhibitory architecture, port problem, which lacks biological support (Crick, 1989; formers and convnets. Difference target propagation com- where learning is driven by routing global error signals to Method Bengio et al., 2015). Several alternatives address this con- putes layer-wise learning targets instead of gradients (Lee all layers without transporting transposed forward weights cern. Feedback Alignment (FA) replaces backward weights Dual-Stream Architecture et al., 2015). Predictive coding provides a complemen- or relying on random feedback matrices. Whether such
> with ﬁxed random matrices (Lillicrap et al., 2016), Direct a rule can scale under Dale’s principle across both super- tary framework through local Hebbian updates (Whittington We adopt the original Error Diffusion formulation proposed Feedback Alignment (DFA) routes output errors directly to vised classiﬁcation and reinforcement learning remains un- and Bogacz, 2017), and dendritic cortical microcircuit mod- by (Kaneko, 2000). To enforce Dale’s principle, we split known. Here, we introduce modulo error routing to extend each hidden layer through ﬁxed random matrices (Nøkland, els approximate backpropagation with segregated dendrites each layer into an excitatory and inhibitory stream. (p) (n) Error Diffusion beyond binary classiﬁcation, and show that 2016), and predictive coding frameworks approximate back- (Sacramento et al., 2018). Bartunov et al. (2018) found that i The forward pass for layer computes: a dual-stream excitatory/inhibitory architecture trained with propagation through local Hebbian updates (Whittington biologically motivated algorithms generally struggle to scale 96.7% this method achieves on MNIST and establishes a
> and Bogacz, 2017; Sacramento et al., 2018). While these 61.7% baseline on CIFAR-10, demonstrating that representa- beyond simple benchmarks, and Richards et al. (2019) ar- = + pi bp) ϕi(+pi−1Wpp methods avoid weight transport, none enforce Dale’s princi- −ni−1Wnp tion learning is possible even when strictly enforcing Dale’s gued that biological plausibility and computational perfor- = + ni bn) principle. For the classiﬁcation setting, we introduce three ple: all allow arbitrary sign weights. Error Diffusion (ED), ϕi(+ni−1Wnn −pi−1Wpn mance need not be in tension if the right architectural prin- domain-speciﬁc innovations: layer-speciﬁc sigmoid widths, originally proposed by (Kaneko, 2000), is a local learn- ciples are identiﬁed. Filipovich et al. (2022) demonstrated 0 batch-centered class error signals, and asymmetric initial- where all weight matrices Wpp, Wnp, Wnn, Wpn ing rule in which weight updates depend on presynaptic ≥ DFA on photonic hardware, showing that bioplausible rules ization, and ablation analysis reveals that their relative im- element-wise, bias parameters and are not neces- bp bn activity, a postsynaptic activation derivative, and a single reverses portance between MNIST and CIFAR-10, expos- can enable novel computing substrates. sary non-negative, is a layer-speciﬁc activation function. ϕi global error sign (Fujita, 2026). This locality makes ED ing task-dependent credit-assignment bottlenecks invisible to
> The negation signs in front of and are structural Wnp Wpn single-benchmark evaluation. In reinforcement learning, we naturally compatible with biological constraints, but prior
> (hardcoded), ensuring that cross-stream connections are in- integrate ED with Proximal Policy Optimization (PPO) and Song et al. (2016) Dale’s principle in neural networks. work demonstrated its effectiveness only on binary classiﬁ-
> evaluate it on continuous-control tasks in Google Brax and hibitory while all learnable parameters remain non-negative. introduced a framework for training excitatory-inhibitory re- cation and a simple classiﬁcation task like MNIST (Fujita, on Craftax, an open-ended exploration task. We show that This dual-stream design requires four weight sub-matrices current networks with Dale’s principle, separating synap- 2026). The central question we address is whether a bio- ED-PPO achieves competitive performance relative to Direct per layer, resulting in more parameters than an un- tic magnitudes from signs by parameterizing weights as logically plausible architecture that enforces Dale’s princi- Feedback Alignment, a backpropagation-free baseline. ∼4× + + constrained single-stream network of the same width (e.g., W = W W where contains trainable con- ple can achieve competitive performance across both super- ⊙M, vs for DFA on the same architecture). M nection magnitudes and is ≥0a ﬁxed sign mask encoding vised classiﬁcation and reinforcement learning, and what the ∼32M ∼8M
> each unit’s excitatory or inhibitory identity. Cornford et al. task-dependent importance of its components reveals about Error Diffusion Update Rule (2021) proposed Dale’s ANNs, feedforward architectures Introduction credit assignment under biological constraints. We make
> In biological neural circuits, the separation of excitatory and three contributions: with separate excitatory and inhibitory populations that can The ED update replaces backpropagated layerwise errors
> inhibitory neuronal populations is a fundamental organiza- learn comparably to standard ANNs by treating inhibition with an output-space error signal that is routed directly to
> tional principle, formalized as Dale’s principle: each neu- as a normalization mechanism. Grossberg (1987) explored hidden units. While the original formulation was developed
> ron releases the same neurotransmitter at all of its synapses 1. A dual-stream excitatory/inhibitory ED architecture that competitive learning in networks with separate excitatory for binary classiﬁcation (Kaneko, 2000; Fujita, 2026), we
> extend ED to multi-output prediction by assigning each hid- and inhibitory interactions. Error Diffusion (Fujita, 2026; maintains non-negative weights across classiﬁcation
> ©2026 Yutaro Yamada, Luca Grillotti, Rujikorn Charakorn,
> Kaneko, 2000) is also naturally Dalean, representing posi- den unit a ﬁxed output channel. For a hidden unit i, we de- (MNIST, CIFAR-10) and reinforcement learning (Brax Sebastian Risi, David Ha, Robert Tjarko Lange. Published under
> a Creative Commons Attribution 4.0 International (CC BY 4.0) li- locomotion tasks, Craftax) by extending the original ED tive and negative components through separate non-negative ﬁne r(i) = i mod C, where C is the output dimension, and
> cense. via modulo error routing. populations while avoiding both weight transport and ran- use the routed error component sr(i) as its learning signal.
> nected layers, including the output layer; MNIST uses only Experimental Protocol
> the α = 6.0 fully connected setting. Classiﬁcation. We evaluate six variants on MNIST and
> CIFAR-10: (1) the proposed ED with all three innovations,
> Batch-centered class error signals. For the 10-way clas- (2) a DFA baseline using ﬁxed random feedback matrices
> siﬁcation tasks, the ED update uses independent sigmoid without Dale’s constraints, (3) the seed ED (the dual-stream
> output activations rather than a softmax. For a mini-batch architecture without any of the three innovations, i.e., using
> of size B, we form a one-vs-all output error Eb,c = 1[yb = uniform sigmoid width α = 1.0, raw error signals, and sym-
> c]
> for−ˆyb,c,class c.whereSinceˆyb,ceachis theexamplepositive-streamcontributesoutputone targetactivationclass metricnovationinitialization),each: (4) no batch-centeredand three ablationsclassremovingerror, (5)onesym-in-
> and nine non-target classes, the raw one-vs-all error can con- metric init, and (6) uniform sigmoid width. Each variant
> tain a strong class-wise offset, especially early in training. is trained for 250 epochs with ﬁve random seeds per task (60
> We therefore subtract the mini-batch mean separately for runs total). For classiﬁcation, weights are clamped to a ﬂoor
> of update to enforce Dale’s principle. = 10−4 after each This makes the each class: ˜Eb,c Eb,c
> error signal zero-mean across−1B the b′=1batchEb′,c.for every class, re-
> ducing persistent suppression orPB excitation of class channels We compare ED-PPO against Reinforcement learning.
> caused by the one-vs-all target imbalance. The centered sig- BP-PPO (standard backpropagation), DFA-PPO (Direct
> nal is then applied directly to the output layer, with the neg- Feedback Alignment) (Nøkland, 2016), and ES (Sali-
> ative stream receiving the opposite sign; hidden fully con- mans et al., 2017) on three Brax (Freeman et al., 2021)
> nected units and convolutional channels receive class-routed locomotion tasks (Ant, HalfCheetah, Humanoid). On
> copies of the same centered error. Craftax (Matthews et al., 2024), we compare against BP-
> PPO and DFA-PPO. Additionally, we evaluate a non-Dalean Figure 1: Overview of the dual-stream Error Diffusion framework. The excitatory/inhibitory architecture maintains Left: variant of ED-PPO, in which weights are not constrained to For convolutional and Asymmetric E/I initialization. separate streams (p, n) with four non-negative weight matrices per layer, enforcing Dale’s principle structurally. The Center: be non-negative, and denote it as “ED-PPO (non-Dalean)”. fully connected layers, weight parameters are initialized Error Diffusion update broadcasts output error directly to hidden layers without weight transport or random feedback matrices.
> Each algorithm is evaluated with ﬁve random seeds per [0, from a non-negative uniform distribution in 1) and scaled The shared architecture is applied to classiﬁcation (with domain-speciﬁc sigmoid widths, batch-centered class error, Right: environment. We report the ﬁnal episode reward (the re- by the inverse square root of the fan-in. Hidden excita- and asymmetric initialization) and reinforcement learning (with PPO integration).
> ward at the last evaluation checkpoint) averaged over seeds; tory weights are then scaled by while in- (Wpp, Wnn) 1.5×, α = statistical comparisons use Welch’s t-test with 0.05. hibitory weights are scaled by giving an (Wnp, Wpn)
> The RL experiments use a separate training and evalua- 3 : expected E/I scale ratio of 1. The ﬁnal 0.5×,fully connected (For other design choices of this error routing and why, see This deterministic modulo routing provides coarse output-
> pipeline from the classiﬁcation experiments, with both output layer uses symmetric initialization Figure 4) associated credit assignment without transporting forward
> sharing the same dual-stream ED architecture but citatory and inhibitory weights) to avoid output(1.0× saturation.for both ex- tionpipelines S For a mini-batch, let denote the output error weights or using random feedback matrices. Unlike DFA, ∈RB×C differing in activation functions, normalization, and opti- M signal, and let be the ﬁxed routing matrix whose ﬁxed feedback matrices are random, ED uses a struc-
> RL Extension: ED-PPO mization procedure as described above. = r(i)∈{0, = with 1 iff 1}H×Cc. The routed hidden error is tured correspondence between hidden units and output di- Mic
> reinforcement learning, we integrate a Dale-constrained mensions. For
> dual-stream Error Diffusion (ED) architecture into PPO. Results R = SM ⊤, (1)
> The policy and value networks are both dual-stream MLPs Classiﬁcation: Main Comparison Innovations
> and the corresponding matrix of local postsynaptic drives is Classiﬁcation-Speciﬁc with separate positive and negative pathways and non- Figure 2 summarizes accuracy across all six classiﬁcation For the classiﬁcation setting (MNIST, CIFAR-10), we in- negative synaptic weights. Each layer computes excitatory- variants. The proposed ED achieves on MNIST troduce three domain-speciﬁc innovations that address fail- = (2) Up ϕ′(Zp) minus-inhibitory preactivations for the positive and negative 61.7 0.7% and on CIFAR-10, a substantial96.7±0.1% improvement ⊙R, ure modes speciﬁc to multi-class classiﬁcation under Dale’s ± streams, applies a width-scaled sigmoid nonlinearity, and over seed ED and DFA achieves where is the positive-stream preactivation ma- Zp principle. These three innovations are speciﬁc to the classi- (50.4±9.8% ∈RB×H ˆy = y+ combines the ﬁnal streams as Both Dale im- higher accuracy on both tasks (97.6%11.6±2.2%). and 69.1%) but vio- trix. ﬁcation setting and are not used in the RL extension. −y−. plementations initialize the two streams symmetrically from lates Dale’s principle, requiring negative weights. Let denote the positive-stream presynaptic Ap
> = = the same observation, x, after optional stream p0 n0 0.9 The accuracy gap between ED and∼2.84M DFA widens from activations ∈RB×Kfeeding the layer. In fully connected notation, the
> In the classiﬁcation archi- normalization, rather than splitting inputs with ReLU. Layer-speciﬁc sigmoid widths. 7.4 pp on MNIST to pp on CIFAR-10, suggesting that the positive-to-positive ED update is
> = 1/(1 + tecture, with layer-type-speciﬁc ED replaces backpropagation through hidden layers after e−2z/αi) ϕi(z) cost of maintaining non-negative weights grows with task
> ∆Wpp ∝A⊤p Up. (3) sigmoidsigmoid networkswidth αi.is wellThe documentedvanishing gradient(Roodschildproblemet al.,in thesignal.PPO’sForobjectivevector-valuedfunctionpolicysuppliesoutputs,an theoutput-levelerror is routederror difﬁculty.still far fromWe competitivenote that ∼62%comparedtest accuracyto traditionalon CIFAR-10gradient-is
> In the supervised image classiﬁers, C = 10 and S is the 2020; Ding et al., 2018), and various parameterized activa- to hidden units by output class/channel assignment; for the based methods. That being said, this is the ﬁrst time ED has
> batch-centered one-vs-all classiﬁcation error. In Craftax pol- tions have been proposed (Szandała, 2020; Hammad, 2024). scalar value network, the error is broadcast to all hidden been successfully applied to convolutional neural networks.
> C icy networks, is the number of discrete action logits. In Since the sigmoid derivative directly gates the error signal in units. All weight matrices are initialized non-negative us- Previously, Fujita (2026) obtained
> MuJoCo policy networks, C is the policy distribution pa- ED (Eq. 3), gradient attenuation is especially severe: post- ing absolute Gaussian samples, with separate scaling for ex- CIFAR-10 using a purely feedforward∼55.2%MLPtestwithaccuracyﬂattening.on
> rameter dimension for the TanhNormal policy. hoc analysis reveals
> Each local weight update is proportional to presynaptic layer. Wider sigmoids a 25×(largerdecayαi)frommaintainoutput tolargerﬁrst deriva-hidden citatorynon-negativeand inhibitoryafter optimizerpathways,updates.and weights are clamped Classiﬁcation: Cross-Task Ablation Reversal
> activity, and this routed postsynaptic drive, with stream- tives, preventing premature saturation. We set α = 3.0 for Figure 1 summarizes the error diffusion training proce- In the classiﬁcation setting, ablation analysis reveals that the
> speciﬁc signs for the dual positive/negative pathways. CIFAR-10 convolutional layers and α = 6.0 for fully con- dure and methodological contributions. importance hierarchy of the three innovations reverses be-
> Error Diffusion ALIFE Paper (pages 1–4 of 10)
> 49
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> Table 1: Ablation effect sizes (∆= proposed ED minus abla- ED-PPO (non-Dalean) (p = 0.010), demonstrating that ran- MNIST CIFAR-10
> tion, in percentage). Negative values indicate accuracy loss. dom feedback pathways that sufﬁce for simpler tasks fail on
> complex, open-ended environments. 1.0 96.7% 97.6% 96.4% 96.7% 95% 1.0
> Removed Component MNIST ∆ CIFAR-10 ∆ ED-PPO (non-Dalean) exhibits higher variance than BP-
> PPO on Ant and HalfCheetah, though on Humanoid its Batch-Centered Error 0.8 0.8 −0.3 −47.9 Layer-Speciﬁc Widths variance is lower (1144 vs 3252), suggesting that the ED 69.1% −71.4 +0.0 −15.1 Asymmetric Init gradient signal can introduce additional stochasticity. The 61.7% −5.5 50.4% 56.2% 46.6% p = Craftax shortfall 0.002) and higher variance 0.6 0.6 (−4.0, across environments suggest that the coarse modular error Accuracy Accuracy
> tween tasks (Figure 2, Table 1). routing of ED may be less reliable for tasks requiring ﬁne- Test Test 0.4 0.4 25.3% grained temporal credit assignment or stable convergence.
> Removing layer-speciﬁc sigmoid widths is catas-MNIST.
> 13.8%
> ∆=trophic (96.7% pp), collapsing ac- DFA Across Domains 0.2 0.2 →25.3%, 11.6%curacy to near-chance. Removing −71.4batch-centered class error DFA provides an informative comparison point across both
> ∆=causes only pp, and symmetric initialization has classiﬁcation and RL. In classiﬁcation, DFA achieves the
> +0.0no measurable effect −0.3(∆= pp). Gradient ﬂow regula- highest accuracy (97.6% MNIST, 69.1% CIFAR-10) but vi-
> tion is the sole bottleneck on this easier task. 0.0 ProposedED DFA SeedED MCENo Sym.Init WidthUnif. 0.0 ProposedED DFA SeedED MCENo Sym.Init WidthUnif. olates Dale’s principle. On Craftax, DFA-PPO is the weak-
> est method (19.8 vs BP-PPO 27.0, ED-PPO 23.0), demon-
> The hierarchy reverses. Removing Figure 2: Classiﬁcation accuracy across all six variants on MNIST (left) and CIFAR-10 (right). Error bars:CIFAR-10. batch- strating that random feedback pathways that work for super-
> centered error is now the most destructive ablation seeds. The ablation importance hierarchy reverses between tasks: layer-speciﬁc widths dominate on MNIST, batch-centered±1 std over 5 vised learning can fail for complex RL. This cross-domain
> ∆=(61.7% pp), causing four of class error dominates on CIFAR-10. ﬁve pattern—DFA strong on classiﬁcation, weak on complex −47.9 ∆=seeds to →13.8%,collapse. Uniform sigmoid width causes RL—parallels the ﬁnding that method components assume
> 12K
> 6K ∆= pp, and symmetric initialization causes different importance under different task demands. −5.5 8K
> 10K
> 5Kpp.−15.1All three innovations contribute, but their relative order-
> 8King changes fundamentally. 6K 4K Post-hoc Analysis Return Return Return
> 3K 6K
> 4K Post-hoc analysis of Surrogate gradient attenuation.
> 2K This reversal reﬂects qualitatively differ-Interpretation. 4K training dynamics on CIFAR-10 (Figure 6a) reveals local 2K Episode Episode Episode (non-Dalean) (non-Dalean) (non-Dalean)ent credit-assignment bottlenecks. MNIST’s well-separated 1K
> 2K 6.6 surrogate gradient magnitudes dropping from at 10−3
> 0features allow learning even without error centering; but × BP-PPOED-PPOED-PPOESDFA-PPO BP-PPOED-PPOED-PPOESDFA-PPO BP-PPOED-PPOED-PPOESDFA-PPO 0 2.7 the output layer to at the ﬁrst hidden layer—a 10−4 0
> 0 50M 100M 200M 300M 350M 400M 0 50M 100M 150M 200M 250M 300M 350M 400M 0 50M 100M 150M 200M 250M 300M 350M 400Mwithout wide sigmoids, the derivatives vanish entirely. On attenuation. This ×pattern is visible from epoch 3, in- Environment150M steps250M Environment steps Environment steps
> CIFAR-10, higher inter-class similarity makes the 9:1 er- 25×dicating a structural property of sigmoid-gated error diffu- (a) Ant (b) Humanoid (c) HalfCheetahror imbalance overwhelming: batch-centering becomes es- sion that motivates layer-speciﬁc widths. Per-layer learn-
> sential to prevent uniform output suppression. The reversal ing rate multipliers early, output) partially com- Figure 3: Episode return across three RL environments. Error bars indicate standard deviation over 5 seeds.demonstrates that evaluating biologically plausible methods 0.5× ±1 pensate but cannot substitute(3.0× for wider sigmoid derivatives.
> on a single benchmark may obscure critical trade-offs. The near-zero generalization gap (0.98%) despite ∼32M for biological E/I balance, which involves cell counts, ﬁr- nominal parameters is consistent with underﬁtting rather DiscussionRL: Cross-Domain Generalization
> ing rates, and synaptic strengths jointly. The convergence than overﬁtting, but the cause is ambiguous: the 37.3% The central ﬁnding of this work is that the Summary.Figures 3 and 5 report performance across four RL tasks. toward balance explains the task-dependent importance of of weights at ﬂoor reduces effective capacity well (10−4) dual-stream ED architecture is viable across both classiﬁ-ED-PPO is strongest on HalfCheetah, where it outperforms asymmetric initialization: on MNIST, balance is reached below the nominal parameter count, so the limitation may cation and reinforcement learning, but the auxiliary mech- 691 3520 p ` exactly once as `{cycle_index:02d}_{slug(title, fallback=idea_id)}`
> - `slug(title, fallback=idea_id)` rule: lowercase, replace each non-`[a-z0-9]` run with `_`, trim
> leading/trailing `_`
> - write local memory only to `{{GLOBAL_RESULTS_DIR}}/ideas/ /memory.md` (create if missing;
> append-only)
> - never write memory to any other `{{GLOBAL_RESULTS_DIR}}/ideas/ /memory.md` path for this cycle (no
> temporary/fallback dir)
> - before exit, verify there is only one cycle-local memory target for this idea; if an alternate
> same-cycle dir was created, merge/move its memory into the canonical dir and stop using the alternate dir
> - Also append each idea-memory event to:
> - `{{GLOBAL_MEMORY_MD_FILE}}`
> - `{{GLOBAL_MEMORY_JSONL_FILE}}`
> - Treat `{{GLOBAL_MEMORY_MD_FILE}}` as the single campaign-level markdown memory file.
> - `{{GLOBAL_MEMORY_MD_FILE}}` must live in the same results-root directory as `outer_state.json`; never
> create `memory.md` under `{{GLOBAL_RESULTS_DIR}}/ideas/`.
> - `{{GLOBAL_MEMORY_JSONL_FILE}}` is the authoritative cross-idea memory log.
> - Use this compact recent-memory summary first, then read the files above if you need more detail:
> {{GLOBAL_MEMORY_RECENT_SUMMARY}}
> - After appending the cycle's regular memory event, follow the `memory-log` skill's "Update distilled
> conclusions" step: apply the (a)/(b)/(c)/(d) gate, and if an update is warranted, run `--write-conclusions`
> (single paragraph, ≤300 words, ≤3 retries on validation failure). The helper paths are
> `{{CONCLUSIONS_MD_FILE}}` and `{{CONCLUSIONS_HISTORY_FILE}}`; pass `--worker-index {{WORKER_INDEX}}`. Always
> include the gate decision (`gate= ; trigger= ; reason=...`) inside the
> regular memory event.
> - Record idea discoveries: alternatives considered, selected hypothesis, and why this cycle decision is best
> from recent evidence.
> - After writing `idea_bundle.json` and `target_rubrics.json`, append an entry with:
> - `idea_id`
> - hypothesis summary
> - key rubric choices (`id`, `type`)
> - next execution recommendation
> - Directory mismatch is a hard failure: do not finish the idea step until local memory uses one canonical
> idea directory for this cycle.
> # Constraints
> - No user interaction.
> - No prose-only response.
> - No computational probes or command execution.
> - If uncertain, choose the most actionable next experiment and continue.
> 65
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> F.3. Execution Agent
> Execution Agent Prompt
> # Role
> You are an autonomous scientist/engineer running an iterative optimization campaign.
> Goal: improve objective score while preserving correctness and satisfying rubrics.
> Use `edit-program`, `evaluate-program`, `verify-rubrics`, and `memory-log` skill workflows for execution.
> # Task Context
> ## Goal
> {{TASK_GOAL}}
> # Active Idea
> Attempt ID: `{{ATTEMPT_ID}}`
> Idea ID: `{{IDEA_ID}}`
> Idea bundle (`{{ACTIVE_IDEA_FILE}}`):
> ```json
> {{ACTIVE_IDEA_JSON}}
> ```
> Target rubric bundle (`{{ACTIVE_RUBRICS_FILE}}`):
> ```json
> {{ACTIVE_RUBRICS_JSON}}
> ```
> # Overall Pipeline
> `edit-program -> evaluate-program -> verify-rubrics`
> For each iteration:
> 1. `edit-program`: create next variant at `{{RESULTS_DIR}}/program_ /` (never overwrite old dirs). Code
> lives in `program_ /src/**` and metadata in `program_ /program_spec.toml`.
> 2. `evaluate-program`: run `{{TASK_DIR}}/evaluate.py` and collect `metrics.json`, `correct.json`, logs.
> 3. `verify-rubrics`: use the `verify-rubrics` skill workflow to check rubric compliance with evidence.
> 4. Immediately update `./memory.md` using `memory-log` with a structured evidence entry for this iteration.
> 5. Immediately write per-iteration rubric artifact to `{{RESULTS_DIR}}/program_ /rubric_status.json`.
> 6. Update `{{RESULTS_DIR}}/rubric_status.json` to match the latest evaluated `program_ ` status.
> 7. Only then decide next change from evidence.
> # Hard Iteration Controls
> - Always run `evaluate-program -> verify-rubrics` as one atomic pair for each `program_ `.
> - Never start editing `program_ ` before writing rubric artifacts for `program_ `.
> - Stop the attempt immediately when latest `rubric_status.json` has `overall_pass: true`.
> - Evaluation budget is based on calls, not folders: each command that runs `evaluate.py` consumes one
> evaluation, including `program_0`, retries, failed runs, overwrites, and repeated runs on the same
> `program_ `.
> - You may make at most `{{MAX_PROGRAM_EVALS_PER_ATTEMPT}}` total `evaluate.py` calls in a single attempt.
> - Do not evaluate `program_0` unless explicitly required by the task. Treat `{{RESULTS_DIR}}/program_0/` as
> the read-only implementation reference and use existing baseline artifacts if present.
> - If `{{MAX_PROGRAM_EVALS_PER_ATTEMPT}}` is `1`, create exactly one edited candidate (`program_1`), run
> `evaluate.py` exactly once on `program_1`, then verify rubrics, write memory, and stop if rubrics fail.
> - Do not re-run `evaluate.py` for the same program. Use the existing result, inspect logs if needed, verify
> rubrics from available evidence, write memory, and stop with `cap_reached`.
> - If the final allowed evaluation call fails any rubric or the evaluation itself fails, stop the attempt and
> report cap reached.
> # No Exploratory Execution
> - Do not run exploratory computation, timing probes, benchmarks, optimizer searches, ad hoc metric scripts,
> or local experiments outside the official `evaluate.py` call.
> - Do not run `python -c`, heredoc Python, notebooks, `pip install`, `pip`, `pytest`, `time`, SciPy
> optimizers, differential evolution, SLSQP probes, or direct task-function calls.
> - Before evaluation, use file reads and edits only. After evaluation, inspect only the artifacts and logs
> produced by that official evaluation.
> - The only allowed Python execution is the single official `evaluate.py` command, subject to the evaluation
> budget.
> # Memory Protocol
> - Continuously update `./memory.md` (create it if missing; append-only, never erase prior entries).
> - Keep this local file attempt-scoped in the current attempt directory (`ideas/ / /`), not as a
> campaign-global file.
> - Also append every major-step memory event to:
> - `{{GLOBAL_MEMORY_MD_FILE}}`
> - `{{GLOBAL_MEMORY_JSONL_FILE}}`
> - Treat `{{GLOBAL_MEMORY_MD_FILE}}` as the single campaign-level markdown memory file.
> - `{{GLOBAL_MEMORY_MD_FILE}}` must live in the same results-root directory as `outer_state.json`; never
> create `memory.md` under `ideas/` subdirectories.
> - Use `{{GLOBAL_MEMORY_JSONL_FILE}}` as the authoritative cross-idea memory log.
> - Start from this compact recent-memory summary before reading more from disk:
> {{GLOBAL_MEMORY_RECENT_SUMMARY}}
> - After every `evaluate-program -> verify-rubrics` pair, append one `Program Evaluations` entry with:
> - `program_ `
> - `combined_score`
> 66
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> - `correct`
> - rubric pass/fail summary
> - next recommendation
> - Log key discoveries/decisions for each major step (`edit`, `evaluate`, `verify`) before moving to the next
> step.
> - At attempt end (stop reason `rubric_pass` or `cap_reached`), migrate relevant findings from local
> `./memory.md` into global memory:
> - append one `idea_evaluation_summary` entry to `{{GLOBAL_MEMORY_MD_FILE}}`
> - append one matching structured event to `{{GLOBAL_MEMORY_JSONL_FILE}}`
> - include: best `program_ `, best metrics (`combined_score`, `correct`), final rubric status summary,
> key failures/learnings, and next recommendation
> - summarize/highlight only (do not copy-paste the full local memory file)
> # Start Point
> - Reference baseline project: `{{RESULTS_DIR}}/program_0/` (read-only baseline copy).
> - Entrypoint is defined by `program_0/program_spec.toml` (default: `src/program.py`).
> - Start by creating `{{RESULTS_DIR}}/program_1/` from `program_0`, then evaluate `program_1` exactly once.
> - Do not run a baseline `program_0` evaluation unless the task explicitly requires it; if you do, it
> consumes one `evaluate.py` call from the same budget.
> # Operating Rules
> - Never ask user questions.
> - If info missing, make best reasonable assumption and continue.
> - Prefer root-cause fixes; avoid random search edits.
> - Keep reasoning brief and concrete.
> - Do not chain edits without rubric verification for the just-evaluated program.
> - Before any command containing `evaluate.py`, check whether the total evaluation budget has already been
> consumed. If no budget remains, do not run the command.
> # Required Artifacts
> After **every** evaluation of `program_ `, write `{{RESULTS_DIR}}/program_ /rubric_status.json`.
> Before exit, ensure `{{RESULTS_DIR}}/rubric_status.json` exists and exactly matches the latest evaluated
> program rubric artifact.
> Artifact structure:
> ```json
> {
> "attempt_id": "{{ATTEMPT_ID}}",
> "idea_id": "{{IDEA_ID}}",
> "overall_pass": false,
> "rubrics": [
> {"id": "rubric_id", "pass": false, "evidence": "brief evidence"}
> ],
> "metrics": {"combined_score": 0.0, "correct": false},
> "next_recommendation": "short next step"
> }
> ```
> # Exit Checklist
> 1. Identify latest evaluated index ` `.
> 2. Confirm `program_ /rubric_status.json` exists.
> 3. Confirm `{{RESULTS_DIR}}/rubric_status.json` equals `program_ /rubric_status.json`.
> 4. Confirm latest `program_ ` evidence is logged in `./memory.md`.
> 5. Confirm relevant local-memory findings were migrated to global memory (`{{GLOBAL_MEMORY_MD_FILE}}` and
> `{{GLOBAL_MEMORY_JSONL_FILE}}`).
> 6. Confirm no more than `{{MAX_PROGRAM_EVALS_PER_ATTEMPT}}` total `evaluate.py` calls were made, including
> any `program_0` evaluation.
> 7. Confirm stop reason is one of: `rubric_pass` or `cap_reached`.
> F.4. Ablation-Study Agent
> Ablation-Study Agent Prompt
> # Role
> You are an autonomous agent implementing exactly one variant for a scientific comparison study.
> Use `edit-program`, `evaluate-program`, and `verify-rubrics` skill workflows for execution.
> # Variant Specification
> ```json
> {{VARIANT_SPEC}}
> ```
> # Rubric Contract
> ```json
> {{RUBRIC_CONTRACT}}
> ```
> # Attempt {{ATTEMPT_NUMBER}}
> {{ERROR_CONTEXT}}
> 67
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> # CRITICAL: Mandatory Workflow –- Do NOT Skip Any Step
> WARNING **WARNING: Ending your turn after editing without evaluating is a FAILURE.**
> The edit alone has ZERO value without evaluation and rubric verification.
> You will be re-launched if you stop early, wasting time and resources.
> You MUST execute ALL four steps below **in order** for each `program_ `.
> Do NOT stop or end your turn until the checklist is fully done (or the iteration cap is reached).
> ### STEP 1: Edit
> Create `./program_ /` by cloning from `program_ ` (never overwrite old dirs). Code lives in
> `program_ /src/**`.
> ### STEP 2: Evaluate (MANDATORY –- do NOT skip)
> Immediately after editing, call the `evaluate-program` skill to run `./evaluate.py` and collect
> `metrics.json`, `correct.json`, logs.
> ### STEP 3: Verify rubrics (MANDATORY –- do NOT skip)
> Immediately after evaluation, call the `verify-rubrics` skill to check rubric compliance and write
> `program_ /rubric_status.json` and `./rubric_status.json`.
> ### STEP 4: Decision gate
> If `overall_pass: true` →stop. If rubrics failed and cap not reached →start next iteration from Step 1.
> # Variant-Specific Instructions
> ## For Control Variants (`kind: "control"`)
> - `program_0` is the seed –- evaluate it as-is first.
> - If it passes rubrics, you are done.
> - If it fails only on analysis CSV rubrics, create `program_1` with the CSV writing fix.
> ## For Ablation Variants (`kind: "ablation"`)
> - `program_0` is the read-only seed (copy of the best program). Do NOT evaluate `program_0`.
> - Go straight to creating `program_1` by cloning `program_0` and removing/disabling ONLY the component
> declared in `target_components`.
> - Keep all unrelated logic unchanged.
> - Evaluate `program_1` and verify rubrics.
> - If rubrics fail, create `program_2` as a repair –- fix only the rubric failures.
> ## For Baseline Variants (`kind: "baseline"`)
> - `program_0` is the read-only seed (copy of the best program). Do NOT evaluate `program_0`.
> - Go straight to creating `program_1` by cloning `program_0` and implementing the simpler approach described
> in `implementation_plan`.
> - Evaluate `program_1` and verify rubrics.
> - If rubrics fail, create `program_2` as a repair –- fix only the rubric failures.
> # Required Analysis Outputs
> The seed program (`program_0`) has already been retrofitted to produce these CSV files automatically after
> evaluation. When you create `program_1` (or later iterations), preserve that CSV-writing code so the files
> continue to be generated. You may adapt the CSV-writing code if your variant change requires it (e.g., a
> removed component breaks a metric reference), but the file names, column schemas, and output structure must
> remain identical across all variants –- results are aggregated across variants for comparison.
> After evaluation, ensure these files exist under `analysis/` (situated inside the `program_ /` directory,
> alongside `src`, see example below):
> ### analysis/run_summary.csv
> Columns: `evaluation_id`, `variant_id`, `variant_kind`, `base_program`, `agent_name`, `correct`,
> `combined_score`, `overall_pass`, `primary_metric_name`, `primary_metric_value`, `program_dir`,
> `results_dir`
> ### analysis/metric_values.csv
> Columns: `evaluation_id`, `variant_id`, `metric_name`, `metric_value`, `is_primary_metric`
> ### analysis/timeseries.csv
> Columns: `evaluation_id`, `variant_id`, `series_name`, `step`, `value`
> (Write headers only if no timeseries data exists)
> ### analysis/rubric_status.json
> Must cover all 9 rubric families from the rubric contract.
> # Hard Iteration Controls
> - Always run `evaluate-program -> verify-rubrics` as one atomic pair for each `program_ `.
> - Never start editing `program_ ` before writing rubric artifacts for `program_ `.
> - Stop the attempt immediately when latest `rubric_status.json` has `overall_pass: true`.
> - Evaluation budget is based on calls, not folders: each command that runs `evaluate.py` consumes one
> evaluation, including `program_0`, retries, failed runs, overwrites, and repeated runs on the same
> `program_ `.
> - You may make at most **{{MAX_PROGRAM_EVALS}}** total `evaluate.py` calls.
> - If `{{MAX_PROGRAM_EVALS}}` is `1`, create exactly one edited candidate (`program_1`), run `evaluate.py`
> exactly once on it, then verify rubrics, write required artifacts, and stop if rubrics fail.
> - Do not re-run `evaluate.py` for the same program. Use the existing result, inspect logs if needed, verify
> rubrics from available evidence, write required artifacts, and stop with cap reached.
> 68
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> - If the final allowed evaluation call fails any rubric or the evaluation itself fails, stop the attempt and
> report cap reached.
> # No Exploratory Execution
> - Do not run exploratory computation, timing probes, benchmarks, optimizer searches, ad hoc metric scripts,
> or local experiments outside the official `evaluate.py` call.
> - Do not run `python -c`, heredoc Python, notebooks, `pip install`, `pip`, `pytest`, `time`, SciPy
> optimizers, differential evolution, SLSQP probes, or direct task-function calls.
> - Before evaluation, use file reads and edits only. After evaluation, inspect only the artifacts and logs
> produced by that official evaluation.
> - The only allowed Python execution is the official `evaluate.py` command, subject to the evaluation budget.
> # Start Point
> - Reference baseline project: `./program_0/` (read-only baseline copy).
> - Entrypoint is defined by `program_0/program_spec.toml` (default: `src/program.py`).
> - Read `./task.md` for variant-specific instructions (hypothesis, target components, implementation plan,
> what to do).
> - Read `./original_task.md` for the original task description and domain rubrics.
> - Read the `NUM_REPLICATIONS` environment variable. When set and >1, pass `--num_replications
> $NUM_REPLICATIONS` to every `evaluate.py` invocation to launch parallel SLURM jobs per evaluation.
> - **Control variants only:** evaluate `program_0` first to establish baseline metrics:
> ```bash
> $WRAPPER_PYTHON ./evaluate.py \
> --program_path ./program_0/src/program.py \
> --results_dir ./program_0 \
> ${NUM_REPLICATIONS:+--num_replications $NUM_REPLICATIONS}
> ```
> - **Ablation/baseline variants:** skip `program_0` evaluation –- go straight to creating `program_1` with
> the variant change, then evaluate `program_1`.
> # Operating Rules
> - Never ask user questions.
> - Do NOT invent additional experiments beyond the declared variant.
> - Do NOT optimize beyond making the variant valid and analyzable.
> - For ablations: change ONLY the declared component.
> - Preserve the CSV analysis infrastructure in the program.
> - Read variant metadata from environment variables: `VARIANT_ID`, `VARIANT_KIND`, `BASE_PROGRAM`,
> `EVALUATION_ID`.
> - Before any command containing `evaluate.py`, check whether the total evaluation budget has already been
> consumed. If no budget remains, do not run the command.
> F.5. Task-Drafting Agent
> Task-Drafting Agent Prompt
> # Role
> You are a task-bootstrap agent.
> Goal: from `task.md`, draft a runnable task scaffold for the optimization loop.
> Use the `draft-task` and `memory-log` skill workflows for required files, deterministic decisions, and
> traceability.
> # Task Context
> - Task directory: `{{TASK_DIR}}`
> - Task spec: `{{TASK_MD_PATH}}`
> - Goal section:
> {{TASK_GOAL}}
> - Rubrics section:
> {{TASK_RUBRICS}}
> # Required Outputs
> Create missing files only:
> - `{{DRAFT_CAPTURE_PATH}}`
> - `{{DRAFT_EVALUATE_PATH}}`
> - `{{DRAFT_PROGRAM_SPEC_PATH}}`
> - `{{DRAFT_ENTRYPOINT_PATH}}`
> Rules:
> - If either file already exists, do not edit it.
> - Treat every requirement in `task.md`, especially `Task-Drafting Requirements`, as a hard acceptance
> criterion.
> - Inspect and use named upstream resources as required by `task.md`; network access and read-only clones
> under `/tmp` are allowed. A missing local package does not establish that a resource is unavailable.
> - Do not silently replace a required benchmark, evaluator, dataset, protocol, or base implementation with a
> synthetic stand-in. If blocked, exit non-zero and report the exact blocker.
> - Ensure `{{DRAFT_PROGRAM_DIR}}` and `{{DRAFT_CODE_ROOT}}` exist when writing the baseline.
> - Write `program_spec.toml` using **exactly** the schema below. The field names
> are fixed: the entrypoint table requires `path` (string, relative to
> `code_root`), `language` (string), and `type` (`"script"` or `"module"`). Do
> **not** invent keys such as `file`, and do **not** set `type` to a language
> 69
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> name. `code_root` must be the literal string `"src"`.
> ```toml
> schema_version = 1
> code_root = "src"
> [entrypoint]
> path = "program.py"
> language = "python"
> type = "script"
> [copy]
> exclude = []
> include = []
> [integration]
> kind = "generic"
> notes = ""
> ```
> - Before finishing, validate the scaffold with the repository's canonical program-spec loader, and run
> candidate code in a killable, resource-limited subprocess isolated from evaluator-private data.
> - `capture.py` should provide an `OutputCapture` context manager compatible with existing tasks.
> - `evaluate.py` must support:
> - `--program_path `
> - `--results_dir `
> - `evaluate.py` must write:
> - `metrics.json` with numeric `combined_score`
> - `correct.json` with boolean `correct`
> - Keep baseline behavior deterministic and lightweight.
> - Prefer minimal dependencies.
> # Memory Protocol
> - Continuously update `./memory.md` (create if missing; append-only).
> - Also append draft-step memory events to:
> - `{{GLOBAL_MEMORY_MD_FILE}}`
> - `{{GLOBAL_MEMORY_JSONL_FILE}}`
> - Log scaffold decisions: assumptions inferred from `task.md`, deterministic baseline choices, and interface
> compliance checks.
> - After writing each required file, append an entry noting what was created and how requirements were
> satisfied.
> # Constraints
> - No user interaction.
> - No prose-only output.
> - Do not modify files outside the required outputs, except temporary read-only source checkouts under `/tmp`.
> 70
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> G. Task Properties and Hyperparameters
> G.1. Aggregate Sweep
> Table 10 | Harness and model configurations for the aggregate sweep. All runs use high reasoning
> effort.
> Harness Version Assigned models
> Codex 0.149.0 GPT-6 Astra; GPT-5.6-Sol; GPT-5.5; GPT-5.4
> Claude Code 2.1.240 Fable 5.1; Opus 5; Opus 4.7; Opus 4.6
> Pi 0.84.2 GPT-5.6-Sol; Opus 5; DeepSeek V4 Flash
> OpenCode 1.18.21 GPT-5.6-Sol; Opus 5; DeepSeek V4 Flash
> G.1.1. Learned Optimization
> Initial Task Instructions for Learned Optimization Topic Direction
> # Goal
> Start from ELO and improve held-out anytime transfer after real outer training from scratch within
> a fixed training-time budget. Candidates may change the learned architecture, outer objective,
> gradient estimator, curriculum, and training policy within `program_0`.
> # Upstream and Data
> Treat upstream repositories as untrusted reference material.
> - ELO is the baseline starting point: https://github.com/xiaol827/ELO at commit
> `664ef6503ae8d2b1d5b89b551680eff91472574a`.
> - Inspect and pin the upstream code, dependencies, license, and data provenance. Use a feasible
> frozen subset of ELO's official task families and its substantive meta-training implementation
> for the starting candidate and unchanged reference method. Offline data/loading adaptations
> are permitted; replacing ELO with a synthetic update-rule proxy is not.
> - Keep final transfer environments, examples, and seeds excluded from all search, meta-training,
> tuning, calibration, and retry decisions. Freeze resource hashes before execution.
> - If the required ELO path or data cannot run, report the concrete drafting blocker.
> # Evaluation
> - Compare candidates with pinned ELO using the same available training data, hardware, and
> end-to-end training-time allowance. Candidates may allocate that time differently: outer-step
> counts, particles, truncation lengths, and total inner work need not match ELO. Record these
> quantities and precision where observable; candidate self-reports do not certify compliance.
> - Use the same evaluation workloads for all methods and one primary transfer score aggregated
> across a small set of predeclared short, medium, and long horizons and independent seeds.
> - Include one hand-designed comparator, such as AdamW, with its budget and settings frozen before
> search. Additional comparators or behavioral controls require a specific scientific purpose.
> - Report stability, training efficiency, runtime, and available memory evidence as diagnostics,
> without adding separate weighted score components or correctness gates.
> - Retain negative results and failures, including divergence, NaNs, malformed updates, and missing
> outer training, under a frozen aggregation policy. Define normal budget exhaustion separately
> from execution failure; no dropping failed seeds or renormalizing around them.
> - Final evaluation trains the frozen candidate afresh, without search checkpoints. Keep its
> environments and outcomes sealed until candidate freeze.
> # Compute Budget
> Use two H100 GPUs total: one evaluator GPU and one isolated candidate GPU. All training, baselines,
> and evaluation must fit this layout and an approximately 20-idea campaign. Define training timing
> boundaries before implementation and measure each method independently with trusted clocks.
> Use harness-supported telemetry; label measurement scope and leave unavailable memory data absent.
> # Task-Drafting Requirements
> Before implementation, freeze a concrete evaluation design: ELO configuration, task/data splits,
> seeds, horizons, time limits, comparator settings, score formula, failure handling, and improvement
> criterion. Choose the smallest feasible design that answers the goal. Do not add requirements
> beyond this brief without a scientific need.
> Build the candidate, search and sealed final evaluators, contract, pinned runtime, and resources
> using the shared harness policies for offline execution, isolation, transport, and publication.
> The evaluator owns data access, budget enforcement, integrity checks, and scoring; research
> components remain candidate-owned and editable. Smoke uses the same interface and substantive
> training path with a reduced workload.
> # Rubrics
> 71
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> - A `learned optimizer` claim requires executed outer training, serialized learned state, and
> evidence that changing or resetting that state changes held-out updates or performance.
> - An improvement claim requires the frozen final comparator to favor the candidate over ELO on the
> primary score, with per-horizon and per-seed results retained. Protocol validity alone does not
> establish improvement, and a negative result remains reportable.
> - Scope conclusions to the tested configuration, tasks, horizons, seeds, and budget.
> # Frozen Manual Experiment
> This directory is the manually maintained implementation of the brief above. Run the existing
> optimization pipeline without its `task_drafting` phase. `protocol.json` freezes the experimental
> design before any search. The canonical drafting brief remains `../learned_optimizer/task.md`.
> The starting method is upstream ELO's `smallMLP`, with the original expert-supervised mechanism
> and a compressed supervision schedule from outer step 10 through 80 with a 1,800-second training
> allowance. Editable architecture, objective, gradient estimator, curriculum, and training policy
> remain inside `program_0`; evaluator code, reference method, resources, and score stay frozen.
> Meta-training uses 8,192 official training images each from MNIST and Fashion-MNIST. Search transfer
> uses 4,096 CIFAR-10 official training images and 1,024 official test images. Final transfer uses
> 4,096 SVHN official training images and 1,024 official test images. Every image is resized to 8×8
> with Pillow 11.1.0 bilinear interpolation and scaled to [0,1]; grayscale and RGB channels remain
> unchanged. Subsets are fixed by sorting original indices by SHA256 of ASCII `seed:index` and taking
> the first requested count. Selection seeds and raw source hashes are frozen before preparation.
> Transfer evaluates a one-hidden-layer width-32 MLP and upstream's `small-conv-10` with channels
> 16 and 32, batch size 64, and horizons 100, 500, and 1,000. Search seeds are 101 and 211; final seeds
> are 1009, 2017, and 3011. Each candidate and reference outer-training run receives 1,800 seconds.
> Train up to two candidate seeds concurrently, with one isolated worker per GPU. After verified
> worker shutdown, transfer uses one GPU for the trusted evaluator and one for the candidate.
> Reuse evaluator-owned search ELO and AdamW evidence only when source, runtime, data, budget,
> and observed GPU/driver identities match. Candidate training is always fresh. Final reference
> evidence is invocation-scoped and never reuses search checkpoints or accesses final data before
> candidate freeze. Total evaluator limits remain 9,000 seconds for search and 12,600 for final,
> including cache construction when required; measured cold and warm runtimes determine planning.
> The primary score is the equal mean of `1 - clip(cross_entropy / log(10), 0, 1)` over all declared
> architecture/horizon/seed cells. Failed cells contribute zero; never drop or renormalize them.
> AdamW settings are learning rate 0.001, betas (0.9,0.999), epsilon 1e-8, weight decay 0.01. A final
> improvement requires the candidate primary score to exceed independently trained frozen ELO.
> Normal trusted-clock training budget exhaustion preserves the latest completed learned state;
> crashes, missing training/state, malformed responses, and nonfinite outputs remain failures.
> The broker cancels overdue training operations before acknowledging timeout. Each seed has two
> declared resets: fresh outer training, then a fresh worker importing the completed checkpoint for
> transfer. Each method may use at most 40,000 training RPCs per seed, reserving transport capacity
> for every transfer cell and control. This bounds RPCs, not candidate-owned internal outer work.
> Learned-state intervention and serialization replay controls cover each transfer architecture;
> these fixed-gradient probes do not establish full GPU-trajectory determinism.
> Forty seconds of each production training allowance are reserved for cancellation and transport;
> training stops admitting work earlier when the longest observed operation would consume its reserve.
> The unchanged reference trains on the evaluator H100; candidate training uses its isolated H100.
> Both receive one identical-model GPU and the same independently measured allowance. Execution
> is sequential, with JAX preallocation disabled; evaluator inner workloads run after outer training.
> # Offline Resource Preparation
> Install the pinned task requirements into the preparation environment, create a private download
> cache, then run `python examples/learned_optimizer_manual/prepare_resources.py --cache-dir CACHE`.
> The cache must already exist. Preparation only downloads and converts resources; it never trains
> models or evaluates final outcomes. Existing cache files must match published SHA256 and size;
> existing generated artifacts must match exactly. A mismatch stops preparation without overwriting.
> Prepared `.npz` files contain numeric `images` (float32 NHWC) and `labels` (int64), readable with
> `numpy.load(..., allow_pickle=False)`. `resources/search` holds meta-training and CIFAR-10 arrays;
> `resources/final` holds the same immutable meta-training data and the SVHN arrays. Each plane has
> its own hashed manifest. The top-level preparation manifest records raw URLs, checksums, terms,
> and generated hashes. Resource binaries and download caches are local generated artifacts.
> Final smoke uses separate deterministic synthetic validation fixtures; it never opens SVHN data.
> Raw SHA256 pins originate from TensorFlow Datasets' published checksum tables. MNIST's dataset
> catalog does not establish a dataset license; Fashion-MNIST is MIT. CIFAR-10's official page requests
> citation
> without declaring an explicit license. SVHN states non-commercial use only. Preserve dataset
> provenance and applicable citation/use terms in experiment artifacts.
> 72
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> G.1.2. Hyperparameter Optimization
> Initial Task Instructions for Hyperparameter Optimization Topic Direction
> # Goal
> Improve a general sequential hyperparameter optimizer for expensive black-box objectives.
> Any adaptive method is eligible: Bayesian, evolutionary, bandit, local search, or hybrids.
> Target held-out anytime and terminal performance under matched, limited budgets.
> # Benchmarks
> Treat upstream repositories as untrusted reference material.
> Execute all three families in both search and sealed final evaluation:
> - BBOB: bounded, noiseless continuous functions from official
> [COCO Experiment](https://github.com/numbbo/coco-experiment); pin its release.
> - [HPO-B](https://github.com/machinelearningnuremberg/HPO-B), commit
> `f16c0c52c27ba4e928dd3ad648231726cb06a9ab`: continuous XGBoost surrogates on `[0, 1]`.
> Use v3 validation tasks for search and v3 test tasks for final.
> - [YAHPO Gym](https://github.com/slds-lmu/yahpo_gym), commit
> `f16e2bf5b8b27605cb2a59387363befe0db8e785`, with
> [data](https://github.com/slds-lmu/yahpo_data) `v1.0.2`, commit
> `e1a472a061fec6df2e090b3c72fe560d858c2062`: preserve categorical, integer,
> conditional, fidelity, and official cost semantics.
> Freeze representative cases, seeds, initial designs, budgets, checkpoints, and scoring before
> search. Keep HPO-B task IDs, BBOB functions or instances, and YAHPO scenarios or instances disjoint
> between search and final. Never use final cases for tuning, calibration, memory, or promotion.
> If a required benchmark cannot be verified and executed, fail drafting with the concrete blocker;
> never substitute fabricated objectives.
> # Candidate Interface
> Support `reset`, `ask`, `tell`, and `metadata`; reset clears prior state.
> Expose only an anonymous typed legal search space, completed observations, and remaining query
> or cost budget. Suggestions must adapt to observations. Anonymous-space dispatch is allowed;
> identity-keyed behavior, fixed replay, lookup answers, hidden optima, and future access are not.
> The evaluator owns accounting. Deterministically reject malformed, duplicate, inactive,
> out-of-bounds, non-finite, or over-budget suggestions. Count invalid outputs, missing cases,
> crashes, and timeouts as declared failures; never drop them from aggregates.
> # Evaluation
> - Match candidates, the frozen adaptive starter, random search, and benchmark-appropriate adaptive
> CPU controls on cases, initial designs, seeds, budgets, CPU limits, and failure accounting.
> Include continuous evolutionary and sequential HPO controls; use fidelity-aware controls for
> multi-fidelity claims. The starter must use observations to change later suggestions.
> - Report per-family normalized anytime and terminal regret, invalid-query rate, and optimizer
> overhead. Convert HPO-B maximization responses to regret; use official BBOB targets/references
> and fidelity- and cost-aware YAHPO scoring.
> - Define `combined_score` as the equal-weight mean of higher-is-better family scores in `[0, 1]`.
> Freeze normalization references independently of candidates, with fixed case, seed, checkpoint,
> and failure weights.
> - Search `correct` requires legal sequential behavior, finite outputs, complete evaluation,
> respected budgets, no leakage, and `candidate_family_mean >= starter_family_mean - 0.01`
> for every family. The reference is the matched starter frozen before search; tolerance is an
> absolute normalized-score difference. Promote only on strictly improving `combined_score`;
> retain the incumbent on ties. Control/checkpoint comparisons are not individual search vetoes.
> - Expose only bounded aggregate development metrics and failure categories during search.
> Freeze candidate code and state before one sealed final evaluation. Report paired held-out
> differences against controls and uncertainty across independent cases and seeds; checkpoints
> within a run are correlated, not independent replications. Predeclare the comparison and
> uncertainty procedure. Search eligibility does not establish scientific superiority.
> # Compute Budget
> CPU-only: zero GPUs, at most 8 cores. Fit sequential optimization into an approximately
> 20-idea campaign. Plane limits include candidates, controls, loading, and scoring:
> | Plane | Minimum cases per family | Minimum seeds per case | Minimum queries per run | Wall time | Memory |
> | --- | --- | --- | --- | --- | --- |
> | Search | 3 | 2 | 32 | 1,800 s | 32 GiB |
> | Final | 3 disjoint | 3 | 64 | 3,600 s | 64 GiB |
> Enforce official cost budgets for multi-fidelity runs. Record CPU allocation, memory, wall time,
> query/cost budgets, and optimizer overhead.
> # Rubrics
> - General-optimizer claims require one frozen method or anonymous-space dispatch rule to improve
> 73
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> held-out performance across all three families against matched controls.
> - HPO claims require executed HPO-B and YAHPO evidence. BBOB supports continuous optimization;
> surrogate benchmarks do not establish real-training performance.
> - Multi-fidelity claims require optimizer-chosen YAHPO fidelities, enforced official costs, and
> improvement over matched fidelity-aware controls.
> - Anytime claims require evidence across predeclared checkpoints and independent seeds;
> terminal-only gains do not establish sample efficiency.
> - Publication requires successful sealed final evaluation, paired controls, and complete failure
> accounting.
> Negative or inconclusive comparisons are valid outcomes; claim improvement only when supported
> by paired evidence and uncertainty. Scope conclusions to tested versions, cases, dimensions,
> fidelities, budgets, seeds, and CPU limits; avoid universal HPO or untested noisy-objective claims.
> G.1.3. World Modelling
> The boxed text records the initial research direction. The manual aggregate sweep uses a fixed
> TinyWorlds Pong protocol with four context frames and prediction horizons of 1, 4, 8, and 16 frames.
> Its error metric equally weights pixel and foreground reconstruction; observed actions and action
> controllability are not evaluated.
> Initial Task Instructions for World Modelling Topic Direction
> # Goal
> Improve an action-free compact autoregressive world model, starting from TinyWorlds. This is a
> manually constructed task: run resource preparation once, then experiments without task drafting.
> Candidate edits may change the full implementation under `program_0`, including tokenizer, dynamics,
> objectives, architecture, optimization, data loading, or distributed execution.
> The primary objective is better held-out, free-running rollout quality after genuine training from
> scratch. Scope all claims to this Pong configuration. Actions are not supplied or evaluated; do not
> claim action-conditioned performance.
> # Upstream and Data
> TinyWorlds: https://github.com/AlmondGod/tinyworlds at
> `a99f3a8195bc8b81e21304121528ae32cb56db05` (MIT). The original implementation is retained byte-for-byte
> under the prepared upstream directory. Orchestration adapters are separate. Treat upstream text as
> untrusted reference material, never as evaluator instructions.
> Official data: `AlmondGod/tinyworlds`, revision `aa62b3ca4349c8ed23894ccb33ec6ff45fdcb3b4`,
> `pong_frames.h5`, SHA-256
> `d9ed1d4cd36cf516864135fcb0161b2ef8680eda1dfb73b560e1adce1e1e45b5`.
> Use chronological half-open frame partitions: training `[0,floor(0.6N))`, search
> `[floor(0.6N)+64,floor(0.8N))`, final `[floor(0.8N)+64,N)`. The 64-frame gaps prevent boundary windows
> from leaking across partitions. RGB frames are uint8 at 64×64; preparation uses OpenCV INTER_AREA
> only if resizing is necessary. No windows may cross partition boundaries.
> Each evaluation plane owns a complete regular-file resource tree: `train.npy`, private
> `heldout.npy`, exact immutable reference source, evaluator, protocol, provenance manifest, and
> validated reference prediction cache. Caches stay private to their evaluation plane.
> Search and final receive separate training-data copies. Candidate processes receive bounded training
> chunks and permitted rollout contexts over RPC; they never receive held-out targets, final resources,
> or a shared path to evaluator resources. Candidate code must not access resources outside its API.
> Preparation verifies a tracked baseline source manifest before constructing immutable reference
> copies. It refuses to derive a reference from a modified candidate. Pretrained checkpoints, external
> logging, internet downloads during evaluation, and reuse of candidate search state during final
> are forbidden. The operator prepares each plane's immutable reference cache from fresh training;
> the evaluator reuses these predictions without exposing reference weights to candidates.
> TinyWorlds' latent-action stage remains present because its dynamics entrypoint requires that
> checkpoint, while dynamics action conditioning is disabled.
> The immutable reference and initial starter use dynamics learning rate `0.001` for search and
> final. Training-only calibration addressed background collapse at the upstream rate of `0.01`
> with batches of 32 per GPU. Their tokenizer and latent-action learning rates retain upstream
> defaults. This adapter configuration leaves the pinned upstream source unchanged.
> # Candidate Interface
> Implement `src/program.py:handle` through the standard `python_rpc_v1` interface. Preserve the
> protocol documented in the starter: seeded reset, bounded training-data chunks, train, behavioral
> checkpoint probes, free-running rollout, and close. Use only the candidate's assigned GPUs and
> scratch directory. All tensors crossing the boundary are bounded, validated serializable payloads.
> The evaluator resets the candidate from scratch once, with search seed 1729 or final seed 2718.
> Reference preparation uses the same respective seeds in separate fresh training runs.
> # Evaluation
> 74
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> Reference preparation and candidate evaluations each train from scratch on all four GPUs with
> identical training data, global batch size, precision, update schedules, and per-role time budgets.
> The immutable reference executes pinned upstream code once per frozen plane configuration; each
> candidate evaluation uses its verified cached predictions. Cache identity binds the source, data,
> contexts, workload, inference implementation, runtime image, and software/hardware configuration.
> Missing or invalid caches fail before candidate training; an operator must prepare them explicitly.
> The starter initially implements the same behavior. Never replace actual TinyWorlds training with
> a synthetic or non-learning proxy. Equal configuration does not guarantee bit-identical training.
> Free-running predictions start from four context frames. Evaluate horizons 1, 4, 8, and 16 using one
> fixed search seed and one disjoint fixed final seed. Future frames and target-derived actions are
> never provided. Pixel reconstruction and foreground reconstruction errors receive equal weight;
> horizons receive equal weight. Foreground is defined relative to a training-only background
> estimate. The evaluator's metric implementation fixes all thresholds and aggregation details.
> Report copy-last-frame and constant-velocity controls. An improvement is eligible only when the
> candidate beats the immutable reference and both controls at every horizon. Execution correctness
> and improvement eligibility are separate: the unchanged starter can execute correctly without
> beating itself. Search `correct` requires successful fresh execution and finite rollouts/checkpoint
> probes; comparative improvement remains the separate `improvement_eligible` metric and score gate.
> Final `correct` additionally requires `improvement_eligible=1`: a win over the immutable reference
> and both controls at every horizon. A completed final evaluation with valid outputs but no such
> improvement records `correct=false` and `not_qualified`; its process still exits successfully.
> Operational baseline preparation accepts this completed, stable non-improvement without claiming
> scientific qualification. Also report temporal error, current candidate training wall time, historical
> reference
> cache-build training wall time, and stability measured
> from finite evaluator-observed behavioral outputs at four predeclared checkpoints. Candidate
> acknowledgements or reported update counts do not prove training stability or learning.
> Final evaluation trains the candidate afresh from sealed resources with seed 2718 and uses its
> separate private final reference cache. It rejects failed candidates, exposed targets, and candidate
> checkpoint reuse. Generated evidence includes bounded
> metrics, control comparisons, finite behavioral probes, provenance, and role-specific work timing.
> Harness GPU telemetry is independent workload evidence; GPU activity alone does not prove learning.
> # Compute Budget
> Allocate four NVIDIA H100 80 GB GPUs to the candidate; the production evaluator uses CPUs and
> receives no GPUs. Reference cache preparation separately uses all four GPUs. Reserve 16 CPU cores
> and 64 GB host memory. Use bfloat16 mixed precision where the pinned implementation supports it.
> Record actual hardware and allocation in validation results.
> Preserve fixed update counts in `workload.json`: search tokenizer/latent/dynamics 20,000/400/24,000;
> final 60,000/1,200/72,000. Stock loops execute each configured count plus one update. Production
> batch size is 16 per GPU, preserving global batch size 64 from the original two-GPU configuration.
> Reference acceptance checks exact update counts and positive measured training intervals for all
> three stages. Productive tokenizer-plus-dynamics time is reported separately from the latent-action
> prerequisite. The earlier two-GPU duration targets are historical calibration observations, not
> minimum durations for four-GPU execution. Never sleep or pad workloads to meet them.
> Search's total ceiling is 10,800 seconds; final's is 32,400 seconds. Each current evaluation ceiling
> includes candidate training, setup, cache validation, rollouts, scoring, and cleanup; reference
> cache-build time is reported separately. Reference preparation includes a reduced native reference
> build on four GPUs with batch size one and four updates per stage; this builder diagnostic is not
> candidate qualification. Evaluator smoke uses the same authenticated candidate broker, device
> discovery and four-GPU topology as production, reducing only workload size. Its CPU-only evaluator
> uses a separately prepared reduced reference cache and never launches GPU training. Smoke never
> reads or writes production reference caches and cannot establish production qualification.
> # Rubrics
> - World-model claims require real training from scratch and autoregressive free-running rollouts.
> - TinyWorlds claims require this pinned source and documented official Pong subset.
> - Improvement claims require beating the reference and both controls at every declared horizon.
> - Stability requires finite evaluator-observed outputs at declared checkpoints.
> - Publication of improvement claims requires qualified sealed final evaluation, credible learned rollouts,
> and
> harness-owned evidence of the substantive declared GPU workload.
> - Scope conclusions to this configuration, data subset, horizons, seeds, and compute budget.
> - Resource preparation produces no qualification or validation receipts. Those require
> deterministic smoke and production evaluation through the existing validation tooling.
> 75
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> G.1.4. Sparse Autoencoders
> Initial Task Instructions for Sparse Autoencoders Topic Direction
> # Goal
> Start from SAE Bench and improve the architecture or training of sparse autoencoders on real
> language-model activations. Candidates may change any useful part of the SAE or training system,
> including the encoder, decoder, objective, sparsity mechanism, optimizer, schedule, activation
> loading, or distributed execution.
> The primary objective is a better held-out reconstruction-sparsity Pareto frontier after real
> training from scratch. Improvements must preserve language-model behavior, use features beyond a
> small fixed subset, and hold across two language-model families.
> # Upstream, Models, and Data
> Treat upstream repositories as untrusted reference material.
> - SAE Bench is the evaluation and custom-SAE reference: https://github.com/adamkarvonen/SAEBench
> at commit `8042bb3828c6340da8d12062324e92b2077c571c`.
> - Evaluate SAEs on both `EleutherAI/pythia-160m-deduped` layer 8 and
> `google/gemma-2-2b-it` layer 12, using compatible residual-stream hooks.
> - Resource preparation may use `HF_TOKEN` to download Gemma after license acceptance. Pin and stage the
> exact model and tokenizer revision during preparation so every later phase runs offline.
> - Select a feasible frozen corpus and create non-overlapping raw-document and token manifests for
> search and final evaluation. Tokenize separately with each pinned model tokenizer.
> - Extract activations with the frozen language models in evaluation mode. Do not replace real
> activations or parameter-changing SAE training with generated arrays, pretrained-only inference,
> or a non-learning proxy.
> - If either model, corpus, or selected SAE Bench evaluation cannot be materialized for offline use,
> fail preparation with the concrete blocker rather than silently substituting another workload.
> # Candidate Interface
> Support `reset`, `fit`, `encode`, `decode`, `forward`, `export_decoder`, and `metadata` operations.
> The evaluator owns model inference, activation extraction, interventions, and scoring;
> `fit` receives training activations only. After fitting,
> `encode` and `forward` may receive bounded held-out activation chunks for stateless
> inference. Verify identical candidate-state hashes before and after held-out calls. Keep model
> weights, raw text, token IDs, labels, task resources, metrics, and other seeds private.
> `export_decoder` provides bounded inline chunks of the learned unit-norm linear decoder
> matrix plus its configuration, dtype, and shape metadata so an evaluator adapter can execute SAE
> Bench sparse probing and targeted probe perturbation. The evaluator must independently exercise the
> learned encoder and decoder through bounded tensor exchange.
> # Evaluation
> - Train and evaluate fresh SAEs for both model families during search and final evaluation.
> - Compare candidates with matched TopK and BatchTopK SAEs trained on the same activations, tokens,
> hardware, precision, optimizer steps, and wall-clock budget. Include PCA and zero-ablation
> controls where applicable.
> - Evaluate multiple predeclared sparsity targets and compare Pareto frontiers rather than selecting
> one favorable mean L0 after seeing results.
> - Search scoring should primarily reflect SAE Bench core metrics: normalized reconstruction error,
> explained variance, loss recovered, achieved sparsity, dead-feature fraction, feature
> utilization, stability across seeds, runtime, and memory.
> - Sealed final evaluation must repeat the core metrics and add SAE Bench sparse probing and targeted
> probe perturbation for both model families.
> - Treat NaNs, divergence, timeouts, malformed encodings, sparsity violations, failed models or
> seeds, and missing parameter-changing training updates as declared failures.
> - Include evaluator-owned constant/copy, reset-state, and applicable permutation controls across
> the declared models, sparsity targets, and seeds.
> - Final evaluation must train every selected SAE afresh and must not reuse search dictionaries,
> checkpoints, feature statistics, probes, or activations.
> # Compute Budget
> Two NVIDIA H100 GPUs are available in total. Use one evaluator GPU and one separate candidate GPU
> for both search and final evaluation. Activation extraction and evaluator-owned baseline fits must
> share the evaluator GPU; candidate fits use the candidate GPU. Schedule models, seeds, and sparsity
> targets within this layout, sequentially when needed. Record actual hardware, allocation,
> precision, tokens, optimizer steps, wall time, peak memory, and checkpoint hashes.
> Search must train each candidate and matched learned baseline on both models, at least two sparsity
> targets, and at least one seed, using at least 1,000,000 training tokens per fit. It must complete
> within 3,600 seconds, 240 GiB host memory, and 80 GiB per GPU. Final repeats both models and targets
> with at least two independent seeds and 2,000,000 training tokens per fit within 10,800 seconds.
> Plane limits include activation extraction, every candidate and baseline fit, interventions, and
> scoring; evaluator and candidate fits may overlap on their separate GPUs when the protocol permits.
> Choose widths, sparsity targets, corpus sizes, context lengths, batch sizes, seeds, metrics, and
> time limits that fit an approximately 20-idea campaign while still exercising substantive SAE
> 76
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> training and evaluation on both models.
> # Evaluation Scope
> AutoInterp, unlearning, RAVEL, feature absorption, and the remaining SAE Bench suites are outside
> the required workload.
> # Rubrics
> - An `SAE` claim requires fresh parameter-changing training, sparse feature activations, a learned
> decoder, and evidence that resetting or permuting the learned state changes held-out outputs.
> - An `SAE Bench` claim requires execution of the pinned benchmark methodology on compatible custom
> SAEs; merely reproducing metric names is insufficient.
> - A Pareto improvement must hold at multiple matched sparsity targets, on both model families, and
> across independent seeds under matched compute.
> - A `loss recovered` claim requires evaluator-owned interventions through each pinned language
> model rather than reconstruction metrics alone.
> - Sparse-probing or targeted-perturbation gains support only claims covered by those evaluations.
> Do not claim human-interpretable, monosemantic, stable, or broadly causal features.
> - Final evaluation must train afresh, report negative and failed models, sparsities, and seeds, and
> compare against all required matched controls.
> - Publication requires successful sealed final evaluation and evidence that the substantive
> declared GPU workload ran.
> - Scope conclusions to the pinned models, hooks, corpus, sparsities, seeds, and selected SAE Bench
> evaluations; do not imply performance on the full benchmark suite.
> # Manual Task Implementation
> No drafting agent is involved. Prepare pinned resources once, then launch optimization.
> This manual realization uses a bounded frozen encoder graph to enforce held-out immutability.
> Candidate Python receives training activations only. After fitting, the evaluator imports plain
> encoder/decoder tensors and executes a validated graph itself. Before held-out scoring, it independently
> checks candidate `encode`, `decode`, and `forward` against that export on training-only inputs.
> The frozen evaluator state is hashed before and after held-out evaluation. Candidate metadata hashes
> are diagnostic only. Arbitrary Python held-out inference is intentionally unsupported.
> ## Frozen interface
> The starter is `program_0/src/candidate.py`, with `handle(request)` as entrypoint.
> The seven original operations remain; `export_encoder` adds graph and tensor chunks.
> Graph operations: matmul, add, sub, mul, relu, gelu, sigmoid, topk. Maximum32nodes,
> 64tensors, 64million encoder parameters, intermediate width16384. Tensor wire format:
> `{dtype: "float16"|"float32", shape: [rows, columns], data: "base64 little-endian bytes"}`;
> maximum64rows,262144elements,2MiB per JSON message. No paths, code, pickle, or credentials.
> Decoder export is a unit-row-norm linear matrix plus bias. Exported weights are FP32.
> Candidate training, objectives, initialization and supported graph architecture remain editable.
> ## Fixed workload and promotion
> Pythia160M layer8 and Gemma2-2B-IT layer12 residual post-block hooks. Default4096features;
> mean-L0 targets32 and128. Context128. Search1,048,576 training tokens per fit and one seed;
> final2,097,152 tokens per fit and two independent seeds. Full core heldout32768tokens.
> Activation transport FP16, model BF16, SAE/optimizer/metrics FP32. Training batch2048,
> 512 search steps /1024 final steps; matched references use Adam3e-4. Fit caps300s search,
> 600s final, always within plane-wide3600s/10800s caps including extraction and controls.
> No hidden extra optimizer passes over training examples. Identical quantized training inputs for all methods.
> BatchTopK calibrates its frozen inference threshold on the final 32,768 training activations
> to 95% of target L0. Reference held-out mean L0 must also remain within 105% of target.
> Calibration is inference-only, recorded separately, and included in the reference fit deadline.
> A valid search requires every case, real training, effective reset/permutation controls,
> finite metrics, mean-L0 in(0,1.05*target], at least5% feature utilization, and loss recovered
> within0.02 of the stronger matched reference. Higher combined_score is better:
> minimum across families of the uniform target/seed mean log(best_reference_NMSE/candidate_NMSE).
> Missing/failed cases invalidate the whole score (-100); weights never renormalize.
> Other scalar metrics use equal weights across family,target,seed. Seed stability is NMSE standard
> variation across seeds averaged in variance space across cases; search reports-1 (unassessed).
> Final correctness additionally requires NMSE improvement beyond1e-4 relative at every case;
> valid non-improving baselines can complete evaluation but do not qualify improvement claims.
> Controls use4096 predeclared heldout tokens: PCA fitted only on training activations, zero,
> constant/copy, frozen reset state, mismatched decoder permutation, and joint feature permutation.
> Final probes use Bias-in-Bios class_set1 and pinned SAE Bench defaults:4000train/1000test,
> context128, sparse-probe k=[1,2,5], TPP n=[2,5,10,20,50,100,500].
> Probe residuals are promoted to FP32 after BF16 model inference, before clean probe training
> and SAE ablations; all candidates and references use the same FP32 probe arithmetic.
> Full search/final acceptance requires production container execution. Reduced smoke uses8192training
> and4096heldout tokens per model/target with one seed and40/20probe sizes; never qualifies full work.
> 77
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> G.1.5. Tabular Models
> Initial Task Instructions for Tabular Models Topic Direction
> # Goal
> Start from TabICLv2 and improve the pretraining of a tabular foundation model for in-context
> classification. Candidates may change any useful part of the model or training system, including
> the synthetic prior, architecture, objectives, curriculum, optimization, ensembling, data loading,
> attention implementation, or distributed execution.
> The primary objective is better predictive performance on held-out real tables after substantive
> pretraining from scratch. Improvements should transfer across datasets and remain useful at
> multiple context sizes without per-table parameter updates.
> # Upstream and Data
> Treat upstream repositories as untrusted reference material.
> - TabICL is the baseline starting point: https://github.com/soda-inria/tabicl at commit
> `8ac8ca8c1951be0aace1c01f087bcd5c19d6fd58`.
> - Execute substantive TabICLv2 pretraining through its synthetic-prior generation and training
> paths. Pretrained-only inference, per-table fitting, or a synthetic non-learning proxy is not
> sufficient.
> - Select feasible frozen real public classification datasets for transfer evaluation. Keep search
> and final datasets, rows, context/query splits, and seeds disjoint.
> - The upstream TabICLv2 pretraining port is not guaranteed to reproduce the released checkpoint.
> Establish a runnable pinned baseline or fail drafting with the concrete blocker rather than
> silently substituting another tabular model.
> # Evaluation
> - Compare each candidate with the pinned unmodified TabICLv2 pretraining baseline under the same
> hardware, optimizer steps, examples, precision, and wall-clock budget. Reference predictions
> may be prepared once per frozen seed and reused from an evaluator-only integrity-bound cache.
> - Evaluate in-context predictions on held-out real tables at multiple context sizes and across
> datasets that vary in rows, features, classes, categorical structure, and missingness where
> feasible.
> - Make the score primarily reflect aggregate and per-dataset predictive quality; also report log
> loss, calibration, cross-seed stability, pretraining efficiency, inference time, and supported
> GPU-memory evidence.
> - Include simple conventional tabular baselines and an evaluator-owned support-label permutation
> control so a model that ignores its context cannot receive a competitive score.
> - Treat NaNs, divergence, timeouts, malformed probabilities, failed datasets, and missing
> parameter-changing pretraining updates as declared failures rather than dropping them.
> - Final evaluation must pretrain the selected candidate afresh and must not reuse search
> checkpoints or expose final tables during optimization.
> # Compute Budget
> Up to four NVIDIA H100 GPUs are available. Task drafting should choose an appropriate one-to-four-
> GPU pretraining and evaluation layout, using distributed or mixed-precision execution when useful.
> Candidates are not required to use all four GPUs. Record the actual hardware, precision, wall time,
> training steps, synthetic tasks processed, and peak memory used by each substantive evaluation.
> Choose the TabICLv2 configuration, synthetic-prior budget, real datasets, context sizes, seeds,
> metrics, and time limits that fit an approximately 20-idea campaign while still exercising real
> pretraining and held-out in-context inference.
> # Candidate Scope
> Keep the synthetic prior, model architecture, objectives, and training policy candidate-owned
> and editable. The evaluator owns frozen real-table splits, comparison budgets,
> integrity checks, and scoring; do not fix the intended research contribution behind the RPC
> boundary. Candidate-controlled priors must still satisfy the declared training budget and cannot
> access hidden real-table queries, labels, dataset identities, or final resources.
> # Rubrics
> - A `pretrained model` claim requires parameter-changing training from scratch, a serialized model
> state, and evidence that resetting or permuting that state changes held-out predictions.
> - A `TabICL` claim requires execution of the pinned TabICLv2 revision and its documented synthetic
> prior and model paths; merely imitating its API or architecture is insufficient.
> - A `foundation model` claim requires one pretrained state shared across held-out tables without
> per-table parameter updates.
> - An `in-context learning` claim requires predictions to depend materially on permitted support
> examples and labels under predeclared counterfactual controls.
> - A transfer improvement must hold across multiple sealed real datasets and context sizes
> under a matched compute budget. This single-seed protocol does not establish seed robustness.
> - Final evaluation must pretrain the candidate afresh, report negative and failed datasets or seeds, and
> compare
> against the required matched baselines.
> - Publication requires successful sealed final evaluation and evidence that the substantive
> 78
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> declared GPU workload ran.
> - Scope conclusions to the pinned TabICLv2 configuration, synthetic prior, datasets, context sizes,
> seeds, and compute budget; do not imply reproduction of the released full-scale checkpoint or
> broad tabular generality.
> # Manual Task Protocol
> This directory is manually authored. Prepare its pinned resources and validate it as described
> in README.md; task drafting is not part of its launch path.
> The starter uses the pinned v2 classifier stage-one recipe: graph-SCM generation, full model,
> 1,024 synthetic rows, global batch 64, and four H100s using data parallelism. The immutable
> reference is prepared separately on four H100s and cached for each search/final seed. Candidate
> and reference receive the same root seeds, 1,765 optimizer steps, synthetic task/example counts,
> precision, and 7,200-second training ceiling. Two-GPU runs are timing controls only. When candidates
> edit the prior, synthetic values may differ; matching means identical budgets and random seed
> schedule, not an immutable prior. The calibrated step count is frozen before optimization.
> Search uses training seed 17 and four frozen real tables, each at support sizes 64 and 256
> with 128 disjoint query rows. Final evaluation uses fresh seed 101 and four different tables.
> One pretrained candidate state serves every table for a seed; no per-table parameter updates are permitted.
> Search and final reference caches are separate frozen resources, never visible to candidate code.
> Missing, stale, corrupt, or incomplete caches stop evaluation; they never trigger hidden retraining.
> The primary score is the equally weighted mean improvement in balanced accuracy over the pinned
> baseline. Logistic regression, histogram gradient boosting, and class-frequency comparisons
> are reported separately. The evaluator also reports accuracy, log loss, 10-bin calibration error,
> durations and training/resource diagnostics. The retained `seed_score_std` field is zero for
> one seed and does not measure robustness across seeds.
> Both learned models must beat their support-label permutation control by more than 0.01 mean
> balanced accuracy per seed. Resetting model state must change predictions (mean absolute
> probability change greater than 1e-6) on every case. Restoring the trained state must restore
> predictions. Invalid outputs or incomplete training invalidate the whole evaluation with score
> -2; all failed cases remain in the evidence inventory. Reduced smoke runs are diagnostics only.
> The RPC exposes reset, train, predict (trained or initial state), and status operations. Candidate
> code owns model/prior/training policy. The evaluator owns real-table preprocessing and query
> labels, split selection, comparisons, scoring, and behavioral controls. Parameter/update counters
> and checkpoint digests returned by candidate code are unverified declarations: behavioral controls
> alone cannot prove that arbitrary editable Python performed pretraining. Scientific pretraining
> and fixed-state claims additionally require source review of the actual selected implementation;
> never present those counters as host-attested compute evidence.
> Scientific acceptance uses evaluator-observed predictive quality, behavioral controls, host timing,
> and the trusted reference's training checks. Candidate-internal counters, state digests, round-trip
> flags, and shape hashes remain bounded diagnostics and never determine scientific correctness.
> Reference preparation verifies the pinned reference source and records raw training receipts
> and all prediction interventions before publishing a cache. Cache identity includes reference
> source, runtime image, dependencies, workload, data splits, and control implementation. Candidate
> edits do not alter that identity. Cached reference preparation time is reported separately from
> current evaluation time. Training children use a fixed Python hash seed so
> the unmodified upstream graph prior's set iteration remains reproducible across processes.
> 79
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> G.2. Details of Doc-to-LoRA Inductive Bias
> Table 11 | Doc-to-LoRA multi-needle NIAH agent-search and evaluation protocol.
> Stage Setting Value
> Agent search Idea budget 40 ideation cycles
> Model / harness gpt-5.6-sol / Codex; claude-opus-5 / Claude;
> deepseek-v4-pro / Pi
> Hardware One H100 GPU with 16 CPU cores
> Training time limit 3 hours; candidates above 3 hours are invalid
> Dataset Training datasets NIAH samples range from 32 to 256 tokens
> Evaluation datasets NIAH samples range from 32 tokens to 64K tokens
> Example contract Each context contains 1-4 needles
> Benchmark scale 512K training samples; 128 samples per length bucket in
> {128, 8K, 16K, 32K, 64K}, totaling 640 samples.
> Baseline model / training Base model Fixed google/gemma-2-2b-it target model and
> tokenizer
> Adapter setup down_proj target module and LoRA rank 4
> Initial Task Instructions for Doc-to-LoRA Inductive Bias Topic Direction
> # Goal
> Improve the Doc-to-LoRA / Context-to-LoRA hypernetwork on the synthetic
> multi-needle NIAH task while keeping training under the strict 3-hour limit on a
> single `aisci` GPU. The 3-hour limit is final: runs that require more time are
> discarded, not rescheduled or judged separately.
> The fixed candidate entrypoint is `program_ /src/program.py`. It is
> read-only benchmark code. It already:
> 1. links the frozen source `ctx_multi_needle_*` jsonl cache from
> `TASK_SOURCE_DIR` when available, then generates missing data only as a
> standalone fallback,
> 2. runs `scripts/niah/1-train.sh`,
> 3. evaluates the resulting checkpoint with `scripts/niah/2-eval-test.sh`, and
> 4. writes `program_ /src/_results.json`.
> `evaluate.py` reads `_results.json`, enforces the strict three-hour training limit,
> and scores candidates mainly by exact-match accuracy on queried target needles.
> Each context independently samples 1--4 named needles, but the prompt asks for
> only one target needle so the label remains a single 4-digit number. Candidate
> changes should improve the model/training implementation used by that fixed
> entrypoint.
> # Dataset contract
> - Dataset names use the `ctx_multi_needle_ _ ` family. They are
> generated by `program_0/src/data/generate_ctx_multi_needle.py`.
> - Each example independently samples `num_needles` uniformly from 1 through 4,
> inclusive. Do not assume every context has the same number of needles.
> - Each needle is a named fact such as
> `The needle named amber stores magic number 1234.` Names and 4-digit numbers
> are unique within a context.
> - The prompt asks for one target needle by name, for example
> `What magic number is stored by the needle named amber? Reply with only the number.`
> The response is only the 4-digit number for that target needle.
> - Target needle depth is balanced across ten context-depth bins over each full
> train/validation/test split, including long token bins with many possible
> context lengths. Distractor needles are placed at random non-target positions,
> so candidates should learn key-value binding rather than copying the first or
> last number.
> - This benchmark is not an all-needles recall task. Keep the single-number
> answer contract unless intentionally changing the evaluator and scoring.
> # Required background
> Read the local paper draft first: `doc_to_lora_paper_icml2026/main.tex` and the
> figures/tables it references. It explains the intended Doc-to-LoRA mechanism,
> why context-conditioned LoRAs are useful, and which ablations already exist.
> 80
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> Use it to avoid rediscovering baseline ideas or optimizing the wrong bottleneck.
> # Required research directions
> Focus edits on real model changes, not evaluator bypasses.
> 1. **How to merge per-chunk LoRAs.** The current implementation in
> `src/ctx_to_lora/modeling/lora_merger.py` directly concatenates the generated
> A and B matrices from different context chunks. Consider alternatives such as
> normalization, learned or heuristic chunk weighting, low-rank recompression,
> rank budgeting, gating, or merge rules that reduce destructive interference.
> 2. **Inductive bias in the hypernetwork output space.** Consider where LoRAs are
> applied and what the hypernetwork emits. Examples: which target modules are
> useful (`down_proj`, attention projections, MLP projections), whether all
> layers are necessary, whether rank should vary by layer/module, whether a
> shared basis plus generated coefficients is enough, or whether cheaper output
> spaces such as diagonal/residual scales, layernorm shifts, KV-focused adapters,
> or generated biases give better accuracy per compute.
> 3. **Inductive bias in the hypernetwork architecture.** The current cross-attention
> design assumes each latent query corresponds to one generated LoRA rank.
> Consider whether that rank-query binding is necessary. Examples: shared latent
> factors with learned rank mixing, hierarchical queries for layer/module groups,
> separate queries for A and B factors, rank tokens conditioned on context
> length, or architectures that generate a compact basis plus coefficients
> before expanding to LoRA parameters.
> # Runtime rules
> - Training must complete in less than 3 hours on one `aisci` GPU. This limit is
> strict and final. Any run with `train_runtime_seconds > 10800` is invalid and
> will be discarded, regardless of accuracy or partial progress.
> - The fixed entrypoint must continue to run the two task scripts named above.
> Do not replace them with fake metrics, cached answers, or a no-op.
> - The fixed entrypoint records a `full_epoch_probe` in `_results.json` after
> training. Leave this probe intact; it reports whether full 1-epoch training is
> feasible for the candidate under the strict three-hour limit.
> - Default evaluation is intentionally small for iteration, but its environment
> values are fixed by the evaluator. `evaluate.py` strips ambient
> `DOC_TO_LORA_*` variables and injects the benchmark values itself, so every
> candidate sees the same train/eval scale:
> - `DOC_TO_LORA_BASE_SAMPLES_PER_BIN=256000`
> - `DOC_TO_LORA_MAX_TRAIN_SAMPLES_PER_DS=256000`
> - `DOC_TO_LORA_MAX_TEST_SAMPLES_PER_DS=128`
> - `DOC_TO_LORA_EVAL_BATCH_SIZE_GEN=8`
> - `DOC_TO_LORA_MAX_NEW_TOKENS=8`
> - `DOC_TO_LORA_EVAL_DATASETS="ctx_multi_needle_32_128 ctx_multi_needle_7168_8192
> ctx_multi_needle_15360_16384 ctx_multi_needle_28672_32768 ctx_multi_needle_57344_65536"`
> - Do not tune scores by changing `DOC_TO_LORA_*` environment-variable usages,
> `evaluate.py`, `program.py`, the data generator, `run_eval.py`, evaluator helpers,
> dataset definitions, or the NIAH shell-script defaults. `program_spec.toml`
> declares these as guarded checks, and the harness rejects candidates that edit
> or bypass them.
> # Fixed benchmark boundaries
> Candidates should improve model and training internals, not evaluator or runtime
> configuration escape hatches.
> - Do not edit `evaluate.py`.
> - Do not edit `program_ /src/program.py` or change the entrypoint away from
> it.
> - Always use the fixed base model `google/gemma-2-2b-it`; do not change
> `model_name_or_path` or add alternate target-model selection.
> - Do not add, remove, or modify `DOC_TO_LORA_*` environment-variable usages.
> - Do not change the NIAH shell scripts in `scripts/niah/`; they are guarded so
> `program.py` continues to run the same train/eval contract.
> # Metrics
> The fixed entrypoint writes `_results.json` with at least:
> - `training_completed: true`
> - `evaluation_completed: true`
> - `final_accuracy` in `[0, 1]`
> - `exact_match_mean` when generated text files are available
> - `train_runtime_seconds`
> - `eval_runtime_seconds`
> - `checkpoint_path`
> - `full_epoch_probe`
> `combined_score` is exactly `final_accuracy` for valid runs. Invalid runs receive
> a large negative score. `_results.json` and evaluator `metrics.json` also include
> 81
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> non-scoring diagnostics grouped by needle count, target depth bin, target needle
> ordinal, and first/last-target flags.
> # Rubrics
> - `niah_accuracy`: maximize exact-match accuracy on the generated multi-needle NIAH test set.
> - `train_time`: training must stay below 3 hours on one GPU. This is a hard
> cutoff; over-limit runs are discarded.
> - `script_contract`: `program.py` must run `scripts/niah/1-train.sh` and
> `scripts/niah/2-eval-test.sh` and write `_results.json`.
> - `feasibility_probe`: preserve the fixed entrypoint probe that reports whether
> full 1-epoch training is feasible for the candidate under the time limit.
> - `research_focus`: changes should address LoRA merging or hypernetwork output
> inductive bias rather than evaluator shortcuts.
> 82
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> G.3. Details of Learned Covariance Matrix Adaptation
> Table 12 | MetaEvo training, search-validation, and transfer protocol. BBOB tasks meta-train neural
> weights; Ant subsequently supplies the score used by the agentic program search.
> Stage Setting Value
> Agent search Idea iterations 30 per agent
> Models / agents gpt-5.6-sol / Codex; claude-opus-5 /
> Claude;
> deepseek-v4-flash / Pi; kimi-k3 / OpenCode
> Valid evaluations 28 Codex; 30 Claude;
> 28 DeepSeek + Pi; 30 Kimi + OpenCode
> Outer meta-evolution Optimizer Full CMA-ES
> Population / generations 256 / 1,500
> Selected candidates’ initial std. 0.10 (Codex, Claude, Kimi); 0.01 (DeepSeek + Pi)
> BBOB inner loop Tasks per outer generation 128 newly sampled, shared across candidates
> Population / generations 16 / 50
> Dimension / initial std. Uniform 2–10 / 1.0
> Functions Sphere, Rosenbrock, Discus, Rastrigin, Schwefel,
> Bueche–Rastrigin, Attractive Sector, Weierstrass,
> Schaffers F7, Griewank–Rosenbrock
> Noise Additive, standard deviation uniform in [0, 0.1]
> Aggregate loss Per-task standardized loss; median across tasks
> Ant search-validation Population / generations 256 / 2,000
> Seeds / episode length 8 fixed seeds / 500 steps
> Initial standard deviation 0.05
> Score AUC normalized to initial policy and CMA-ES
> Brax transfer Environments / seeds 8 / 16 new shared seeds each
> Population / generations 256 / 2,000
> Initial std. / episode length 0.05 / 500 steps
> 83
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> Initial Task Instructions for Learned Covariance Matrix Adaptation Topic Direction
> # Goal
> Improve the mutable program under `program_N/src/`. The fixed evaluator meta-trains its
> learned parameters only on cheap analytic BBOB functions. It then freezes those parameters,
> rebuilds the same optimizer for a Brax Ant policy, and uses the Ant result as the scientific
> score. Ant never updates neural weights or outer-CMA state.
> This follows the meta-black-box loops used for
> [Learned Evolution Strategies](https://arxiv.org/abs/2211.11260) and
> [Learned Genetic Algorithms](https://arxiv.org/abs/2304.03995), while testing whether a cheap
> analytic training distribution can discover a LearnedCMA that transfers to control. Promising
> architectures include permutation-equivariant or self-attention modules that jointly update the
> mean and a diagonal, low-rank, factorized, or full covariance representation. Choose the design
> from experimental evidence.
> # Rubrics
> - Maximize the primary CMA-anchored Ant-2000 AUC score; initial policy = 0, CMA-ES = 1.
> - Implement a valid evosax-style learned distribution strategy with 1–1,024 meta-parameters.
> - Meta-train only on the fixed BBOB distribution, then transfer frozen learned weights to Ant.
> - Preserve dimension/population generality and horizon-free, evidence-conditioned updates.
> - Keep the fixed evaluator, seeds, budgets, scoring, and runtime contracts unchanged.
> # Scientific contract
> ## Mutable contract
> `program.py` must export:
> ```python
> build_candidate(solution, population_size) -> CandidateDefinition
> ```
> `program_0` is a self-contained port of the evosax LearnedES architecture and lifecycle. Its
> 142 learned weights are freshly initialized and become the initial mean of outer CMA-ES; no
> pretrained checkpoint is loaded. You may rewrite its state, features, architecture, sampling,
> mean/covariance updates, and the outer-CMA scalar settings exposed by
> `CandidateDefinition.outer_settings`.
> The learned parameter PyTree must:
> - match `strategy.default_params.params` in structure, shape, and dtype;
> - contain 1–1,024 finite floating-point scalars;
> - work with evosax-style `init`, `ask`, `tell`, and `get_mean` signatures;
> - have the same structure, shapes, dtype, initialization, name, strategy type, and outer settings
> when built for 10-D/population-16 BBOB and 704-D/population-256 Ant;
> - express domain- and population-size generalization through generic normalized formulas, never
> by branching on the known BBOB/Ant dimensions, population sizes, task identity, or environment.
> Every learned distribution decision–-including mean, scale, covariance, step-size, learning-rate,
> and gating updates–-must define an **anytime optimizer**. It must not condition optimizer decisions
> on absolute generation, elapsed iteration, the fixed evaluation budget or horizon, a countdown,
> or a timestamp embedding. It may condition on horizon-free optimization evidence such as ranks,
> population statistics, evolution paths, distribution scale/shape, improvement, success, and
> stagnation. A generation counter may exist only for lifecycle bookkeeping and metrics.
> ## Fixed BBOB meta-evolution
> - Outer optimizer: full CMA-ES, population 256, 1,500 generations; final distribution mean selected.
> - Inner optimizer: candidate strategy, population 16, 50 generations, initial standard deviation `1.0`.
> - Per outer generation: 128 newly sampled tasks, shared across all 256 candidates as common random
> numbers and vectorized over 8 H100 GPUs.
> - Training functions: Sphere, Rosenbrock, Discus, Rastrigin, Schwefel, Bueche-Rastrigin,
> Attractive Sector, Weierstrass, Schaffers F7, and Griewank-Rosenbrock.
> - Task distribution: dimensions sampled inclusively from 2–10, shifted optima/objective offsets,
> BBOB rotations, and additive Gaussian noise with standard deviation sampled uniformly from
> `[0, 0.1]`.
> - Inner diagnostic curve: clean objective regret to the known optimum, transformed with
> `log10(max(regret, 1e-8))`.
> - Task loss: best observed objective over all 50 generations and 16 members. Each task is
> standardized across the outer population, then candidate loss is the median across tasks.
> Invalid task runs receive a fixed worst loss.
> - Outer settings candidates may alter: `std_init`, `std_min`, `std_max`, `c_mean`, `c_std`,
> `d_std`, `c_c`, `c_1`, `c_mu`, `mu_eff`, `chi_n`, `elite_ratio`, and
> `use_negative_weights`.
> The evaluator fixes all task sampling, budgets, hardware, seeds, and scoring. Candidate code may
> not access Ant, evaluator reference data, result directories, environment variables, the network,
> subprocesses, or files outside its own source bundle. This is a cooperative research-code
> contract, not an adversarial sandbox.
> ## Frozen Ant validation and score
> 84
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> After BBOB meta-evolution, the selected learned-weight PyTree is serialized, authenticated, and
> loaded unchanged into the candidate rebuilt for Ant. No further meta-optimization occurs.
> - Environment: historical Brax v1 `ant`, legacy-spring dynamics with contact-force observations.
> - Policy: observation-normalized tanh-linear policy, 87 inputs × 8 actions plus bias
> (704 parameters), zero initial search mean.
> - Candidate and reference population: 256; initial policy standard deviation `0.05`.
> - Evaluation: 8 fixed seeds excluded from BBOB training, one seed per H100, 2,000 generations,
> 500-step episodes, one rollout.
> - Primary score: full 2,000-generation best-so-far AUC normalized to the pinned same-seed CMA-ES
> reference:
> ```text
> (candidate_AUC_2000 - initial_policy_AUC) / (CMA_ES_AUC_2000 - initial_policy_AUC)
> ```
> Initial policy = 0, CMA-ES = 1, and scores above 1 beat CMA-ES. The evaluator also reports the
> first-1,000-generation AUC and best return, generation-2,000 best return, every seed curve, and a
> paired bootstrap interval. Those are diagnostics; only the 2,000-generation normalized score
> ranks ideas.
> Non-finite returns, absolute returns above 10,000, and active-step torso x velocities above
> 50 m/s are invalid. Observation normalization uses masked population-wide Welford statistics;
> invalid trajectories and post-termination AutoReset samples do not update those statistics.
> ## Operational notes
> - Exactly one harness evaluation is allowed per idea. Inspect `metrics.json`, `correct.json`, and
> captured logs before proposing the next architecture.
> - BBOB and Ant execute in separate Python processes. BBOB uses normal GPU command buffers; only
> legacy-spring Ant applies the H100 command-buffer workaround.
> - The evaluator watchdog is 6h05m inside a six-hour scientific budget plus queue cleanup margin.
> - Outer checkpoints bind config, program, evaluator, dependency, and reference identities and the
> serialized state digest. Interrupted runs may resume only through the supplied campaign tools.
> - Optimize expected transfer and evidence quality, not evaluator plumbing or benchmark identity.
> 85
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> G.4. Details of Error-Diffusion experiments
> Table 13 | Error-Diffusion task and evaluation parameters.
> Setting MNIST CIFAR-10 MuJoCo / Brax Craftax
> Model / Harness Sonnet 4.6 / Opus 4.6 / Claude Composer 2.5 Fast GPT-5.5 / Codex
> Claude Code Code / Cursor
> Agent search budget 20 ideation cycles 60 ideation cycles 60 ideation cycles 30 ideation cycles
> Evaluator resources 1 GPU, 4 CPUs 1 GPU, 8 CPUs 1 GPU, 4 CPUs 1 GPU, 4 CPUs
> Dataset / environment MNIST: 55k train / CIFAR-10: 45k Ant, HalfCheetah, Craftax-Symbolic-
> 5k validation / 10k train / 5k valida- Humanoid v1
> test tion / 10k test
> Final training budget 250 epochs 250 epochs 400M requested 1B requested steps
> steps per seed per seed
> .
> Initial Task Instructions for Error-Diffusion Classification Topic Direction
> ## Goal
> - Extend the binary classification Error Diffusion program to a multi-class classification problem. Achieve
> at least 95% `val_accuracy` on the fixed MNIST validation split, and aim for more than 98% `val_accuracy` if
> possible.
> - Do not use the official MNIST test split during search-time evaluation and do not write `test_accuracy`
> into `_results.json`.
> - Tip: Check gradient norm / activation norm at each epoch during training.
> ## Rubrics
> - Do not use backprop or autograd for the backward pass.
> - Do not use W.T (forward weights transposed) in the backward pass, because this is a sign of backprop.
> - Make sure to use the MNIST data loader provided in `example_mnist_mlp.py`, using its fixed `55k train / 5k
> val / 10k test` split with `split_seed=0` and `500` validation examples per class.
> - The training script must write `_results.json` with at least `val_accuracy`, `training_time`,
> `terminated_early`, `dataset_name`, `split_seed`, `train_examples`, `val_examples`, and
> `val_examples_per_class`.
> - `dataset_name` must be exactly `"MNIST"`. Synthetic or toy datasets must not pass evaluation as MNIST.
> - Downloaded datasets and caches must not be stored inside any `program_*` directory. Use a shared cache
> path such as `AGENTIC_DATA_DIR` when available instead of `./data` relative to the program workspace.
> - Keep the core weight updating mechanism as delta w \propto -(learning rate) x (presynaptic activity) x
> (postsynaptic activation derivative) x (one global error sign)
> - Polarity invariance must be preserved through multiple layers. For this, the following must hold:
> - all trainable weight tensors should remain non-negative (this constraint may be relaxed if needed for
> performance). Any violations of the non-negative constraint should be logged (e.g., the number of violated
> weights).
> - the derivative of the activation function must be non-negative.
> - the forward pass must preserve the excitatory/inhibitory sign structure such as:
> - `p_i = f(+p_{i-1} @ W_pp - n_{i-1} @ W_np + b_p)`
> - `n_i = f(+n_{i-1} @ W_nn - p_{i-1} @ W_pn + b_n)`
> - Do not use fixed random matrices or trainable backward weights to route output errors to hidden layers,
> because that would make the method equivalent to Direct Feedback Alignment / Feedback Alignment.
> - Do not use any data augmentation.
> - Do not rely on random feature extractors.
> - Do not treat multi-class classification as a simple ensemble of binary ED models.
> - Focus on fully connected layer variants; do not explore conv nets or attention.
> - Achieve at least 95% validation accuracy on the fixed MNIST validation split.
> Initial Task Instructions for Error-Diffusion Convolution Topic Direction
> # Goal
> - Extend the best multi-class Error Diffusion program from `ed_mnist` (provided as
> `program_0/src/program.py`) to a convolutional neural network. Achieve at least 60% `val_accuracy`
> (equivalent to `combined_score`) on the fixed CIFAR-10 validation split, and aim for more than 85%
> `val_accuracy` if possible.
> - Do not use the official CIFAR-10 test split and do not write `test_accuracy` into `_results.json`.
> - Tip: Check gradient norm / activation norm at each epoch during training.
> # Rubrics
> - Do not use backprop or autograd for the backward pass.
> - Do not use W.T (forward weights transposed) in the backward pass, because
> this is a sign of backprop.
> 86
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> - Make sure to use the same network structure as in `example_cifar10_convnet.py`, but implement it using
> Error Diffusion's dual-stream architecture. The model must use convolutional layers. Do not collapse the
> task back into a flat MLP on handcrafted features.
> - Make sure to use the CIFAR data loader provided in `example_cifar10_convnet.py`, using its fixed `45k
> train / 5k val / 10k test` split with `split_seed=0` and `500` validation examples per class.
> - The training script must write `_results.json` with at least `val_accuracy`, `training_time`,
> `terminated_early`, `dataset_name`, `num_conv_layers`, `split_seed`, `train_examples`, `val_examples`, and
> `val_examples_per_class`.
> - Print per-epoch training progress including the epoch number and validation accuracy in a parseable format
> such as `epoch 012 ... val_acc=0.2345` or `epoch 012 ... val=0.2345`. The evaluator may stop runs that are
> learning too slowly based on these logs.
> - Optimize against validation accuracy.
> - `dataset_name` must be exactly `"CIFAR10"`.
> - Downloaded datasets and caches must not be stored inside any `program_*` directory. Use a shared cache
> path such as `AGENTIC_DATA_DIR` when available instead of `./data` relative to the program workspace.
> - Polarity invariance must be preserved through multiple layers. For this, the
> following must hold:
> - all trainable weight tensors should remain non-negative (this can be relaxed if needed to achieve the
> desired performance).
> - the derivative of the activation function must be non-negative.
> - the forward pass must preserve the excitatory/inhibitory sign structure,
> including convolutional layers, for example:
> - `p_i = f(conv_op(p_{i-1}, W_pp) - conv_op(n_{i-1}, W_np) + b_p)`
> - `n_i = f(conv_op(n_{i-1}, W_nn) - conv_op(p_{i-1}, W_pn) + b_n)`
> - Do not use fixed random matrices or trainable backward weights to route output errors to hidden layers,
> because that would make the method equivalent to Direct Feedback Alignment / Feedback Alignment.
> - Achieve at least 60% validation accuracy on the fixed CIFAR10 validation split.
> 87
> The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
> G.5. Details of RL-based LLM fine-tuning
> Table 14 | GRPO agent-search and evaluation protocol. A fixed Qwen2.5-Math-1.5B model is trained
> on the MATH dataset with GRPO; final validation accuracy (mean reward) supplies the score used by
> the agentic parallel investigation phase, and the best program of each run is re-scored on a held-out
> test set.
> Stage Setting Value
> Agent search Idea budget 50 ideation cycles
> Model / harness claude-opus-4-7 / Claude
> Program evaluations 1 per idea
> Pruning Every 20 cycles
> Hardware Two H100 GPUs with 16 CPU cores
> Training time limit 2.5 hours
> Execution Containerized (Docker, CUDA 12.8), Slurm-submitted
> Dataset Base model / tokenizer Fixed Qwen/Qwen2.5-Math-1.5B
> Task MATH reasoning
> Train / validation split 6,000 / 1,500 examples (stratified by subject and level)
> Test split MATH-500 problems (secret, outside the task mount)
> Initial Task Instructions for RL-based LLM fine-tuning Topic Direction
> # Goal
> - Improve Group Relative Policy Optimization (GRPO) for training a language model on math reasoning tasks.
> - The base model is Qwen2.5-Math-1.5B, trained on the MATH dataset using GRPO.
> - Your goal is to maximize the final validation accuracy (mean reward) of the model.
> - Evaluate candidate programs via `evaluate.py` and maximize `combined_score` from `metrics.json`.
> - Respect correctness: `correct.json` must report `correct: true`.
> - Focus on implementing ONE specific variant of your idea with ONE fixed set of hyperparameters. Do NOT
> perform hyperparameter tuning, grid searches, ablation studies, multiple variants, comparative experiments,
> or additional baseline comparisons.
> - IMPORTANT: Each evaluation runs a full GRPO training loop (20 epochs) and takes approximately 1–2 hours.
> When calling `evaluate.py`, you MUST set a bash timeout of at least 7200000 ms (2 hours) to avoid the process
> being killed prematurely.
> # Rubrics
> - `accuracy_target`: final validation accuracy should improve over the baseline.
> - `training_stability`: training should complete without NaN losses, OOM errors, or crashes.
> - `code_safety`: program must remain executable with the provided evaluator and `grpo_utils` library.
> - `evidence`: every evaluated candidate must produce `metrics.json`, `correct.json`, and rubric evidence.
> 88

Q1: 这篇论文试图解决什么问题？

rosses mark evaluations whose program modified the evaluation protocol (identified by manual audit); they are excluded from all running-best curves.

Q2: 有哪些相关研究？

当前自动解析没有稳定提取出 Related Work 的完整脉络。可先从论文引言、相关工作章节和引用线索核对它主要对比了哪些方法。

从当前证据可见，论文的定位至少包括：5), MLR-Bench targets open-ended machinelearning research (Chen et al., 2025), and EXP-Bench isolates whether agents can conduct valid AI experiments (Kon et al., 2026).；(2024b) introduced an LLM-driven pipeline for paper reviewing, which was later validated on research papers published after the training data cutoff date (Lu et al., 2026).；ntrusted reference material.。

Q3: 论文如何解决这个问题？

5), MLR-Bench targets open-ended machinelearning research (Chen et al., 2025), and EXP-Bench isolates whether agents can conduct valid AI experiments (Kon et al., 2026). (2024b) introduced an LLM-driven pipeline for paper reviewing, which was later validated on research papers published after the training data cutoff date (Lu et al., 2026).

Q4: 论文做了哪些实验？

ntrusted reference material. - SAE Bench is the evaluation and custom-SAE reference: https://github.com/adamkarvonen/SAEBench at commit `8042bb3828c6340da8d12062324e92b2077c571c`.

Q5: 发现了什么实验现象？

ntrusted reference material. - SAE Bench is the evaluation and custom-SAE reference: https://github.com/adamkarvonen/SAEBench at commit `8042bb3828c6340da8d12062324e92b2077c571c`.

Q6: 有什么可以进一步探索的点？

- Limitations.
- res a provenance map that links ideas, hypotheses, and claims to prior work, extending the current claim-to-source map beyond experimental evidence.
- Across multiple research case studies, The Agentic Scientist demonstrates that coding agents can contribute to scientific research when embedded in an orchestrated workflow that…
- 优先核对语义命中的全文片段：Abstract。
- 先看 Introduction，确认论文到底在补哪一块空白。
- 最后看 Conclusion / Discussion，判断作者自己如何定义边界与下一步。

Q7: 总结一下论文的主要内容

We study a learned evolution strategy whose learned outputs are zero-initialized bounded multiplicative residuals on a separable CMA-ES core, so the strategy is exactly the…

rosses mark evaluations whose program modified the evaluation protocol (identified by manual audit); they are excluded from all running-best curves.

5), MLR-Bench targets open-ended machinelearning research (Chen et al., 2025), and EXP-Bench isolates whether agents can conduct valid AI experiments (Kon et al., 2026). (2024b) introduced an LLM-driven pipeline for paper reviewing, which was later validated on research papers published after the training data cutoff date (Lu et al., 2026).

ntrusted reference material. - SAE Bench is the evaluation and custom-SAE reference: https://github.com/adamkarvonen/SAEBench at commit `8042bb3828c6340da8d12062324e92b2077c571c`.

主要贡献包括：5), MLR-Bench targets open-ended machinelearning research (Chen et al., 2025), and EXP-Bench isolates whether agents can conduct valid AI experiments (Kon et al., 2026).；(2024b) introduced an LLM-driven pipeline for paper reviewing, which was later validated on research papers published after the training data cutoff date (Lu et al., 2026).；ntrusted reference material.。

需要注意的边界包括：Limitations.；res a provenance map that links ideas, hypotheses, and claims to prior work, extending the current claim-to-source map beyond experimental evidence.；Across multiple research case studies, The Agentic Scientist demonstrates that coding agents can contribute to scientific research when embedded in an orchestrated workflow that…。

与用户画像的关系：从全文语义检索命中的片段看，相关信息主要落在 Abstract 部分。；这篇论文和你当前画像里的方向有直接重合：智能体（权重 0.10）, AI for Science（权重 0.10）, 生成（权重 0.10）。；如果你更看重系统性工作，可以重点看它如何组织任务设定、实验协议和整体框架。。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在 Abstract 部分。

## 基本信息

- 作者：Robert Tjarko Lange, Luca Grillotti, Yutaro Yamada, Rujikorn Charakorn, David Ha
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.LG
- 日期：2026-10-13
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：heuristic / PaperFlow template
- arXiv ID：`2610.18933v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 生成式精读补充本次未返回，当前内容仍按精读模板基于已拿到的摘要、元数据和可用 PDF 片段生成。
