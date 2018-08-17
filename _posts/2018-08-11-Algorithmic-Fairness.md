---
layout: post
title:  "Bias, Discrimination, and Algorithmic Fairness"
date:   2018-08-11
---

This is a living document on recent literature concerning discrimination and fairness issues in decisions driven by machine learning algorithms. In particular, it covers two broad topics: (1) the definition of fairness, and (2) the detection and prevention/mitigation of algorithmic bias.

# Understanding Fairness

Various notions of fairness have been discussed in different domains. Different fairness definitions are not necessarily compatible with each other, in the sense that it may not be possible to simultaneously satisfy multiple notions of fairness in a single machine learning model.

## Definition of Fairness

A common notion of fairness distinguishes **direct discrimination** and **indirect discrimination**. Direct discrimination happens when a person is treated less favorably than another person in comparable situation on protected ground (Romei and Ruggieri 2013; Zliobaite 2015). Here, _comparable situation_ means the two persons are otherwise similarly except on a _protected_ attribute, such as gender, race, etc. In contrast, indirect discrimination happens when an "apparently neutral practice put persons of a protected ground at a particular disadvantage compared with other persons" (Zliobaite 2015).

The two main types of discrimination are often referred to by other terms under different contexts. Direct discrimination is also known as _systematic discrimination_ or _disparate treatment_, and indirect discrimination is also known as _structural discrimination_ or _disparate outcome_. The disparate treatment/outcome terminology is often used in legal settings (e.g., Barocas and Selbst 2016).

More operational definitions of fairness are available for specific machine learning tasks. Consider a binary classification task. The outcome/label represent an important (binary) decision ($pos$ and $neg$). One of the features is protected (e.g., gender, race), and it separates the population into several non-overlapping groups (e.g., $GroupA$ and $GroupB$). The classifier estimates the probability that a given instance belongs to $pos$ based on its features. 


## Relationship among Different Fairness Definitions


## Relationship between Fairness and Predictive Performance


# Addressing Algorithmic Bias

## Measurement


## Detection


## Prevention/Mitigation



# References

Calders, T., & Verwer, S. (2010). Three naive Bayes approaches for discrimination-free classification. _Data Mining and Knowledge Discovery_, 21(2), 277–292.

Kleinberg, J., Mullainathan, S., & Raghavan, M. (2016). Inherent Trade-Offs in the Fair Determination of Risk Scores. [arXiv](https://arxiv.org/pdf/1609.05807.pdf)

Pleiss, G., Raghavan, M., Wu, F., Kleinberg, J., & Weinberger, K. Q. (2017). On Fairness and Calibration. [arXiv](http://arxiv.org/abs/1709.02012)

Kleinberg, J., Ludwig, J., Mullainathan, S., & Rambachan, A. (2018). Algorithmic Fairness. _AEA Papers and Proceedings_, 108, 22–27.

Kleinberg, J., & Raghavan, M. (2018). Selection Problems in the Presence of Implicit Bias. [arXiv](https://arxiv.org/pdf/1801.03533.pdf)

Kleinberg, J., Lakkaraju, H., Leskovec, J., Ludwig, J., & Mullainathan, S. (2017). Human decisions and machine predictions. _The quarterly journal of economics_, 133(1), 237-293.

Calders, T., Karim, A., Kamiran, F., Ali, W., & Zhang, X. (2013). Controlling attribute effect in linear regression. Proceedings - IEEE International Conference on Data Mining, _ICDM_, (1), 71–80.

Custers, B. (2013). Discrimination and Privacy in the information society. Discrimination and Privacy in the Information Society (Vol. 3).

Zliobaite, I. (2015). A survey on measuring indirect discrimination in machine learning. [arXiv](https://arxiv.org/pdf/1511.00148.pdf)

Barocas, S., & Selbst, A. (2016). Big Data's Disparate Impact. _California Law Review_, 104(1), 671–729. 

Romei, A., & Ruggieri, S. (2013). A multidisciplinary survey on discrimination analysis. _Knowledge Engineering Review_, 29(5), 582–638.


# Other Resources

[Emergence of Intelligent Machines](https://www.youtube.com/channel/UC6f1sYAoj6_sxF-lhroS6SQ): a series of talks on algorithmic fairness, biases, interpretability, etc.
