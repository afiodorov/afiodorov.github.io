---
layout: post
title: SHAP feature importances tested
---

I am currently reading [Advances in Financial Machine Learning] by Marcos Lopez
de Prado and the author emphasises examining the trained models before putting
any faith in them - something I wholeheartedly agree with. Since interpreting
models is important, Marcos put several methods of examining feature
importances to the test in a hope to determine weaknesses and strengths of
each method.

I have interest in examining tree-based models and I briefly talked about it in
my previous posts [1][contribs], [2][howard], and I have become an advocate for
using the [SHAP] library for computing feature importances. Marcos examined
permutation feature importance, mean impurity decrease and single-feature
importances (where a classifier is trained on a single feature at a time), and
determined that the first two do quite well: they rank feature that are really
important higher than non-important features.

Unfortunately, SHAP is missing from his analysis, and I decided to replicate
his test on a synthesised data for the library.

## Creating a synthetic dataset

{% highlight python %}
import pandas as pd
from sklearn.datasets import make_classification

n_samples = 10000
n_features = 40
n_informative = 10
n_redundant = 10

X_train, y_train = make_classification(n_samples=n_samples,
                                       n_features=n_features,
                                       n_informative=n_informative,
                                       n_redundant=n_redundant,
                                       shuffle=False)

col_names = [f'I_{i}' for i in range(n_informative)]
col_names += [f'R_{i}' for i in range(n_redundant)]
col_names += [f'N_{i}' for i in range(n_features - n_informative - n_redundant)]

df_train = pd.DataFrame(X_train, columns=col_names)
{% endhighlight %}


Following the methodology of Marcos I created a dataset with 10 informative
features `I_*`, 10 redundant features (those are linear combinations of the
informative) `R_*` and 20 non-informative `N_*`. I then trained a [LightGBM]
classifier and computed the SHAP values. `LightGBM` library computes SHAP
values without installing extra dependencies:

{% highlight python %}
import numpy as np
from lightgbm import LGBMClassifier

classifier = LGBMClassifier()
classifier.fit(df_train, y_train)
shap_values = classifier.predict(df_train, pred_contrib=True)[:, :-1]
shap_feature = np.abs(shap_values).sum(axis=0)
shap_feature = shap_feature / shap_feature.sum()
feature_importances = pd.DataFrame(
    {'SHAP importance': shap_feature, 'feature name': col_names})
{% endhighlight %}

In the above, I sum SHAP contributions for each row. Notice I had to take
absolute value as contributions can be negative. Finally, lets plot the SHAP
feature importances using [Altair]:


{% highlight python %}
import altair as alt

base = alt.Chart(feature_importances)

bar = base.mark_bar().encode(
    x='SHAP importance:Q',
    y=alt.Y("feature name:O",
            sort=alt.EncodingSortField(
                     field='SHAP importance',
                     order='descending')))

rule = base.mark_rule(color='red').encode(
    x='mean(SHAP importance):Q')

(bar + rule).properties(width=630)
{% endhighlight %}


{::nomarkdown}<div id="shap"></div>{:/}

In the above bar chart we see that all informative and redundant features score
higher than non-informative. This is a manifestation of consistency of SHAP
values: more important features should score higher. The red line is the mean
score. We see that certain informative and redundant features, specifically
I_2, I_9, R_6, R_9 and R_5 are below the average. I don't think that's a
particularly bad sign, although Marcos typically remarks on such observation as
it being a negative aspect of the method.

# Conclusion

So far nothing wrong with SHAP values detected. For complete treatment I am including
mean impurity decrease (MID) and permutation (based on [ROC AUC] score) importances.

{::nomarkdown}<div id="mid"></div>{:/}

{::nomarkdown}<div id="perm"></div>{:/}

{::nomarkdown}
<script type="text/javascript">
  var shapSpec = {
    "config": {"view": {"width": 400, "height": 300}, "mark": {"tooltip": null}},
    "layer": [
      {
        "data": {"name": "data-f0d4c35e1a222bc486d13c576f7d6ba1"},
        "mark": "bar",
        "encoding": {
          "x": {"type": "quantitative", "field": "SHAP importance"},
          "y": {
            "type": "ordinal",
            "field": "feature name",
            "sort": {"field": "SHAP importance", "order": "descending"}
          }
        }
      },
      {
        "data": {"name": "data-f0d4c35e1a222bc486d13c576f7d6ba1"},
        "mark": {"type": "rule", "color": "red"},
        "encoding": {
          "x": {
            "type": "quantitative",
            "aggregate": "mean",
            "field": "SHAP importance"
          }
        }
      }
    ],
    "width": 630,
    "$schema": "https://vega.github.io/schema/vega-lite/v3.2.1.json",
    "datasets": {
      "data-f0d4c35e1a222bc486d13c576f7d6ba1": [
        {"SHAP importance": 0.1861217801938816, "feature name": "I_0"},
        {"SHAP importance": 0.042039585809688355, "feature name": "I_1"},
        {"SHAP importance": 0.01811474991082647, "feature name": "I_2"},
        {"SHAP importance": 0.048956041669445165, "feature name": "I_3"},
        {"SHAP importance": 0.028914740802112964, "feature name": "I_4"},
        {"SHAP importance": 0.03299789588820446, "feature name": "I_5"},
        {"SHAP importance": 0.028545032046634358, "feature name": "I_6"},
        {"SHAP importance": 0.03162925071581949, "feature name": "I_7"},
        {"SHAP importance": 0.04304368048591059, "feature name": "I_8"},
        {"SHAP importance": 0.01612365109217281, "feature name": "I_9"},
        {"SHAP importance": 0.02997159605765381, "feature name": "R_0"},
        {"SHAP importance": 0.02383557891666086, "feature name": "R_1"},
        {"SHAP importance": 0.10563188530463495, "feature name": "R_2"},
        {"SHAP importance": 0.14776220473602555, "feature name": "R_3"},
        {"SHAP importance": 0.031178394927841215, "feature name": "R_4"},
        {"SHAP importance": 0.007653437248407346, "feature name": "R_5"},
        {"SHAP importance": 0.015742857807342307, "feature name": "R_6"},
        {"SHAP importance": 0.06333297332452649, "feature name": "R_7"},
        {"SHAP importance": 0.03082321661125894, "feature name": "R_8"},
        {"SHAP importance": 0.011818107387418996, "feature name": "R_9"},
        {"SHAP importance": 0.0010602026154060739, "feature name": "N_0"},
        {"SHAP importance": 0.004361876064579154, "feature name": "N_1"},
        {"SHAP importance": 0.0024498550052693955, "feature name": "N_2"},
        {"SHAP importance": 0.003326819017801216, "feature name": "N_3"},
        {"SHAP importance": 0.002710459928353178, "feature name": "N_4"},
        {"SHAP importance": 0.0027741462698448152, "feature name": "N_5"},
        {"SHAP importance": 0.004711120226047194, "feature name": "N_6"},
        {"SHAP importance": 0.0035981613214511256, "feature name": "N_7"},
        {"SHAP importance": 0.0027267612726391264, "feature name": "N_8"},
        {"SHAP importance": 0.0026501310989316368, "feature name": "N_9"},
        {"SHAP importance": 0.0018550761552829887, "feature name": "N_10"},
        {"SHAP importance": 0.002013281997124515, "feature name": "N_11"},
        {"SHAP importance": 0.0011694100628915722, "feature name": "N_12"},
        {"SHAP importance": 0.0037487927617326318, "feature name": "N_13"},
        {"SHAP importance": 0.0016385902961120263, "feature name": "N_14"},
        {"SHAP importance": 0.0011838501962640976, "feature name": "N_15"},
        {"SHAP importance": 0.004391175164050959, "feature name": "N_16"},
        {"SHAP importance": 0.005313458192549006, "feature name": "N_17"},
        {"SHAP importance": 0.001288294255020786, "feature name": "N_18"},
        {"SHAP importance": 0.0027918771621816187, "feature name": "N_19"}
      ]
    }
  };

  var midSpec = {
    "config": {"view": {"width": 400, "height": 300}, "mark": {"tooltip": null}},
    "layer": [
      {
        "data": {"name": "data-7e5a5bedcf03518b86b79e94a2da7163"},
        "mark": "bar",
        "encoding": {
          "x": {"type": "quantitative", "field": "MID importance"},
          "y": {
            "type": "ordinal",
            "field": "feature name",
            "sort": {"field": "MID importance", "order": "descending"}
          }
        }
      },
      {
        "data": {"name": "data-7e5a5bedcf03518b86b79e94a2da7163"},
        "mark": {"type": "rule", "color": "red"},
        "encoding": {
          "x": {
            "type": "quantitative",
            "aggregate": "mean",
            "field": "MID importance"
          }
        }
      }
    ],
    "width": 630,
    "$schema": "https://vega.github.io/schema/vega-lite/v3.2.1.json",
    "datasets": {
      "data-7e5a5bedcf03518b86b79e94a2da7163": [
        {"MID importance": 0.07866666666666666, "feature name": "I_0"},
        {"MID importance": 0.037333333333333336, "feature name": "I_1"},
        {"MID importance": 0.03266666666666666, "feature name": "I_2"},
        {"MID importance": 0.041666666666666664, "feature name": "I_3"},
        {"MID importance": 0.033, "feature name": "I_4"},
        {"MID importance": 0.07, "feature name": "I_5"},
        {"MID importance": 0.039, "feature name": "I_6"},
        {"MID importance": 0.06433333333333334, "feature name": "I_7"},
        {"MID importance": 0.043, "feature name": "I_8"},
        {"MID importance": 0.027, "feature name": "I_9"},
        {"MID importance": 0.043333333333333335, "feature name": "R_0"},
        {"MID importance": 0.033666666666666664, "feature name": "R_1"},
        {"MID importance": 0.07266666666666667, "feature name": "R_2"},
        {"MID importance": 0.063, "feature name": "R_3"},
        {"MID importance": 0.036333333333333336, "feature name": "R_4"},
        {"MID importance": 0.021, "feature name": "R_5"},
        {"MID importance": 0.022, "feature name": "R_6"},
        {"MID importance": 0.05433333333333333, "feature name": "R_7"},
        {"MID importance": 0.04, "feature name": "R_8"},
        {"MID importance": 0.019333333333333334, "feature name": "R_9"},
        {"MID importance": 0.004, "feature name": "N_0"},
        {"MID importance": 0.01, "feature name": "N_1"},
        {"MID importance": 0.009333333333333334, "feature name": "N_2"},
        {"MID importance": 0.007666666666666666, "feature name": "N_3"},
        {"MID importance": 0.006, "feature name": "N_4"},
        {"MID importance": 0.004, "feature name": "N_5"},
        {"MID importance": 0.005666666666666667, "feature name": "N_6"},
        {"MID importance": 0.008666666666666666, "feature name": "N_7"},
        {"MID importance": 0.005666666666666667, "feature name": "N_8"},
        {"MID importance": 0.008, "feature name": "N_9"},
        {"MID importance": 0.006333333333333333, "feature name": "N_10"},
        {"MID importance": 0.006333333333333333, "feature name": "N_11"},
        {"MID importance": 0.0033333333333333335, "feature name": "N_12"},
        {"MID importance": 0.008, "feature name": "N_13"},
        {"MID importance": 0.0036666666666666666, "feature name": "N_14"},
        {"MID importance": 0.004333333333333333, "feature name": "N_15"},
        {"MID importance": 0.008, "feature name": "N_16"},
        {"MID importance": 0.007, "feature name": "N_17"},
        {"MID importance": 0.0036666666666666666, "feature name": "N_18"},
        {"MID importance": 0.008, "feature name": "N_19"}
      ]
    }
  };

  var permSpec = {
    "config": {"view": {"width": 400, "height": 300}, "mark": {"tooltip": null}},
    "layer": [
      {
        "data": {"name": "data-9b3180fe40192033ad5782ba7c6699a0"},
        "mark": "bar",
        "encoding": {
          "x": {"type": "quantitative", "field": "permutation importance"},
          "y": {
            "type": "ordinal",
            "field": "feature name",
            "sort": {"field": "permutation importance", "order": "descending"}
          }
        }
      },
      {
        "data": {"name": "data-9b3180fe40192033ad5782ba7c6699a0"},
        "mark": {"type": "rule", "color": "red"},
        "encoding": {
          "x": {
            "type": "quantitative",
            "aggregate": "mean",
            "field": "permutation importance"
          }
        }
      }
    ],
    "width": 630,
    "$schema": "https://vega.github.io/schema/vega-lite/v3.2.1.json",
    "datasets": {
      "data-9b3180fe40192033ad5782ba7c6699a0": [
        {"permutation importance": 0.5294478589227452, "feature name": "I_0"},
        {"permutation importance": 0.013132171812124697, "feature name": "I_1"},
        {"permutation importance": 0.007984126708748306, "feature name": "I_2"},
        {"permutation importance": 0.011593602106699154, "feature name": "I_3"},
        {"permutation importance": 0.015592057145309436, "feature name": "I_4"},
        {"permutation importance": 0.03408776512934483, "feature name": "I_5"},
        {"permutation importance": 0.006839102132722428, "feature name": "I_6"},
        {"permutation importance": 0.023718627102749974, "feature name": "I_7"},
        {"permutation importance": 0.013744860401050815, "feature name": "I_8"},
        {"permutation importance": 0.005186395208793541, "feature name": "I_9"},
        {"permutation importance": 0.011979842454114556, "feature name": "R_0"},
        {"permutation importance": 0.00862694752335934, "feature name": "R_1"},
        {"permutation importance": 0.09695545817875346, "feature name": "R_2"},
        {"permutation importance": 0.053770500185815445, "feature name": "R_3"},
        {"permutation importance": 0.010501537200058845, "feature name": "R_4"},
        {"permutation importance": 0.005282270472336333, "feature name": "R_5"},
        {"permutation importance": 0.005924178189199339, "feature name": "R_6"},
        {"permutation importance": 0.030386066858843246, "feature name": "R_7"},
        {"permutation importance": 0.011274017894889729, "feature name": "R_8"},
        {"permutation importance": 0.005241181073675121, "feature name": "R_9"},
        {"permutation importance": 0.004108939866121612, "feature name": "N_0"},
        {"permutation importance": 0.006039228505450724, "feature name": "N_1"},
        {"permutation importance": 0.004868637192480108, "feature name": "N_2"},
        {"permutation importance": 0.005513284202587206, "feature name": "N_3"},
        {"permutation importance": 0.004409349025222529, "feature name": "N_4"},
        {"permutation importance": 0.00460657813879635, "feature name": "N_5"},
        {"permutation importance": 0.0067432268691795675, "feature name": "N_6"},
        {"permutation importance": 0.005855695858097302, "feature name": "N_7"},
        {"permutation importance": 0.00454357439418248, "feature name": "N_8"},
        {"permutation importance": 0.004570054228875282, "feature name": "N_9"},
        {"permutation importance": 0.004499745702277179, "feature name": "N_10"},
        {"permutation importance": 0.005140740321392138, "feature name": "N_11"},
        {"permutation importance": 0.00432534403240403, "feature name": "N_12"},
        {"permutation importance": 0.005552547405752376, "feature name": "N_13"},
        {"permutation importance": 0.0043070820774435, "feature name": "N_14"},
        {"permutation importance": 0.00406419807646829, "feature name": "N_15"},
        {"permutation importance": 0.005506892518351004, "feature name": "N_16"},
        {"permutation importance": 0.0048202430118346636, "feature name": "N_17"},
        {"permutation importance": 0.004244078332829616, "feature name": "N_18"},
        {"permutation importance": 0.005011993538920353, "feature name": "N_19"}
      ]
    }
  };

  vegaEmbed('#shap', shapSpec);
  vegaEmbed('#mid', midSpec);
  vegaEmbed('#perm', permSpec);
</script>
{:/}

And the code:

{% highlight python %}
from sklearn.metrics import roc_auc_score

# Permutations

base = roc_auc_score(y_train, classifier.predict_proba(X_train)[:,1])
importances = []

for i in range(X_train.shape[1]):
    A = X_train.copy()
    A[:, i] = np.random.shuffle(A[:, i])
    proba = classifier.predict_proba(A)[:,1]
    importances.append(base - roc_auc_score(y_train, proba))

importances = np.array(importances) / np.array(importances).sum()
perm_importances = pd.DataFrame(
    {'permutation importance': importances, 'feature name': col_names})

# MID

mid = classifier.feature_importances_ / np.sum(classifier.feature_importances_)
mid_importances = pd.DataFrame(
    {'MID importance': mid, 'feature name': col_names})
{% endhighlight %}

[Advances in Financial Machine Learning]: https://www.amazon.co.uk/Advances-Financial-Machine-Learning-Marcos/dp/1119482089

[contribs]: {%post_url 2016-04-26-feature-contributions %}
[howard]: {%post_url 2019-04-20-notes-on-jeremy-howard-videos %}
[SHAP]: https://github.com/slundberg/shap
[LightGBM]: https://lightgbm.readthedocs.io/en/latest/
[Altair]: https://altair-viz.github.io/
[ROC AUC]: https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html#sklearn.metrics.roc_auc_score
