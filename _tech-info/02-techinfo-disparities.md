---
title: "Evaluating predictive models for disparate performance"
permalink: /tech-info/disparities/
excerpt: "Evaluating predictive models for disparate performance"
last_modified_at: 2024-11-06
toc: true
---

Methods for evaluting a model for disparities often depend on whether or not the evaluator has access to demographic data
about individuals in the dataset. Here, we consider both possibilities and give suggestions for evaluation methodologies. 

### Demographic data available

There are several useful metrics for measuring demographic disparities. For information on their definitions, see [this resource](https://fairlearn.org/main/user_guide/assessment/common_fairness_metrics.html) from the fairlearn project. The short sample below shows how `fairlearn`'s implementations can be imported into your Python code and used.  

```python
from fairlearn.metrics import demographic_parity_difference
from fairlearn.metrics import equalized_odds_difference
from fairlearn.metrics import demographic_parity_ratio
from fairlearn.metrics import equalized_odds_ratio

def get_fairness_metrics(y_true, y_pred, sensitive_features):
   dpd = demographic_parity_difference(
    y_true, y_pred, sensitive_features=sensitive_features
   )

   dpr = demographic_parity_ratio(
    y_true, y_pred, sensitive_features=sensitive_features
   )

   eod = equalized_odds_difference(
    y_true, y_pred, sensitive_features=sensitive_features
   )

   eor = equalized_odds_ratio(
    y_true, y_pred, sensitive_features=sensitive_features
   )
   
   return np.array([dpd, dpr, eod, eor])
```

### Demographic data unavailable

When an evaluator does not have direct access to individual-level demographic data, there two primary options. The first is to construct a metric that is not reliant on that data. The second is to use privacy-enhancing technologies (PETs) to do computations on another owner's sensitive data without ever seeing it ourselves.

#### Modified fairness metrics

One way to modify the fairness metric is to use coarser demographic information, for example at the county level. For example, researchers at Twitter [used county-level demographic information](https://arxiv.org/abs/2211.08667) to evaluate the allocation of attention on the home timeline. The `jurity` package, created by [Fidelity](https://github.com/fidelity/jurity), contains routines for computing probabilistic fairness metrics when the true group membership is unknown. 

Another way is to use a metric that inherently does not do a groupwise comparison. An example of such metrics are "inequality metrics," which measure the overall skew of a distribution rather than comparing across demographics. The most commonly used of these is the Gini coefficient. [Several](https://dl.acm.org/doi/10.1145/3219819.3220046) [papers](https://arxiv.org/abs/2002.05819) have [employed](https://arxiv.org/abs/2202.01615) this technique. 

#### Privacy enhancing technologies

Recent [work](https://www.christchurchcall.org/safe-secure-private-research-finds-third-parties-can-audit-online-algorithms/) from OpenMined and collaborators as part of the Christchurch Call has demonstrated the computation of inequality metrics on private company data. 


### Applicable statutes

* [Section 2.1]({{ "statutes/section2.1" | relative_url }})