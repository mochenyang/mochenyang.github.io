---
layout: post
title:  "Notes: Efficient Computational Strategies for Dynamic Inventory Liquidation"
date:   2019-07-08
---

Our paper, _[Efficient Computational Strategies for Dynamic Inventory Liquidation](https://pubsonline.informs.org/doi/abs/10.1287/isre.2018.0819)_, recently came out in the June issue of _ISR_. We develop a computationally efficient strategy to support the pricing decisions in large-scale inventory liquidation problems. Although the paper is positioned in a revenue management context, the theoretical results are in fact broadly relevant to other problems as well. The following notes describe a generalized version of our main results (which underpins the heuristic pricing approach).

Consider a real-valued matrix \\( P = (p\_{ij})\_{B \times D} \\), where the values each column are ranked in descending order, i.e., \\( \forall j \in \\{1,\ldots, D\\}, p\_{ij} \geq p\_{(i+1)j} \\). In the specific context of inventory liquidation, \\( p\_{ij}\\) represents the \\(i\\)-th highest willingness-to-pay of consumers arriving on pricing period \\(j\\). Given a constant \\(N\\), e.g., representing the total number of items to be liquidated, a feasible allocation of \\(N\\) over \\(P\\) can be denoted as a vector \\( (n\_1, \ldots, n\_D) \\), where \\(0 \leq n\_j \leq B\\) and \\(\sum\_{j=1}^{D} n\_j \leq N \\). The "value" (e.g., revenue) associated with such an allocation is 

$$
V\left(P, (n\_1, \ldots, n\_D) \right) = \sum\_{j=1}^{D} n\_j \times p\_{n\_j j}
$$

We are often concerned with maximizing the value with respect to all feasible allocations (e.g., finding the best pricing strategy to maximizing liquidation revenue). The maximization problem is formulated as 

$$
\max\_{(n\_1, \ldots, n\_D)} V\left(P, (n\_1, \ldots, n\_D) \right)
$$

This can be solved by standard dynamic programming. However, under a realistic condition on \\(P\\), it can be solved even faster with a heuristic approach -- this is the main contribution of our paper. First, construct a "differencing" matrix of \\(P\\) as \\(\Delta P = (\Delta p\_{ij})\_{B \times D} \\), where \\( \Delta p\_{ij} = i \times p\_{ij} - (i-1) \times p\_{(i-1)j} \\). In an inventory liquidation problem, \\(\Delta p\_{ij} \\) represents the marginal revenue of selling \\(i\\) items over selling \\(i-1\\) items on pricing period \\(j\\). We show that if \\(\forall j \in \\{1,\ldots, D\\}\\), \\( \Delta p\_{ij} \\) is non-increasing with respect to \\(i\\), then the maximization problem can be solved by simply finding the \\(N\\) largest non-negative elements in \\(\Delta P\\). A proof by induction of this statement is provided in our paper [appendix](https://pubsonline.informs.org/doi/suppl/10.1287/isre.2018.0819/suppl_file/isre.2018.0819.sm1.pdf).

Interestingly, the proof does not rely on each column of the matrix having equal length. In other words, the results can be generalized to a collection of real-valued vectors \\( \mathbb{P} = (P\_1, \ldots, P\_D) \\) where \\( P\_j = (p\_{1j}, \ldots, p\_{B\_j j}) \\), which makes the results even more general.