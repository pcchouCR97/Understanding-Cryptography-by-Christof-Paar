# Quadratic unconstrained Binary Optimization Problems

## The Max-Cut problem and the Ising model
That is, it is a partition of the graph's vertices into two complementary sets S and T, such that the number of edges between S and T is as large as possible. Finding such a cut is known as the max-cut problem.

![MaxCut](../QuantumOpt/images/MaxCut.png)

### Problem formulation

![MaxCut2](../QuantumOpt/images/MaxCut2.png)

We can formulate the above graph as the follwoing:

$$
\begin{array}{ll}
    \text{Minimize} & z_{0}z_{1}+z_{0}z_{2}+z_{1}z_{0}+z_{1}z_{2}+z_{1}z_{4}+z_{2}z_{0}+z_{2}z_{1}+z_{2}z_{3}\\
     & +z_{3}z_{4}+z_{3}z_{5}+z_{4}z_{5}\\
    \text{subject to} & z_{j} \in {-1,1}, j = 0, \cdots,4.
\end{array}
$$

## The Ising model

As you can easily check that if $J_{jk}$ coefficients are $-1$ and all the $h_j$ coefficients are $0$, the problem is exactly the same as the max-cut problem.