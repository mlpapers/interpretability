# Interpretability, Explainable AI
Interpretable Machine Learning. Overview of different methods:
- [Interpretable Machine Learning](https://christophm.github.io/interpretable-ml-book/) *Christoph Molnar*
- [Explanatory Model Analysis](https://pbiecek.github.io/ema/) *Przemyslaw Biecek, Tomasz Burzykowski*

### Interpretable models
- **RETAIN** ([Code](https://github.com/mp2893/retain))
  - [RETAIN: An Interpretable Predictive Model for Healthcare using Reverse Time Attention Mechanism](https://arxiv.org/pdf/1608.05745) (2016) *Edward Choi, Mohammad Taha Bahadori, Joshua A. Kulas, Andy Schuetz, Walter F. Stewart, Jimeng Sun*
  - [Interpreting Deep Glucose Predictive Models forDiabetic People Using RETAIN](https://arxiv.org/pdf/2009.04524.pdf) (2020) *Maxime De Bois, Mounim A. El Yacoubi, Mehdi Ammi*

### Local explanations
- LIME. Local Interpretable Model-Agnostic Explanations ([Code](https://github.com/marcotcr/lime), [CRAN](https://cran.r-project.org/web/packages/lime/), [PyPI](https://pypi.org/project/lime/))
  - [“Why should I trust you?” Explaining the predictions of any classifier](https://arxiv.org/pdf/1602.04938.pdf) (2016) *Marco Tulio Ribeiro, Sameer Singh, Carlos Guestrin*
  - [MeLIME: Meaningful Local Explanation forMachine Learning Models](https://arxiv.org/pdf/2009.05818.pdf) (2020) *Tiago Botari, Frederik Hvilshøj, Rafael Izbicki, Andre C. P. L. F. de Carvalho*

### Global explanations
- **Feature importance**
  - [Model-agnostic Feature Importance and Effectswith Dependent Features – A Conditional Subgroup Approach](https://arxiv.org/pdf/2006.04628.pdf) (2020) *Christoph Molnar, Gunnar Konig, Bernd Bischl, Giuseppe Casalicchio*
- SHAP. Shapley Additive Exlanations ([Code](https://github.com/slundberg/shap), [PyPI](https://pypi.org/project/shap/))
  - [A Unified Approach to Interpreting ModelPredictions](https://papers.nips.cc/paper/7062-a-unified-approach-to-interpreting-model-predictions.pdf) (2017) *Scott M. Lundberg, Su-In Lee*
- DeepLift. Deep Learning Important Features ([Code](https://github.com/kundajelab/deeplift), [Video](https://www.youtube.com/playlist?list=PLJLjQOkqSRTP3cLB2cOOi_bQFw6KPGKML))
  - [Learning Important Features Through Propagating Activation Differences](https://arxiv.org/pdf/1704.02685.pdf) (2019) *Avanti Shrikumar1, Peyton Greenside1, Anshul Kundaje*

### Timeseries
- TSViz
  - [TSViz: Demystification of Deep Learning Models for Time-Series Analysis](https://arxiv.org/pdf/1802.02952.pdf) (2018) *Siddiqui S.A., Mercier D., Munir M., Dengel A., Ahmed S.*

### Critics
- [Stop explaining black box machine learning models for high stakes decisions and use interpretablemodels instead](https://arxiv.org/pdf/1811.10154.pdf) (2019) *Cynthia Rudin*
- [Limitations of Interpretable Machine Learning Methods](https://compstat-lmu.github.io/iml_methods_limitations/) (2020)
- [Please Stop Permuting Features. An Explanation and Alternatives](https://arxiv.org/pdf/1905.03151v1.pdf) (2019) *Giles Hooker, Lucas Mentch*
- [Attention is not Explanation](https://arxiv.org/pdf/1902.10186.pdf) (2019) *Sarthak Jain, Byron C. Wallace*
- [Pitfalls to Avoid when Interpreting Machine Learning Models](https://arxiv.org/pdf/2007.04131.pdf) (2020) *Christoph Molnar, Gunnar Konig, Julia Herbinger, Timo Freiesleben, Susanne Dandl, Christian A. Scholbeck, Giuseppe Casalicchio, Moritz Grosse-Wentrup, Bernd Bisch*

## Software
- **Python**
  - Captum - model interpretability and understanding for PyTorch ([Homepage](https://captum.ai/), [Paper](https://arxiv.org/pdf/2009.07896.pdf))
  - AIX360 ([Homepage](http://aix360.mybluemix.net/))
  - Alibi ([Homepage](https://docs.seldon.io/projects/alibi/), [PyPI](https://pypi.org/project/alibi/))
  - Eli5 ([Homepage](https://eli5.readthedocs.io/en/latest/index.html), [PyPI](https://pypi.org/project/eli5/))


## Related Topics
- [Neural Networks](https://mlpapers.org/neural-nets/)
- [Models](https://mlpapers.org/models/)
- [Feature Selection](https://mlpapers.org/feature-selection/)
- [Ensemble Learning](https://mlpapers.org/ensemble-learning/)
