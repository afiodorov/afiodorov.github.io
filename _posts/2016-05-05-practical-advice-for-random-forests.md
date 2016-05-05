---
layout: post
title: Practical advice for using Random Forests
---

After reading a chapter on Random Forests from "Elements of Statistical
Learning", I made the following notes:

* Performance of Random Forests decreases drastically when a number
  of uninformative features is increased.
* Random Forest underperform compared to boosting, but only slightly.
* Keep `max_depth=None` and set `min_samples_leaf` instead. Controlling the
  depth of a tree can only increase the performance marginally and it's not
  worth having to grid-search over an extra parameter.
* Keep the number of trees, `n_estimators` high. RFs don't overfit when this
  parameter is increased.
* Random Forest work by reducing variance of individual trees. One important
  mechanism to achieve it is through column selection so optimising over
  `max_features` should be beneficial.
* Cross-validation isn't necessary as bootstrapping allows one to use
  out-of-bag errors instead enabling cross-validation and fitting in one go.
  ESL shows that CS errors are very close to OOB errors, so use
  `oob_score=True`.
* Don't bother with proximity plots for classification trees.

I referred to all parameters by names from the [scikit-learn package][sk].

[sk]: http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html
