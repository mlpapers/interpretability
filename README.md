# Interpretability and Explainable AI

## Explanatory targets

- **Predictor and behavior:** inspect the scoring rule, explain one output, or measure predictive performance. [Removal-based explanations](https://jmlr.org/papers/v22/20-1316.html) distinguish output contributions from loss-based importance; these targets need different comparisons.
- **Training data:** influence methods estimate how training observations affect predictions or loss. [Koh and Liang](https://proceedings.mlr.press/v70/koh17a.html) approximate parameter responses; deletion-and-retraining is a separate validation target.
- **Representation and computation:** [probes](https://arxiv.org/abs/1909.03368) test decodable information; [causal abstractions](https://arxiv.org/abs/2106.02997) test proposed computations through aligned interventions. Accessible information alone does not establish its use.
- **Decision changes:** [recourse](https://arxiv.org/abs/2002.06278) concerns feasible actions and their downstream effects. Intervening on model activations tests the program; intervening on people or physical systems requires assumptions about the world.

## Interpretable models

- **Sparse scoring systems:** learn small integer coefficients with sparsity and application-specific constraints. [RiskSLIM — Ustun and Rudin](https://jmlr.org/papers/v20/18-615.html) (2019). The score is the predictor itself.
- **Rule lists:** ordered conditions followed by a default prediction. [CORELS](https://jmlr.org/papers/v18/17-716.html) (2017/2018) certifies optimal regularized training risk over its supplied rule candidates when optimization completes.
- **Additive models and interactions:** GAMs sum feature functions on a link/score scale; [GA²M](https://www.microsoft.com/en-us/research/publication/accurate-intelligible-models-pairwise-interactions/) (2013) adds pairwise terms. [EBMs](https://interpret.ml/docs/ebm.html) learn them by boosting. Additivity on that scale need not imply additive probability changes.
- **Neural additive models:** jointly train one neural network per input and sum their outputs on the score/link scale. [Agarwal et al.](https://arxiv.org/abs/2004.13912) (2020/2021). The original architecture exposes feature functions without explicit cross-feature interaction terms.
- **RETAIN:** uses reverse-time, two-level attention and computes visit/variable contributions with attention held fixed. [Choi et al.](https://arxiv.org/abs/1608.05745) (2016); [code](https://github.com/mp2893/retain). Interpretation relies on this architecture's contribution calculation.

## Feature effects and global importance

- **Removal-based explanations:** [Covert et al.](https://jmlr.org/papers/v22/20-1316.html) (JMLR 2021) connect SHAP, LIME, permutation tests and other methods through three choices: how features are removed, which behavior is measured, and how removal effects are summarized.
- **Partial dependence (PDP):** averages model predictions while varying selected inputs and retaining other observed values. Correlated inputs can produce unrealistic combinations. [Definition and implementation](https://scikit-learn.org/stable/modules/partial_dependence.html).
- **Individual conditional expectation (ICE):** plots a separate prediction curve for each observation, exposing heterogeneity hidden by average partial dependence. [Goldstein et al.](https://arxiv.org/abs/1309.6392) (2013/2015).
- **Accumulated local effects (ALE):** accumulates locally averaged prediction differences, reducing extrapolation caused by dependent inputs. [Apley and Zhu](https://arxiv.org/abs/1612.08468) (2016/2020). Feature effects describe the fitted model, not causal effects.
- **Permutation importance:** measures a fitted model's score change after shuffling one input. Results depend on the model, score, evaluation data, and correlated substitutes. [Definitions and limitations](https://scikit-learn.org/stable/modules/permutation_importance.html).
- **Conditional subgroup importance:** forms interpretable subgroups with trees, then permutes within them to approximate conditional feature importance; also adapts PDPs. [Molnar et al.](https://arxiv.org/abs/2006.04628) (2020/2023).
- **Dependence and extrapolation:** breaking feature relationships can make importance and effect plots reflect behavior in sparsely observed regions. [Hooker and Mentch](https://arxiv.org/abs/1905.03151v1) (2019) analyze this failure and alternatives.
- **SAGE:** assigns Shapley values to features using their contribution to expected predictive loss reduction. [Covert et al.](https://arxiv.org/abs/2004.00668) (NeurIPS 2020). This measures predictive power rather than averaging local prediction attributions.
- **Model class reliance:** ranges over feature importance across a specified class of sufficiently accurate predictors. [Fisher et al.](https://jmlr.org/papers/v20/18-760.html) (JMLR 2019). Different models can rely on different inputs; the class and performance tolerance define the range.

## Attribution and local explanations

### Feature attribution

- **SHAP:** decomposes an individual prediction into additive Shapley contributions. [Lundberg and Lee](https://papers.nips.cc/paper/7062-a-unified-approach-to-interpreting-model-predictions.pdf) (2017); [code](https://github.com/shap/shap). Aggregation summarizes contributions over chosen observations; it does not turn them into loss-based importance.
- **TreeSHAP:** computes tree-model attributions under specified assumptions about missing or dependent features. [Lundberg et al.](https://arxiv.org/abs/1802.03888) (2018); [TreeExplainer](https://shap.readthedocs.io/en/latest/generated/shap.TreeExplainer.html). Background data and perturbation mode affect the explanation.
- **Shapley-value choices:** the value function and treatment of absent features define the attribution problem; different constructions can disagree. [Sundararajan and Najmi](https://proceedings.mlr.press/v119/sundararajan20b.html) (ICML 2020).
- **Duplicate-input example:** let X2=X1 be a fair bit and f(x)=x1. At (1,1), conditional Shapley gives (0.25,0.25); marginal replacement gives (0.5,0). [Game definitions](https://proceedings.mlr.press/v119/sundararajan20b.html) change whether an unused but informative input receives credit.
- **Integrated Gradients:** integrates input gradients along a straight path from a baseline to the observation. [Sundararajan, Taly, Yan](https://proceedings.mlr.press/v70/sundararajan17a.html) (ICML 2017). Attributions sum to the output difference; the baseline is part of the explanation.
- **DeepLIFT:** propagates activation differences relative to reference activations to assign input contributions. [Shrikumar, Greenside, Kundaje](https://proceedings.mlr.press/v70/shrikumar17a.html) (ICML 2017); [code](https://github.com/kundajelab/deeplift).
- **Layer-wise relevance propagation (LRP):** redistributes a prediction backward through layers using relevance-propagation rules. [Bach et al.](https://doi.org/10.1371/journal.pone.0130140) (2015). The propagation rule determines the decomposition.
- **Grad-CAM:** weights convolutional feature maps with target gradients to produce a coarse spatial attribution map. [Selvaraju et al.](https://arxiv.org/abs/1610.02391) (2016/2017). A heatmap's visual appeal does not establish faithfulness.
- **Attribution guarantees:** [IG completeness](https://proceedings.mlr.press/v70/sundararajan17a.html) constrains a sum; [Shapley axioms](https://proceedings.mlr.press/v119/sundararajan20b.html) characterize an allocation for a specified game. Neither property alone identifies a unique internal computation.

### Local surrogates and rules

- **LIME:** fits an interpretable surrogate around one prediction using weighted perturbed samples. [Ribeiro, Singh, Guestrin](https://arxiv.org/abs/1602.04938) (2016); [code](https://github.com/marcotcr/lime). Sampling, neighborhood weights, and surrogate capacity shape the result.
    - **MeLIME:** extends LIME with data-distribution-aware sampling and alternative local interpretable models. [Botari et al.](https://arxiv.org/abs/2009.05818) (2020, arXiv). Its changes address the perturbation and surrogate-fitting stages.
- **Anchors:** searches for rules that preserve a prediction with high probability under a specified perturbation distribution. [Ribeiro, Singh, Guestrin](https://homes.cs.washington.edu/~marcotcr/aaai18.pdf) (AAAI 2018); [code](https://github.com/marcotcr/anchor). Precision depends on that distribution.

### Formal sufficient explanations

- **Abductive explanations:** find input assignments sufficient to preserve a prediction for every completion in the encoded domain. [Ignatiev et al.](https://arxiv.org/abs/1811.10656) (2018). The guarantee requires a model/domain encoding and a sound constraint solver.
- **Minimality and scope:** [subset-minimal](https://arxiv.org/abs/1811.10656) means no assignment can be dropped; minimum-cardinality means fewest assignments. Unlike Anchors’ perturbation-distribution precision, formal sufficiency quantifies over all admissible completions.

## Counterfactual explanations and recourse

- **Counterfactual explanations:** search for nearby input changes that produce a desired model output. [Wachter, Mittelstadt, Russell](https://arxiv.org/abs/1711.00399) (2017/2018). A changed prediction alone does not establish an actionable intervention.
- **Diverse counterfactuals:** DiCE balances proximity and diversity to offer several ways to change a prediction. [Mothilal, Sharma, Tan](https://arxiv.org/abs/1905.07697) (2019/2020); [code](https://github.com/interpretml/DiCE). Feature constraints specify permitted changes.
- **Actionable recourse:** optimizes changes to actionable inputs for linear classifiers. [Ustun et al.](https://arxiv.org/abs/1809.06514) (2018/2019). [Karimi et al.](https://arxiv.org/abs/2002.06278) (2020/2021) distinguish input edits from interventions with downstream causal effects.

<a id="training-examples-concepts-and-representations"></a>

## Training-data attribution

- **Influence functions:** approximate how upweighting a training example changes a test prediction or loss using derivatives of the fitted model. [Koh and Liang](https://proceedings.mlr.press/v70/koh17a.html) (ICML 2017). Deep-network approximations require validation.
- **TracIn:** estimates training-example influence using gradient inner products across saved training checkpoints. [Pruthi et al.](https://proceedings.neurips.cc/paper/2020/hash/e6385d39ec9394f2f3a354d9d2b88eec-Abstract.html) (NeurIPS 2020). This differs from a single-checkpoint Hessian approximation.
- **[Datamodels](https://arxiv.org/abs/2202.00622)** — 2022. Predict a target example’s model output from which training examples were included. Ilyas et al. fit these surrogates using many subset-trained models, enabling dataset counterfactuals; the training algorithm and subset distribution define the target.
- **[TRAK](https://proceedings.mlr.press/v202/park23c.html)** — 2023. Approximates training-data effects using randomly projected model gradients and a kernel-based calculation. Evaluated against retraining-based prediction changes across vision and language tasks; these scores remain approximations to dataset interventions.

## Concepts and representations

- **TCAV:** learns concept directions from examples and measures prediction sensitivity through directional derivatives. [Kim et al.](https://proceedings.mlr.press/v80/kim18d.html) (ICML 2018). Results depend on concept examples and the chosen representation layer.
- **Concept bottleneck models:** predict annotated concepts before predicting the target; editing predicted concept values changes downstream predictions. [Koh et al.](https://proceedings.mlr.press/v119/koh20a.html) (ICML 2020). These are interventions inside the model.
- **Concept leakage:** soft concept outputs can encode task information beyond their labels, even with sequential training. [Mahinpei et al.](https://arxiv.org/html/2106.13314) (2021, preprint) demonstrate this on synthetic and MNIST tasks; concept supervision does not ensure semantic isolation.
- **ProtoPNet:** predicts by matching image regions to learned prototypes associated with training-image patches. [This Looks Like That](https://arxiv.org/abs/1806.10574) (2018/2019). Prototype similarity forms part of the predictor, rather than a separate local surrogate.
- **Probing with controls:** test what a representation makes decodable while checking what the probe itself can memorize. [Hewitt and Liang](https://arxiv.org/abs/1909.03368) (2019). Decodability alone does not establish that the original model uses that information.
- **Visual representations:** [feature visualization](https://distill.pub/2017/feature-visualization/) optimizes inputs for activation; [Network Dissection](https://arxiv.org/abs/1704.05796) measures unit alignment with labeled concepts (2017). Activation patterns and semantic alignment do not by themselves test causal use.

## Mechanistic interpretability

- **Causal abstraction:** aligns neural representations with variables in a proposed causal model and tests corresponding interchange interventions. [Geiger et al.](https://arxiv.org/abs/2106.02997) (2021). This evaluates a specified computational hypothesis, not causal relations in the world.

### Candidate representations

- **Superposition:** controlled toy models store sparse features in overlapping directions, yielding polysemantic neurons. [Elhage et al.](https://arxiv.org/abs/2209.10652) (2022). This motivates sparse dictionaries; it does not establish that a learned dictionary recovers a real network’s true features.
- **Sparse autoencoders:** reconstruct activations as sparse combinations of learned directions, seeking features distributed across neurons. [Towards Monosemanticity](https://transformer-circuits.pub/2023/monosemantic-features/index.html) (2023, technical report); [scaling and evaluation](https://arxiv.org/abs/2406.04093) (2024).
- **Feature absorption:** a child concept can absorb a parent concept in an SAE, causing the parent latent to miss examples. [Chanin et al.](https://arxiv.org/abs/2409.14507) (2024). Larger dictionaries or greater sparsity need not resolve this failure.
- **Native feature baselines:** feed-forward key/value features compete with SAEs and transcoders on the tested interpretability measures. [Ye et al.](https://arxiv.org/abs/2510.22332) (NeurIPS 2025). Dictionary widths and available checkpoints constrain the comparison.
- **SAE seed sensitivity:** different initializations on the same model/data yield different feature dictionaries. [Paulo and Belrose](https://arxiv.org/abs/2501.16615) (2025). Overlap depends on architecture and matching; one fitted SAE is not a unique feature inventory.
- **Neurons versus transcoders:** many late-layer transcoder activations align with MLP neurons. [Shabalin and Piotrowski](https://arxiv.org/abs/2610.24521) (2026 preprint). Sparsified neurons need a learned linear output correction for competitive replacement KL divergence; transcoders better reconstruct outputs.
- **Neuron comparison scope:** neurons match transcoders on top-activation sort tests but fare worse across broader activation quantiles; transcoders better detect fine-grained concepts such as bigrams. [Comparisons](https://arxiv.org/html/2610.24521v1#S2.SS5) (2026), mainly English data.

### Interventions and circuit discovery

- **Activation patching:** replaces internal activations between runs to test their effect on model behavior. [Zhang and Nanda](https://arxiv.org/abs/2309.16042) (2023/2024) show that corruption choices and evaluation metrics can change localization results.
- **Transformer circuit framework:** decomposes attention computation into query–key and output–value paths, including interactions across layers. [Elhage et al.](https://transformer-circuits.pub/2021/framework/index.html) (2021, technical report). The initial analysis studies small attention-only models.
- **Circuit discovery case study:** identifies attention-head roles in GPT-2 small's indirect-object identification task and tests faithfulness, completeness, and minimality. [Wang et al.](https://arxiv.org/abs/2211.00593) (2022/2023). Findings concern that model and task.
- **ACDC:** removes computational edges through activation-patching tests while seeking to retain a specified behavior. [Conmy et al.](https://arxiv.org/abs/2304.14997) (2023). The recovered circuit depends on the dataset, intervention and metric.
- **Sparse Feature Circuits / SHIFT:** humans select task-irrelevant SAE circuit features for ablation, then retrain the classifier head. [Marks et al.](https://arxiv.org/html/2403.19647v3) (2024/ICLR 2025) improve profession classification on gender-balanced biographies in their case study.
- **Circuit tracing:** builds prompt-specific attribution graphs using transcoders that approximate model components. [Ameisen et al.](https://transformer-circuits.pub/2025/attribution-graphs/methods.html) (2025, technical report). Reconstruction error and frozen attention limit the original method.
- **Attention-path extensions:** traces feature interactions through attention query–key computations omitted by the original circuit-tracing method. [Tracing Attention Computation](https://transformer-circuits.pub/2025/attention-qk/index.html) (2025, technical report).
- **Neuron circuits:** MLP activations with RelP attribution yield sparse circuits competitive with SAE circuits on tested tasks. [Arora et al.](https://arxiv.org/abs/2601.22594) (2026). Activation site and attribution rule both matter; sparsity does not establish monosemanticity.

### Mechanism evaluation

- **MIB: circuit localization.** Four tasks and five models; attribution and mask-optimization methods lead on the benchmark’s circuit metrics. [Mueller et al.](https://proceedings.mlr.press/v267/mueller25a.html) (ICML 2025). Scores trade behavioral recovery against circuit size under specified ablations.
- **MIB: causal-variable localization.** Supervised distributed alignment search leads; tested SAE features do not improve over neuron coordinates. [Results](https://proceedings.mlr.press/v267/mueller25a.html) (2025). This compares task-aligned intervention effects, not general feature readability.
- **Circuit-score misranking:** [Geng et al.](https://arxiv.org/html/2610.02098v2) (2026, preprint v2) find intervention scores favoring equally sized circuits with worse held-out behavioral agreement. Restoring intact-model signals reverses selected failures; mechanism identity remains unverified.
- **SAEBench:** evaluates sparse autoencoders across eight metrics spanning interpretation, disentanglement, and downstream uses; reconstruction gains need not improve other scores. [Karvonen et al.](https://arxiv.org/abs/2503.09532) (ICML 2025); [code](https://github.com/adamkarvonen/SAEBench).
- **SAEBench reliability:** [Chanin’s audit](https://arxiv.org/abs/2605.18229) (2026, preprint v1) tests reseed noise, synthetic ground-truth agreement and training-trajectory discrimination. Targeted Probe Perturbation and Spurious Correlation Removal fail multiple tests at canonical settings.
- **Mechanism validation protocol:** record checkpoint, task, intervention, reference behavior and metric; separate hypothesis-discovery examples from held-out tests. [MIB](https://proceedings.mlr.press/v267/mueller25a.html) supplies task datasets and intervention-based comparisons.

<a id="evaluation-and-limitations"></a>

## Evaluation and comparative evidence

- **Evaluation objectives:** distinguish application-grounded, human-grounded, and functionally grounded evaluation. [Doshi-Velez and Kim](https://arxiv.org/abs/1702.08608) (2017, position paper). A proxy score and usefulness to a person answer different questions.
- **Human usefulness:** transparent, few-feature apartment-price models improved prediction simulation, but not advice-following; transparency sometimes impaired error correction. [Poursabzi-Sangdeh et al.](https://arxiv.org/html/1802.07810v5) (CHI 2021), 3,800 participants; effects depended on presentation.
- **Sources of disagreement:** separate explainer approximation noise, changes from model fitting, [different attribution games](https://proceedings.mlr.press/v119/sundararajan20b.html), and [different accurate models](https://jmlr.org/papers/v20/18-760.html). Changing the target is distinct from estimating the same target unreliably.

### Faithfulness, stability, and perturbations

- **Faithfulness versus plausibility:** an explanation's agreement with model behavior differs from whether a person finds it convincing. [Jacovi and Goldberg](https://arxiv.org/abs/2004.03685) (2020) distinguish these evaluation targets.
- **Saliency sanity checks:** randomize model parameters or training labels to test whether explanations depend on learned behavior. [Adebayo et al.](https://arxiv.org/abs/1810.03292) (2018). Some tested methods remained visually similar after randomization.
- **Infidelity and sensitivity:** compare attributed changes with output changes under perturbations, and measure explanation variation. [Yeh et al.](https://arxiv.org/abs/1901.09392) (2019). A constant explanation can minimize sensitivity without being informative.
- **Remove-and-retrain (ROAR):** masks highly ranked inputs and retrains to assess retained predictive information. [Hooker et al.](https://arxiv.org/abs/1806.10758) (2018/2019). This tests a different question from perturbing a fixed predictor.
- **Out-of-distribution perturbations:** masking can make feature-importance scores and evaluation metrics reflect behavior outside the training distribution. [Hase et al.](https://arxiv.org/abs/2106.00786) (2021). Perturbation design is part of the evaluation.
- **Adversarial explanations:** classifiers can behave differently on real and perturbed inputs, concealing biased predictions from LIME/SHAP in the studied attacks. [Slack et al.](https://arxiv.org/abs/1911.02508) (2019/2020).
- **Influence-estimation failures:** network depth, regularization, and test-point choice affect approximation accuracy in the investigated neural networks. [Basu, Pope, Feizi](https://arxiv.org/abs/2006.14651) (2020/2021). Reported influence is not an exact retraining result.

<a id="attention-language-explanations-and-learned-features"></a>

### Attention and language explanations

- **Attention as explanation:** [Jain and Wallace](https://arxiv.org/abs/1902.10186) find alternative attention weights with similar predictions; [Wiegreffe and Pinter](https://arxiv.org/abs/1908.04626) challenge the inference and propose additional tests (2019). The result depends on architecture and evaluation.
- **Chain-of-thought faithfulness:** models can use prompt hints or biases without reporting them in their explanations. [Turpin et al.](https://arxiv.org/abs/2305.04388) (2023); [Chen et al.](https://arxiv.org/abs/2505.05410) (2025, arXiv) test reasoning models. Findings are conditional on the tested tasks and hints.
- **Interpretation pitfalls:** feature dependence, interactions, poor generalization, uncertainty, and unjustified causal readings can invalidate conclusions. [Molnar et al.](https://arxiv.org/abs/2007.04131) (2020); [Limitations of Interpretable ML Methods](https://slds-lmu.github.io/iml_methods_limitations/) (2020).

## Software

- **Interpretable models:** [InterpretML](https://github.com/interpretml/interpret) provides Explainable Boosting Machines and post-hoc methods; [framework paper](https://arxiv.org/abs/1909.09223) (2019).
- **Feature effects and attribution:** [scikit-learn](https://scikit-learn.org/stable/modules/partial_dependence.html) for PDP/ICE and permutation importance; [SHAP](https://github.com/shap/shap), [LIME](https://github.com/marcotcr/lime), [ELI5](https://eli5.readthedocs.io/en/latest/index.html) for model explanations.
- **Neural-network attribution:** [Captum](https://captum.ai/) provides input, neuron, and layer attribution plus evaluation metrics for PyTorch; [paper](https://arxiv.org/abs/2009.07896) (2020). [DeepLIFT](https://github.com/kundajelab/deeplift) supplies reference-based propagation.
- **Explanation toolkits:** [AIX360](https://github.com/Trusted-AI/AIX360) and [Alibi](https://github.com/SeldonIO/alibi) collect multiple explanation methods; [DiCE](https://github.com/interpretml/DiCE) generates counterfactual examples.
- **Mechanistic analysis:** [TransformerLens](https://github.com/TransformerLensOrg/TransformerLens) exposes activations and interventions; [SAELens](https://github.com/decoderesearch/SAELens) trains and analyzes sparse autoencoders; [circuit-tracer](https://github.com/decoderesearch/circuit-tracer) constructs attribution graphs and supports interventions.
- **Explanation evaluation:** [Quantus](https://github.com/understandable-machine-intelligence-lab/Quantus) collects metrics for neural-network explanations; [paper](https://arxiv.org/abs/2202.06861) (2022). Metric assumptions and perturbation choices remain part of the comparison.
- **TSViz:** combines input/filter influence, filter clustering, and inverse optimization to inspect convolutional time-series models. [Siddiqui et al.](https://arxiv.org/abs/1802.02952) (2018).

## Surveys and perspectives

- [Interpretable Machine Learning](https://christophm.github.io/interpretable-ml-book/) — Molnar. Interpretable models, feature effects, attribution, examples, and limitations.
- [Explanatory Model Analysis](https://pbiecek.github.io/ema/) (2021) — Biecek and Burzykowski. Model diagnostics and local/global explanations, with R and Python examples.
- **Interpretable models versus post-hoc explanations:** Rudin argues for directly interpretable predictors in high-stakes settings where suitable models can be built. [Perspective](https://arxiv.org/abs/1811.10154) (2018/2019).
- **Already Clean:** [Goodfire research essay](https://already-clean.goodfire.com/) (2026) argues that better visualizations and concept descriptions explain apparent polysemanticity. Its neuron examples motivate direct baselines; broad cleanliness claims remain preliminary.

### Applications

- **Glucose forecasting:** applies RETAIN to longitudinal glucose prediction and examines its explanations. [De Bois et al.](https://arxiv.org/abs/2009.04524) (2020). Evidence concerns the studied type-2 diabetes cohort.

## Related topics

[Feature selection](https://mlpapers.org/feature-selection/) · [Feature extraction](https://mlpapers.org/feature-extraction/) · [Causal inference](https://mlpapers.org/causal-inference/) · [Neural networks](https://mlpapers.org/neural-nets/) · [Ensemble learning](https://mlpapers.org/ensemble-learning/) · [Models](https://mlpapers.org/models/).
