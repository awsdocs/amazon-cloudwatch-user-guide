# How Evidently calculates results<a name="CloudWatch-Evidently-calculate-results"></a>

You can use Amazon CloudWatch Evidently A/B testing as a tool for data\-driven decision making\. In an A/B test, users are randomly assigned to either the control group \(also called the default variation\), or one of the treatment groups \(also called the tested variations\)\. For example, users in the control group might experience the website, service, or application in the same way that they did before the experiment started\. Meanwhile, users in the treatment group might experience the change\.

CloudWatch Evidently supports up to five different variations in an experiment\. Evidently randomly assigns traffic to these variations\. This way, you can track business metrics \(such as revenue\) and performance metrics \(such as latency\) for each group\. Evidently does the following: 
+ Compares the treatment with the control\. \(For example, compares whether revenue increases or decreases with a new checkout process\.\) 
+ Indicates whether the observed difference between the treatment and the control is *significant*\. For this, Evidently offers two approaches: *Frequentist significance levels* and *Bayesian probabilities*\. 

## Why use Frequentist and Bayesian approaches?<a name="CloudWatch-Evidently-calculate-results-approaches"></a>

Consider a case where the treatment has no effect compared to the control, or a case where the treatment is identical to the control \(an *A/A test*\)\. You would still observe a small difference between the treatment and the control in the data\. This is because the test participants consist of a finite sample of users, representing a small percentage of all users of the website, service, or application\. Frequentist significance levels and Bayesian probabilities provide insights into whether the observed difference is significant or due to chance\.

Evidently considers the following to determine whether the observed difference is significant: 
+ How big the difference is
+ How many samples are part of the test
+ How the data is distributed

## Frequentist analysis in Evidently<a name="CloudWatch-Evidently-calculate-results-Frequentist"></a>

Evidently uses sequential testing, which avoids the usual problems of *peeking*, a common pitfall of frequentist statistics\. Peeking is the practice of checking the results of an ongoing A/B test in order to stop it and make a decision based on the observed results\. For more information about sequential testing, see [ Time\-uniform, nonparametric, nonasymptotic confidence sequences](https://projecteuclid.org/journals/annals-of-statistics/volume-49/issue-2/Time-uniform-nonparametric-nonasymptotic-confidence-sequences/10.1214/20-AOS1991.full) by Howard et al\. \(Ann\. Statist\. 49 \(2\) 1055 \- 1080, 2021\)\.

Because Evidently's results are valid at any time \(*anytime\-valid* results\), you can peek at results during the experiment and still draw sound conclusions\. This can reduce some of the costs of experimentation, because you can stop an experiment before the scheduled time if the results already have significance\. 

Evidently generates anytime\-valid significance levels and anytime\-valid 95% confidence intervals of the difference between the tested variation and the default variation in the target metric\. The **Result** column in the experiment results indicates the tested variation performance, which can be one of the following: 
+ **Inconclusive** – The significance level is less than 95%
+ **Better** – The significance level is 95% or higher and one of the following is true:
  + The lower bound of the 95%\-confidence interval is higher than zero and the metric should increase
  + The upper bound of the 95%\-confidence interval is lower than zero and the metric should decrease
+ **Worse** – The significance level is 95% or higher and one of the following is true:
  + The upper bound of the 95%\-confidence interval is higher than zero and the metric should increase
  + The lower bound of the 95%\-confidence interval is lower than zero and the metric should decrease
+ **Best** – The experiment has two or more tested variations in addition to the default variation, and the following conditions are met: 
  + The variation qualifies for the *Better* designation
  + One of the following is true:
    + The lower bound of the 95%\-confidence interval is higher than the upper bound of the 95%\-confidence intervals of all the other variations and the metric should increase
    + The upper bound of the 95%\-confidence interval is lower than the lower bound of the 95%\-confidence intervals of all the other variations and the metric should decrease

## Bayesian analysis in Evidently<a name="CloudWatch-Evidently-calculate-results-Bayesian"></a>

With Bayesian analysis, you can calculate the probability that the mean in the tested variation is larger or smaller than the mean in the default variation\. Evidently performs Bayesian inference for the mean of the target metric by using *conjugate priors*\. With conjugate priors, Evidently can more efficiently infer the posterior distribution needed for the Bayesian analysis\.

Evidently waits until the end date of the experiment to compute the results of the Bayesian analysis\. The results page displays the following:
+ *probability of increase* – The probability that the mean of the metric in the tested variation is at least 3% larger than the mean in the default variation
+ *probability of decrease* – The probability that the mean of the metric in the tested variation is at least 3% smaller than the mean in the default variation
+ *probability of no change* – The probability that the mean of the metric in the tested variation lies within ±3% of the mean in the default variation

The **Result** column indicates the performance of the variation, and can be one of the following:
+ **Better** – The probability of increase is at least 90% and the metric should increase, or the probability of decrease is at least 90% and the metric should decrease
+ **Worse** – The probability of decrease is at least 90% and the metric should increase, or the probability of increase is at least 90% and the metric should decrease 