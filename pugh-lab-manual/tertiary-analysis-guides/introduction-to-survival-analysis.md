---
description: >-
  This section was originally developed by members of the Boutros Lab at OICR.
  Example plots were created using the R plotting package BPG
  (BoutrosLab.plotting.general).
---

# Introduction to Survival Analysis

## What is Survival Analysis?

The purpose of a survival model is to model time-to-event data. In time-to-event data, the quantity of interest is the time elapsed from a baseline timepoint to the occurence of a specific event. Often, the event of interest is death (hence the name, ‘survival analysis’). However, other events such as recurrence of disease or progression of disease can be used. In engineering, survival analysis is called ‘failure-time analysis’, and a typical event of interest is mechanical failure. In sociology, it is refered to as ‘event-history analysis’, and events of interest include divorce and unemployment.

## Basic Survival Analysis Definitions

<table><thead><tr><th width="199">Term</th><th>Definition</th></tr></thead><tbody><tr><td>Censoring</td><td><p>We say a measurement is ‘censored’ when it is only partially known. In survival analysis, there are three types of censoring:</p><p>1) <em><mark style="color:blue;">Right censoring</mark></em> The event is known to happen after a specific time (ie. the subject is alive when the study ends)</p><p>2) <em><mark style="color:blue;">Left censoring</mark></em> The event is know to have happened before a specific time.</p><p>3) <em><mark style="color:blue;">Interval censoring</mark></em> The event is known to have happened between two specific time points.<br>The most common type of censoring is right censoring - it is also the type that has been most studied and can be best handled by existing methods.</p></td></tr><tr><td>Non-parametric estimate</td><td>A non-parametric estimate is an estimate that makes no assumptions about the distribution of the data.</td></tr><tr><td>Parametric estimate</td><td>A parametric estimate assumes that data has a particular distribution and estimates the parameters of the distribution, based on observed data.</td></tr><tr><td>Semi-parametric estimate</td><td>A semi-parametric estimate contains both parametric and non-parametric parts.</td></tr><tr><td>Survival outcome</td><td>The survival outcome is the state of a unit of the population after the study, so the outcome could be that the event occurred, or that the event didn’t occur.</td></tr><tr><td>Hazard</td><td>A hazard rate is the event rate at a specific point in time.</td></tr><tr><td>Hazard Ratio (HR)</td><td>The hazard ratio is a ratio of hazard rates, so is an instantaneous com- parison of two population’s hazard rates. A hazard ratio is not the same thing as a relative risk (RR).</td></tr><tr><td>Baseline Hazard</td><td>The hazard rate before predictors are considered.</td></tr><tr><td>Proportional Hazard (Assumption)</td><td>The effect of a covariate is multiplicative with respect to hazard rate.</td></tr><tr><td>Survival Profile Stratify / Strata / Stratification</td><td>Stratification involves grouping data then sampling the groups indepen- dently. A strata is a group and stratify means to group a population for sampling.</td></tr><tr><td>Interaction</td><td>Interaction occurs when two variables affect a third variable but not independently of one another.</td></tr><tr><td>Covariate/Predictor</td><td>A covariate or predictor is a variable which may impact the outcome in survival data.</td></tr></tbody></table>

## What is Distinctive About Survival Analysis?

A common feature of survival data is that some of the survival times are censored. In other words, for certain subjects, the true survival time is not known. For these ‘censored’ subjects, we only know that they have not experienced the event (or ‘failed’) by a certain time - but it is still possible that they experience the event in the future. If censoring is not taken into account, the results of any statistical analysis performed on survival data can be very biased.

<figure><img src="../.gitbook/assets/Survival Analysis Figure1.png" alt=""><figcaption></figcaption></figure>

## Defining Startpoint and Endpoint

The first step in conducting a Survival Analysis is to define the start and end points of interest. These must be clearly defined in order to ensure the data is consistent. In the case of the study of disease progression, the start time of interest would perhaps be ‘onset of disease’, but this is usually not known. Often, diagnosis, surgery, or start of treatment are used as the startpoint (i.e., time 0). The endpoint must also be very clearly defined to ensure that subjects are classified correctly. For example, the following are possible endpoints of interest

* death of any cause
* death of a particular cause (lung cancer, for example)
* progression of disease

## Basics of the Kaplan-Meier Estimate of Survival Curve

A Kaplan-Meier survival curve is a non-parametric estimate of the true survival curve. Non- parametric estimates make no assumptions about the distribution of the data, which is convenient as the true distribution is rarely known.

## Interpretation of Kaplan-Meier Curve

In the Figure below, each curve is the estimated survival curve for a particular group, in this case, I, II, III or IV. On the x-axis, we have time, and on the y-axis, we have the estimated probability of survival (i.e., of not experiencing a failure). By convention, at time t = 0 none of the subjects has experienced a failure so we always have Sˆ(0) = 1. For t∗ > 0, Sˆ(t∗) is the estimated probability

<figure><img src="../.gitbook/assets/Survival Analysis Figure2.png" alt="Example of Kaplan-Meier plot"><figcaption><p>Example of Kaplan-Meier plot</p></figcaption></figure>

that a subject has not yet experienced an event by time t∗. For example, in Figure 6.1, the estimated probability that a subject in group I has not experienced a failure in the first 80 months is approximately 0.6. Hence, the survival outlook is better for a group whose KM curve is ‘high’, compared to a group whose KM curve takes on lower values. In a KM curve, a discontinuity occurs each time a subject experiences an event. The height of the jump is proportional to the ratio of the number of subjects that fail at a given time point and the number of subjects at risk of failing (i.e., under observation) at that time. The small vertical lines on a KM curve correspond to censoring times. Note that the KM curve goes down to 0 if and only if the last observation is a failure.

## Basics of Cox Proportional Hazards (PH) Regression

The Cox Proportional Hazards Model is a semi-parametric regression model of the survival curve. In other words, the Cox PH model consists of a non-parametric part and of a parametric part. In the Cox model, no assumptions are made for the form of the baseline hazard (non-parametric part), but the effect of the predictors on the hazard has a parametric form. In particular, the predictors are assumed to have a multiplicative effect on the hazard (or equivalently, an additive effect on the log-hazard). Since the baseline hazard is not specified, the Cox PH model does not estimate the shape of the survival curve in the same way as the Kaplan-Meier estimate does. However, it is very effective at estimating the effect of variables on the hazard, which is ususally what we are interested in anyways. In addition to the assumption of independent censoring, the Cox Model relies on the assumption that the hazards are proportional across all patient groups (where patient groups are defined by all possible combinations of parameter vector values).

### Does the Proportional Hazards Assumption Hold?

To determine whether or not the proportional hazards assumption holds for a Cox model, the cox.zph function in R can be used. This function takes as input a coxph object (i.e., a fitted Cox model) and a tranform. Recommended transforms are “identity” and “log” - ideally, both would be performed. The cox.zph function tests the following hypotheses:

&#x20;    _H0_ : “the hazards are proportional across groups” vs

&#x20;    _HA_ : “the hazards are not proportional across groups”.\
\
This test is performed for each variable in the model, as well as globally for the entire model. If the cox.zph p-value for a particular variable is small, it suggests that the proportional hazards assumption does not hold for that variable. Even if this is not the variable of interest, this is a problem because lack of proportionality for one factor can potentially affect other parameter estimates.

### What do I do if the Proportional Hazards Fails?

#### Method 1: Stratified Model

If a factor fails the Proportional Hazards assumption, a stratified Cox model can be built (stratified on the offending factor). In some cases, the resulting model will satisfy the PH assumption, and standard Cox analysis can be performed. In a stratified Cox model, each stratum can have a different baseline hazard function, but the regression coefficients in the Cox model are assumed to be constant across strata. An advantage of the stratified Cox model approach is that no additional assumptions are required about the form of the model. However, the effect of the stratification parameter can not be estimated, so this is not an adequate approach if the offending covariate is of particular interest. Hence, stratification is most appropriate when we simply want to control for the stratification factor, but don’t really care about its effect. In addition, stratification can only be done on a discrete variable (not continuous), and to avoid sparse strata, it is better if there are not too many levels. If a continuous variable fails the PH assumption, it can be discretized and then used as a stratification factor. Similarly, if a discrete variable with many levels fails the PH assumption, some of the levels can be collapsed together, if necessary.

#### Method 2: Interaction with a Time-Varying Covariate

If a factor fails the Proportional Hazards assumption, introducing an interaction between the offend- ing variable and a time-varying covariate can sometimes resolve the issue. When the proportional hazards assumption fails, it is because the hazard corresponding to a factor is not constant over time. Introducing an interaction with a time varying covariate allows this hazard to vary, in a limited way, over time. In some cases, this leads to the Proportional Hazards assumption holding for the model.

#### Method 3: Use a Non-Parametric Test

If the Proportional Hazards assumption continues to fail after trying to build stratified Cox models and introducing interactions with time-varying covariates, it may be that Cox modelling can not be done. In this case, a non-parametric test such as the log-rank test can be used, as it does not rely on the Proportional Hazards assumption.

## Testing for Differences between Patient Groups

A number of tests can be used to test whether or not the survival curves for two or more groups of patients are equal or not.

### Log-rank Test

The log-rank test tests the following hypotheses:\
&#x20;    _H0_ : “no difference between true survival curves” vs \
&#x20;    _HA_ : “the true survival curves are different”.

The log-rank test can be viewed as a time-stratified Cochran-Mantel-Haenszel test. Since the underlying survival distribution does not need to be specified, the log-rank test is a non-parametric test. An important assumption for the log-rank test is independent censoring. In other words, the censoring process must be completely independent of the failure process. Though a small p- value for the log-rank test suggests that survival profiles differ between patient groups, a large p-value does not imply that there is no difference between groups. In particular, if the hazards are not proportional, the log-rank test will yield a large p-value, even though the survival profiles can be quite different. However, it can be difficult to determine whether or not the hazards are proportional, as direct estimates of the hazard function are typically highly variable. Note that if the survival functions cross, the hazard functions must also cross (i.e., fail to be proportional), but it is possible for the hazards to cross even when the survival functions do not cross. When the hazards are not proportional, a variation of the log-rank test, the WilCoxon test, is more powerful. The WilCoxon test is very similar to the log-rank test, but uses different weights in the test statistic. In the log-rank test, failures are all weighted equally, no matter when they occur, while in the WilCoxon test, early failures have higher weight than later failures.

### Cox Testing

If the proportional hazards assumption holds, it is reasonable to fit a Cox model to estimate survival, and to compare the survival of different groups via the significance of the effect parameter for the corresponding factor. In a Cox model, the parameter estimates are Hazard Ratios (or log Hazard Ratios, technically). If the Hazard Ratio (HR) for a factor is statistically significant, we can say that there is a difference in survival between patient groups defined by levels of that factor. If the proportional hazards assumption does not hold, Cox modelling should not be done (though there are techniques that can be used to tweak the Cox model to try to make the PH assumption hold).

### T-test

If none of the survival times in a dataset are censored, a simple t-test can be used to compare the mean survival time between patient groups.

### References

* Clark, T., Bradburn, M., Love, S. et al. (2003). Survival Analysis Part I: Basic concepts and first analyses. Br J Cancer 89, 232–238. [https://doi.org/10.1038/sj.bjc.6601118](https://doi.org/10.1038/sj.bjc.6601118)
* For further tutorials: [https://www.nature.com/bjc/articles?type=tutorial-paper](https://www.nature.com/bjc/articles?type=tutorial-paper)
* P’ng, E., et al. (2019) BPG: Seamless, automated and interactive visualization of scientific data. BMC Bioinformatics, [https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-019-2610-2](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-019-2610-2)

