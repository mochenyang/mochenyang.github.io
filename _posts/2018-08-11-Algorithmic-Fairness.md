---
layout: post
title:  "Bias, Discrimination, and Algorithmic Fairness"
date:   2018-08-11
---

This is a living document on recent literature concerning discrimination and fairness issues in decisions driven by machine learning algorithms. In particular, it covers two broad topics: (1) the definition of fairness, and (2) the detection and prevention/mitigation of algorithmic bias.

## Understanding Fairness

Various notions of fairness have been discussed in different domains. Different fairness definitions are not necessarily compatible with each other, in the sense that it may not be possible to simultaneously satisfy multiple notions of fairness in a single machine learning model.

### Definition of Fairness

A common notion of fairness distinguishes **direct discrimination** and **indirect discrimination**. Direct discrimination happens when a person is treated less favorably than another person in comparable situation on protected ground (Romei and Ruggieri 2013; Zliobaite 2015). Here, _comparable situation_ means the two persons are otherwise similarly except on a _protected_ attribute, such as gender, race, etc. In contrast, indirect discrimination happens when an "apparently neutral practice put persons of a protected ground at a particular disadvantage compared with other persons" (Zliobaite 2015).

The two main types of discrimination are often referred to by other terms under different contexts. Direct discrimination is also known as _systematic discrimination_ or _disparate treatment_, and indirect discrimination is also known as _structural discrimination_ or _disparate outcome_. The disparate treatment/outcome terminology is often used in legal settings (e.g., Barocas and Selbst 2016).

More operational definitions of fairness are available for specific machine learning tasks. Consider a binary classification task. The outcome/label represent an important (binary) decision (```pos``` and ```neg```). One of the features is protected (e.g., gender, race), and it separates the population into several non-overlapping **groups** (e.g., ```GroupA``` and ```GroupB```). The classifier estimates the probability that a given instance belongs to ```pos``` based on its features. Two notions of fairness are often discussed (e.g., Kleinberg et al. 2016): **calibration within group** and **balance**. 

Calibration within group means that for both groups, among persons who are assigned probability _p_ of being ```pos```, there should be _p_ fraction of them that actually belong to ```pos``` in expectation. In other words, a probability score should mean what it literally means (in a frequentist sense) regardless of group. A violation of calibration means decision-maker has incentive to interpret the classifier's result differently for different groups, leading to disparate treatment. 

Balance is class-specific. Balance for ```pos``` class means that average probability assigned to people in ```GroupA``` who are actually ```pos``` should be equal to the average probability assigned to people in ```GroupB``` who are actually ```pos```. Balance for ```neg``` can be analogously defined. Balance intuitively means the classifier is not disproportionally inaccurate towards people from one group than the other. In other words, condition on the actual label of a person, the chance of misclassification is independent of the group membership. A violation of balance means that, among people who have the same outcome/label, those in one group are treated less favorably (assigned different probabilities) than those in the other. Balance can be formulated equivalently in terms of error rates, under the notion of **equalized odds** (Pleiss et al. 2017). In statistical terms, balance for a class is a type of conditional independence. It means that _condition on the true outcome_, the predicted probability of an instance belong to that class is independent of its group membership.

Some other fairness notions are available. _Statistical Parity_ requires members from the two groups should receive the same probability of being ```pos```. One should not confuse statistical parity with balance, as the former does not concern about the actual outcomes - it simply requires average predicted probability of ```pos``` to be equal for two groups. 

Fairness notions are slightly different (but conceptually related) for numeric prediction or regression tasks. Calders et al. (2013) discuss two definitions. First, **equal means** requires the average predictions for people in the two groups should be equal. This is conceptually similar to _statistical parity_ in classification. Second, **balanced residuals** requires the average residuals (errors) for people in the two groups should be equal. This is conceptually similar to _balance_ in classification.


### Relationship among Different Fairness Definitions

First, **not all fairness notions are equally important in a given context**. For example, when base rate (i.e., the actual proportion of ```pos``` in a population) differs in the two groups, statistical parity may not be feasible (Kleinberg et al., 2016; Pleiss et al., 2017).

Second, **not all fairness notions are compatible with each other**. Kleinberg et al. (2016) shows that the three notions of fairness in binary classification, i.e., calibration within groups, balance for ```pos``` class, and balance for ```neg``` class cannot be achieved simultaneously, unless under one of two trivial cases: (1) perfect prediction, or (2) equal base rates in two groups. Such impossibility holds even approximately (i.e., approximate calibration and approximate balance cannot all be achieved unless under approximately trivial cases). Pleiss et al. (2017) extends their work and shows that, when base rates differ, calibration is compatible only with a substantially relaxed notion of balance, i.e., weighted sum of false positive and false negative rates is equal between the two groups, with at most one particular set of weights. These incompatibility findings indicates trade-offs among different fairness notions.

### Relationship between Fairness and Predictive Performance

In general, a discrimination-aware prediction problem is formulated as a constrained optimization task, which aims to achieve highest accuracy possible, without violating fairness constraints. There is evidence suggesting trade-offs between fairness and predictive performance. Corbett-Davies et al. (2017) demonstrates that maximizing predictive accuracy with a single threshold (that applies to both groups) typically violates fairness constraints. Conversely, fairness-preserving models with group-specific thresholds typically come at the cost of overall accuracy. In essence, the trade-off is again due to different base rates in the two groups. Hardt et al. (2016) proposed algorithms to determine group-specific thresholds that maximize predictive performance under balance constraints, and similarly demonstrated the trade-off between predictive performance and fairness.

Importantly, such trade-off does not mean that one needs to build inferior predictive models in order to achieve fairness goals. In particular, in Hardt et al. (2016), the classifier is still built to be as accurate as possible, and fairness goals are achieved by adjusting classification thresholds. In the same vein, Kleinberg et al. (2018a) proved that "an equity planner" with fairness goals should still build the same classifier as one would without fairness concerns, and adjust decision thresholds. Moreover, such a classifier should take into account the protected attribute (i.e., group identifier) in order to produce correct predicted probabilities. 

## Addressing Algorithmic Bias

### Measurement and Detection

Both Zliobaite (2015) and Romei et al. (2013) surveyed relevant measures of fairness or discrimination. One may compare the number or proportion of instances in each group classified as certain class. Sometimes, the measure of discrimination is mandated by law. For instance, the _forth-fifth rule_ (Romei et al. 2013) in hiring context requires the job selection rate for the protected group is at least 80% that of the other group.

If fairness or discrimination is measured as the number or proportion of instances in each group classified to a certain class, then one can use standard statistical tests (e.g., two sample t-test) to check if there is systematic/statistically significant differences between groups. Zliobaite (2015) reviewed a large number of such methods.

In the particular context of machine learning, previous definitions of fairness offer straightforward measures of discrimination. For instance, the degree of balance of a binary classifier for the positive class can be measured as the difference between average probability assigned to people with positive class in the two groups.

### Prevention/Mitigation

Techniques to prevent/mitigate discrimination in machine learning can be put into three categories (Zliobaite 2015; Romei et al. 2013): (1) **data pre-processing**, (2) **algorithm modification**, and (3) **model post-processing**.

**Data pre-processing** tries to manipulate training data to get rid of discrimination embedded in the data. 

**Algorithm modification** directly modifies machine learning algorithms to take into account fairness constraints. Calders and Verwer (2010) propose to modify naive Bayes model in three different ways: (i) change the conditional probability of a class given the protected attribute; (ii) train two separate naive Bayes classifiers, one for each group, using data only in each group; and (iii) try to estimate a "latent class" free from discrimination.

**Model post-processing** changes how the predictions are made from a model in order to achieve fairness goals. 



## References

Calders, T., & Verwer, S. (2010). Three naive Bayes approaches for discrimination-free classification. _Data Mining and Knowledge Discovery_, 21(2), 277–292.

Kleinberg, J., Mullainathan, S., & Raghavan, M. (2016). Inherent Trade-Offs in the Fair Determination of Risk Scores. [arXiv](https://arxiv.org/pdf/1609.05807.pdf)

Pleiss, G., Raghavan, M., Wu, F., Kleinberg, J., & Weinberger, K. Q. (2017). On Fairness and Calibration. [arXiv](http://arxiv.org/abs/1709.02012)

Kleinberg, J., Ludwig, J., Mullainathan, S., & Rambachan, A. (2018a). Algorithmic Fairness. _AEA Papers and Proceedings_, 108, 22–27.

Kleinberg, J., & Raghavan, M. (2018b). Selection Problems in the Presence of Implicit Bias. [arXiv](https://arxiv.org/pdf/1801.03533.pdf)

Kleinberg, J., Lakkaraju, H., Leskovec, J., Ludwig, J., & Mullainathan, S. (2017). Human decisions and machine predictions. _The quarterly journal of economics_, 133(1), 237-293.

Calders, T., Karim, A., Kamiran, F., Ali, W., & Zhang, X. (2013). Controlling attribute effect in linear regression. Proceedings - IEEE International Conference on Data Mining, _ICDM_, (1), 71–80.

Custers, B. (2013). Discrimination and Privacy in the information society. Discrimination and Privacy in the Information Society (Vol. 3).

Zliobaite, I. (2015). A survey on measuring indirect discrimination in machine learning. [arXiv](https://arxiv.org/pdf/1511.00148.pdf)

Barocas, S., & Selbst, A. (2016). Big Data's Disparate Impact. _California Law Review_, 104(1), 671–729. 

Romei, A., & Ruggieri, S. (2013). A multidisciplinary survey on discrimination analysis. _Knowledge Engineering Review_, 29(5), 582–638.

Hardt, M., Price, E., & Srebro, N. (2016). Equality of Opportunity in Supervised Learning, (Nips).

Corbett-Davies, S., Pierson, E., Feller, A., Goel, S., & Huq, A. (2017). Algorithmic decision making and the cost of fairness. [arXiv](https://arxiv.org/pdf/1701.08230.pdf)

Zliobaite, I. (2015). On the relation between accuracy and fairness in binary classification. [arXiv](https://arxiv.org/pdf/1505.05723.pdf)

## Other Resources

[Emergence of Intelligent Machines](https://www.youtube.com/channel/UC6f1sYAoj6_sxF-lhroS6SQ): a series of talks on algorithmic fairness, biases, interpretability, etc.
