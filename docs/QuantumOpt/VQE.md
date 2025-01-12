# Variational Quantum Eigensolver (VQE)
The VQE can cover more general settings of the optimization problems sucha as chemistry and physis.

The Variational Quantum Eigensolver (VQE), which can be seen as a generalization of the Quantum Approximate Optimization Algorithm of Quantum Approximate Optimization (QAOA). Or we can said that the QAOA algorithm is a special case of the VQE.

## Hamiltonians, observables, and their expectation values 
### Hamiltonians 

From QUBO, we know that we can start with a function $f$ that assigns real numbers to binary strings of a certain length $n$, and we try to find a binary string $x$ with minimum cost $f(x)$. To perform this algorithm, we then define a Hamiltonian $H_{f}$ such that 

$$
\langle x|H|x \rangle = f(x)
$$

holds for every binary string $x$ of length $x$. Then, we can solve the optimal value by finding a ground state of $H_{f}$. That is, a state $\lvert \psi \rangle$ such that the expectation value $\langle x|H_{f}| x \rangle = f(x)$ is minimum.

Then we introduce a new property of the Hamiltonian, For every computational basis state $\lvert x \rangle$, it holds that 

$$
H_{f}\lvert x \rangle = f(x)\lvert x \rangle.
$$

This means that each $\lvert x \rangle$ is an **eigenvector** of $H_{f}$ with associated **eigenvalue**. Becasue:

1. We use Hamiltonians that are sums of tensor products of $Z$ matrices themselves.
2. The sum of diagonal matrices are still diagonal
3. Since these Hamiltonians are diagaonial, the computational basis states are their eignevalues.

From the linear algebra, if we have a state $\lvert \psi \rangle$, we can always write it as a linear combination of the computational basis states such that 

$$
\lvert \psi \rangle = \sum_{x} a_{x} \lvert x \rangle
$$

where the sum is over all the computational basis states $\lvert x \rangle$ and $a_{x} = \langle x | \psi \rangle$, since 

$$
\langle x | \psi \rangle = \langle | \sum_{y} a_{y} \lvert y \rangle = \sum_{y} a_{y} \langle y | \psi \rangle = \sum_{y} a_{y} \langle x | y \rangle = a_{x}.
$$

$\sum_{y} a_{y} \langle x | y \rangle = a_{x}$ is because the fact that $\langle x | y \rangle = 1$  if $x = y$ and 0 otherwise. (The computational basis is an [orthonormal basis](../Math_Fundamentals/matrices.md#orthonormal-basis)).

Thus, the expectation value of $H_{f}$ in the state $\lvert \psi \rangle$ can be computed as 

$$
\begin{array}{lll}
\langle x | H_{f} | x \rangle & = & \sum_{y} a^{*}_{y} \langle y | H_{f} \sum_{x} a_{x} | x \rangle\\
 & = & \sum_{x,y} a^{*}_{y} a_{x} \langle y | H_{f} | x \rangle\\
 & = & \sum_{x,y} a^{*}_{y} a_{x} f(x) \langle y | x \rangle \\
 & = & \sum_{x} a_{x}^{*} a_{x} f(x) = \sum_{x} |a_x|^{2} f(x).
\end{array}
$$

We also know that $|a_x|^{2} = |\langle x | \psi \rangle|^{2}$ is the probability of obtaining $|x\rangle$ when measuring $\lvert \psi \rangle$ in the computational basis. That is, {==The expectation value matches the statistical expected value of the measurement==}.

### Observables
In quantum mechanics, any physical magnitude that you can measure is represented by a Hermitian operator. These are linear operators $A$ that are equal to their adjoints (their [conjugate transposes](../Math_Fundamentals/matrices.md/#conjugate-transpose)) $A^{\dagger} = A$.

The nice thing about Hermitian operators is that one can always {==find an [orthonormal basis](../Math_Fundamentals/matrices.md#orthonormal-basis) of eigenvectors with real eigenvalues==}. This means that there exist real numbers $\lambda_{j}$, $j = 1, \cdots, l,$ all of them different, and states $\lvert \lambda_{j}^{k} \rangle$, where $j = 1, \cdots, l,$ and $k = 1, \cdots, r_{j}$ such that the state $\{\lvert \lambda_{j}^{k}\rangle \}_{j,k} $ form an [orthonormal basis](../Math_Fundamentals/matrices.md#orthonormal-basis) and 

$$
A \lvert \lambda_{j}^{k} \rangle = \lambda_{j} \lvert \lambda_{j}^{k} \rangle,
$$

for every $j = 1, \cdots, l$ and for every $k = 1, \cdots, r_{j}$.

Here, 

1. We are considering the possiblility of having several eigenvectors $\lvert \lambda_{j}^{k} \rangle$ associated with the same eigenvalue $\lambda_{j}$, hence the use of the superindices $k = 1, \cdots, r_{j}$, where $r_{j}$ is the number of eigenvectors associated with the $\lambda_{j}^{k}$ eigenvalue. 
2. If all the eigenvalues are different, then we will have $r_{j} = 1$ for every $j$ and we can drop the $k$ superindices.

Let's consider tan obervable represented by a Hermitian operator $A$, and also an [orthonormal basis](../Math_Fundamentals/matrices.md#orthonormal-basis) of eigenvectors $\{ \lvert \lambda_{j}^{k} \}_{j,k}$ such that $A \lvert \lambda_{j}^{k} \rangle = \lambda_{j} \lvert \lambda_{j}^{k} \rangle$. Plus, the possible outcomes of the measurement of the observable must be represented by the different eigenvalues $\lambda_{j}$. More, upon measurement, the probability that a state $\lvert \psi \rangle$ will yield $\lambda_{j}$ must be $\sum_{k}|\langle \lambda_{j}^{k}|\psi\rangle |^{2}$.

It is a fact that any physical observable can be represented by a Hermitian operator that those are requirements are met.

!!! note
    {==Remember, in quantum mehcanics, if the measurement returns the result associated to an eigenvalue $\lambda_{j}$, the state of the system will then become the normalized projection of $\| \psi \rangle$ onto the space of eigenvectors with eigenvalue $\lambda_{j}$.==}

That is, if we measure a state in a superposition such as 

$$
\sum_{j,k} a_{j}^{k} \lvert \lambda_{j}^{k} \rangle
$$

and we obtain $\lambda_{j}$ as the result, then the new state will be

$$
\frac{\sum_{j,k} a_{j}^{k} \lvert \lambda_{j}^{k} \rangle}{\sqrt{\sum_{j,k} |a_{j}^{k}|^{2}}}.
$$

This is the {==***collapse***==} of the original state! For instance, whenever we measure in the computational basis, we are indeed measuring some physical observable, and this physical observable can be represented by a Hermitian operator.

The coordinated matrix of this measurement operator with respect to the computational basis could be the diagonal matrix 

$$
\begin{pmatrix}
0 & & & \\
& 1 & & \\
& & \ddots & \\
& & & 2^n - 1
\end{pmatrix}
$$

when we measure a single qubit in the computational basis, the coordinate matrix with respect to the computational basis of the associated hermitian operator could be either of

$$
\begin{array}{cc}
    N = 
    \begin{pmatrix}
    0 & 0 \\
    0 & 1
    \end{pmatrix}
    ,&
    Z = 
    \begin{pmatrix}
    1 & 0 \\
    0 & -1
    \end{pmatrix}
    .
\end{array}
$$

Both of these operators represent the same observable; they only differ in the eigenvalues that they associate to the distinct possible outcomes. 

1. N operator: eigenvalues 0 and 1 to the qubit's value being 0 and 1 respectively.
2. Z operator: eigenvalues 1 and -1 to these outcomes.

!!! Note
    {==Measurements in quantum mechanics are represented by Hermitian operators, which we refer to as observable.==} One possible operator corresponding to measuring a qubit in the computational basis can be the Pauli Z matrix.

### Expectation values 
Let's see what **expectation value** is and how it can be computed. 

The expectation value of any observable under a state $\lvert \psi \rangle$ can be defined as 

$$
\langle A \rangle_{\psi} = \sum_{j,k} |\langle \lambda_{j}^{k}|\psi \rangle|^{2} \lambda_{j}, 
$$

which is a natural definition that agrees with the statistical expected value of the results obtained when we measure $\lvert \psi \rangle$ according to $A$. Therefore, we can further simplify it as follows:

$$
\begin{array}{lllll}
\langle A \rangle_{\psi} & = & \sum_{j,k}|\langle \lambda_{j}^{k} | \psi \rangle |^{2} \lambda_{j} & = & \sum_{j,k} \langle \psi | \lambda_ {j}^{k} \rangle \langle \lambda_ {j}^{k} | \psi \rangle \lambda_{j}\\
 & = & \sum_{j,k} \langle \lambda_ {j}^{k} | \psi  \rangle \langle \psi | \lambda_ {j}^{k} \rangle \lambda_{j} & = &\sum_{j,k} \langle \lambda_ {j}^{k} | \psi \rangle \langle \psi | A | \lambda_ {j}^{k} \rangle\\
 & = & \langle \psi | A \sum_{j,k} \rangle \lambda_{j}^{k} | \psi \rangle | \lambda_{j}^{k} \rangle & = & \langle \psi | A | \psi \rangle
\end{array}
$$

Let's look into these steps by steps! Let's start with the first row.

For the first term

$$
\langle A \rangle_{\psi} = \sum_{j,k} \left| \langle \lambda_{j}^{k} | \psi \rangle \right|^2 \lambda_{j}.
$$

- $\langle \lambda_{j}^{k} | \psi \rangle$: Projection (Inner product!) of the state $|\psi\rangle$ onto the eigenstate $|\lambda_{j}^{k}\rangle$.
- $\left| \langle \lambda_{j}^{k} | \psi \rangle \right|^2$: Probability of obtaining the eigenvalue $\lambda_{j}$ upon measurement.
- $\lambda_{j}$: Measurable value (eigenvalue) associated with the eigenstate.

!!! note 
    This form represents the expectation value as a sum over the probabilities of each measurement outcome multiplied by the corresponding eigenvalue.

The second term

$$
\langle A \rangle_{\psi} = \sum_{j,k} \langle \psi | \lambda_{j}^{k} \rangle \langle \lambda_{j}^{k} | \psi \rangle \lambda_{}.
$$

- The probabilities are expressed in terms of the inner products $\langle \psi | \lambda_{j}^{k} \rangle$ and $\langle \lambda_{j}^{k} | \psi \rangle$, showing explicitly how the state $|\psi\rangle$ interacts with the eigenbasis $|\lambda_{j}^{k}\rangle$.

- The eigenstates $|\lambda_{j}^{k}\rangle$ form an [orthonormal basis](../Math_Fundamentals/matrices.md#orthonormal-basis), so the squared magnitude $\left| \langle \lambda_{j}^{k} | \psi \rangle \right|^2$ is equivalent to the product $\langle \psi | \lambda_{j}^{k} \rangle \langle \lambda_{j}^{k} | \psi \rangle$.

The equation shows how the expectation value of a Hermitian operator (observable) is calculated by summing over all possible eigenvalues $\lambda_{j}$, weighted by the probability of measuring $\lambda_{j}$ when the system is in state $|\psi\rangle$. The expectation value $\langle A \rangle_{\psi}$ is the weighted average of the possible outcomes $\lambda_{j}$, with the probabilities $\left| \langle \lambda_{j}^{k} | \psi \rangle \right|^2$ serving as weights.

Then, we moved on to the second row.

These equations show how the **expectation value** of a Hermitian operator $A$ in a quantum state $|\psi\rangle$ is derived, step by step. Let’s break it down:

---

### First Step:
$$
\sum_{j,k} \langle \psi | \lambda_j^k \rangle \langle \lambda_j^k | \psi \rangle \lambda_j
= \sum_{j,k} \langle \lambda_j^k | \psi \rangle \langle \psi | \lambda_j^k \rangle \lambda_j
$$
- This step shows that the ordering of inner products doesn't matter due to the properties of complex conjugation.

---

### Second Step:
$$
\sum_{j,k} \langle \lambda_j^k | \psi \rangle \langle \psi | A | \lambda_j^k \rangle
= \langle \psi | A \sum_{j,k} | \lambda_j^k \rangle \langle \lambda_j^k | \psi \rangle
$$
- Here, $\lambda_j | \lambda_j^k \rangle = A | \lambda_j^k \rangle$, because $| \lambda_j^k \rangle$ is an eigenstate of $A$ with eigenvalue $\lambda_j$.
- The summation $\sum_{j,k} | \lambda_j^k \rangle \langle \lambda_j^k |$ represents the **resolution of identity**, which means that any state can be expressed in this eigenbasis.

---

### Final Step:
$$
\langle \psi | A | \psi \rangle
$$
- This simplifies the expectation value entirely into the form $\langle \psi | A | \psi \rangle$, which is the standard expression for the expectation value of a Hermitian operator $A$ in the state $|\psi\rangle$.

---

### Physical Interpretation:
- The derivation connects the abstract summation over eigenvalues and eigenstates to the compact, widely-used notation $\langle \psi | A | \psi \rangle$.
- The expectation value $\langle \psi | A | \psi \rangle$ represents the average value of many measurements of the observable $A$ when the system is in state $|\psi\rangle$.

---

Notice that we have used the fact that $A|\lambda_{j}^{k}\rangle = \lambda_{j}|\lambda_{j}^{k} \rangle$ and that $|\psi \rangle = \sum_{j,k} \rangle \lambda_{j}^{k} | \psi \rangle | \lambda_{j}^{k} \rangle$.

---

!!! Note 
    The expectation value of any Hermitian operator (observable) $A$ is given by
    
    $$
    \langle A \rangle_{\psi} = \sum_{j,k}|\langle \lambda_{j}^{k} | \psi \rangle |^{2} \lambda_{j} = \langle \psi | A | \psi \rangle.
    $$



The {==variational principle==} states that the smallest expectation value of an observable $A$ is alwyas achieved at an eigenvector of that observable. Suppose that $\lambda_{0}$ is minimal among all the eigenvalues of $A$. Then, for any state $\psi$ it holds that 

$$
\langle A\rangle_\psi = \sum_{j,k}|\langle \lambda_{j}^{k} | \psi \rangle |^{2}\lambda_{j} \geq \sum_{j,k}| \langle \lambda_{j}^{k}| \psi \rangle|^2\lambda_{0} = \lambda_{0}
$$

where the last equality follows from the fact the $\sum_{j,k}| \langle \lambda_{j}^{k}| \psi \rangle|^{2} = 1$, since the sum of the probabilities of all the outcomes must add up to 1. 

If we take any eignevector $|\lambda_{0}^{k} \rangle$ associated to $\lambda_{0}$, its expectation value will be 

$$
\langle \lambda_{0}^{k} |A|\lambda_{0}^{k}\rangle = \lambda_{0} \langle \lambda_{0}^{k} | \lambda_{0}^{k} \rangle = \lambda_{0}^{k},
$$

proving that the minimum expectation value is achieved at and eigenvector of $A$.

## Estimaing the expectation values of observables
As we know, for a fiven state $|\psi\rangle$, the expectation value of $A$ can be computed by

$$
\sum_{j,k}|\langle \lambda_{j}^{k}|\psi\rangle|^{2}\lambda_{j}.
$$

That is, if we know the eigenvalues $\lambda_{j}$ and the eigenvectors $\{|\lambda_{j}^{k}\rangle \}_{j,k}$ of $A$, we could try to compute $\sum_{j,k}|\langle \lambda_{j}^{k}|\psi\rangle|^{2}$ and hence, the expectation value of $A$. However, these values are generally hard to find. In fact, the purpose of VQE is finding certain eigenvalues and eigenvectors of a Hamiltonian.

To find these eigenvalues and eigenvectors, we will use the fact that we can always express and observable $A$ on $n$ qubits as a linear combination of tensor products of Pauli matrices. Let's consider an observable 

!!! note 
    $A$ will be given to us in such form, in most cases, in the same way that the Hamiltonians of our combinatorial optimization problems were always expressed as sums of tensor products of $Z$ matrices.

$$
A = \frac{1}{2}Z\otimes I\otimes X -3I\otimes Y \otimes Y + 2Z\otimes X \otimes Z.
$$

From the linearity,

$$
\begin{array}{lll}
\langle \psi |A|\psi\rangle & = & \langle\psi|\big( \frac{1}{2}Z\otimes I\otimes X -3I\otimes Y \otimes Y + 2Z\otimes X \otimes Z \big)|\psi\rangle\\
& = & \frac{1}{2}\langle\psi|(Z\otimes I\otimes X)|\psi\rangle -3\langle \psi|I\otimes Y \otimes Y|\psi\rangle + 2\langle \psi|(Z\otimes X \otimes Z)\psi\rangle.
\end{array}
$$

To compute the expectation value of $A$, we can compute the expectation values of $Z\otimes I\otimes X$, $I\otimes Y \otimes Y$, and $2Z\otimes X \otimes Z$ as combine their results.

!!! Practice "Exercise 1"
    Suppoer that $|\lambda_{j}\rangle$ is an eigenvector of $A_{j}$ with associated eigenvalue $\lambda_{j}$ for $j = 1, \cdots,n.$ Prove that $|\lambda_{1}\rangle\otimes\cdots\otimes|\lambda_{n}\rangle$ is an eigenvector of $A_{1}\otimes\cdots\otimes A_{n}$ with associated eigenvalue $\lambda_{1} \cdot \lambda_{n}$.

??? Answer "Answer exercise 1"
    Answer 1

!!! Practice "Exercise 2"
    Prove that:

    1.  The eigenvectors of $Z$ are $|0\rangle$ (with assocaiated eignevalue 1) and $|1\rangle$ (with assocaiated eignevalue -1). 
    2.  The eigenvectors of $Z$ are $|+\rangle$ (with assocaiated eignevalue 1) and $|-\rangle$ (with assocaiated eignevalue -1). 
    3.  The eigenvectors of $Z$ are $\frac{1}{2}(|0\rangle+i|1\rangle)$ (with assocaiated eignevalue 1) and $\frac{1}{2}(|0\rangle-i|1\rangle)$ (with assocaiated eignevalue -1).
    4. Any non-null state is an eigenvector of $I$ with associated eigenvalue 1.

??? Answer "Answer exercise 2"
    Answer 2

Using the results from theses exercises, we can find that 

1.  $|0\rangle|+\rangle|0\rangle$, $|0\rangle|-\rangle|1\rangle$, $|1\rangle|+\rangle|1\rangle$, and $|1\rangle|-\rangle|0\rangle$ are eigenvectors of $Z\otimes X\otimes Z$ with eigenvalue of $1$.
2.  $|0\rangle|+\rangle|1\rangle$, $|0\rangle|-\rangle|0\rangle$, $|1\rangle|+\rangle|0\rangle$, and $|1\rangle|-\rangle|1\rangle$ are eigenvectors of $Z\otimes X\otimes Z$ with eigenvalue of $-1$.

All these states together form an orthonormal basis of eigenvectors of $Z\otimes X\otimes Z$.

!!! Practice "Exercise 3"
    Find orthonormal bases of eigenvectors for $Z\otimes I \otimes X$ and $I\otimes Y \otimes Y$. Compute their associated eigenvalues.

??? Answer "Answer exercise 3"
    Answer 3

For a given Hermitian matrix $A$, we can compute their expectation value $\langle \psi|A|\psi\rangle$ by

$$
\sum_{j,k}|\langle \lambda_{j}^{k}|\psi\rangle|^{2}\color{red}{\lambda_{j}}.
$$

where their $eigenvalues of $A$ are $\color{red}{\lambda_{j}}$ and the associated eigenvectors are $\{|\lambda_{j}^{k} \}_{j,k}$. In our case, we only have eigenvalues 1 and -1.

Let's see how we can compute $|\langle \lambda_{j}^{k}|\psi\rangle|^{2}$. By giving an observable $Z\otimes X\otimes Z$, we can use the technique we've just practiced to get the eigenvalues and eigenvectors of any tensor product of Pauli matrices. Now, we focus on one of them: $|0\rangle|+\rangle|0\rangle$. If we want to compute $|(\langle 0 |\langle+|\langle0|)|\psi\rangle|^{2}$, where $|\psi\rangle$ is a 3-qubit state

$$
|0\rangle|+\rangle|0\rangle = (I\otimes H\otimes I)0\rangle|0\rangle|0\rangle
$$

and hence

$$
\begin{array}{lll}
\color{green}{\langle 0 |\langle+|\langle0|} & = & (|0\rangle|+\rangle|0\rangle)^{\dagger}\\
& = & ((I\otimes H\otimes I)|0\rangle|+\rangle|0\rangle)^{\dagger}\\
& = & \color{blue}{\langle 0 |\langle+|\langle0|(I\otimes H\otimes I)^{\dagger}}\\
& = & \langle 0 |\langle+|\langle0|(I\otimes H\otimes I)
\end{array}
$$

as we already know that $I^{\dagger} = I$ and $H^{\dagger} = H$. Therefore,

$$
|\color{green}{(\langle 0 |\langle+|\langle0|)}|\psi\rangle|^{2} = |\color{blue}{\langle 0 |\langle+|\langle0|(I\otimes H\otimes I)^{\dagger}}|\psi\rangle|^{2}.
$$

-   The equation ${(\langle 0 |\langle+|\langle0|)}|\psi\rangle|^{2}$ represents the probability of obtaining the state $|0\rangle|0\rangle|0\rangle$ (all qubits in the "0" state) when measuring the state $|\psi\rangle$ in the computational basis.
-   Quantum mechanics is probabilistic, so each measurement provides one outcome, which may not immediately reflect the actual probability. Repeating the process many times and recording how often $|0\rangle|0\rangle|0\rangle$ is observed allows you to calculate its relative frequency, which approximates the probability ${(\langle 0 |\langle+|\langle0|)}|\psi\rangle|^{2}$.

In fact, this is not only eigenvector for which this works. It turns out that for {==each and every eigenvector $|\lambda_{A}\rangle$==} of $Z\otimes X\otimes Z$, there is a unique state in the computational basis $|\lambda_{C}\rangle$ such that 

$$
|\lambda_{A}\rangle = (I\otimes H \otimes I)|\lambda_{C}\rangle.
$$




## Intoducing Variational Quantum Eigensolver (VQE)
{==The goal of the **Variational Quantum Eigensolver (VQE)** is to find a ground state of a given Hamiltonian $H_1$.==} This Hamiltonian can describe, for instance, the energy of a certain physical or chemical process. We first focus on finding a state $|\psi\rangle$ such that $\langle \psi|H_1|\psi\rangle$ is minimum. In this sub-section, we will use $H_1$ to refer to the Hamiltonian.

The general structure of VQE is very similar to that of QAOA. 

``` mermaid
flowchart TD
    A[Parepare a parameterized quantum state. ***Done by quantum computer***] --> B[Measurement. ***Done by quantum computer***];
    B --> C[Energy estimation. ***Handle by classical computer***];
    C --> F[Minimization. ***Handle by classical computer***];
    F --> E{Minimum energy state reached?}
    E --> |No| D[Change parameters];
    D --> A;
    E --> |Yes| G[Minimum energy state found!]
```

The parameterized circuit is called {==ansatz==} or {==variational form==}, is usually chosen taking into account information from the problem domain. 

!!! Note
    Think about {==ansatz==} that parameterize typical solutions to the kind of problem under study. The ansatz is selected inadvance and it is usually easy to implement on a quantum circuit.

!!! Note 
    In many applications, we distinguish two parts in the creation of the parameterized state:
    -   The preparation of an initial state $|\psi_{0} \rangle$, this is independent on any parameter.
    -   The variational form $V(\theta)$ itself, which depends on $\theta$.
    If we have $|\psi_{0} \rangle = U|0\rangle$, for some unitary transformation $U$ implemented with some quantum gates, the ansatz gives us the state $V(\theta)U|\psi\rangle$. We usually denote $V(\theta)U$ as the ansatz and require the initial state to be $|0\rangle$ to simplify our notation.

---
***Algorithm VQE***

**Require:** $H_1$: given as a linear combination of tensor products of Pauli matrices  

-   Choose a ansatz (variational form) $V(\theta)$  
-   Choose a starting set of values for $\theta$ (initial values)

**While** the stopping criteria are not met **do**:

-   Prepare the state $|\psi(\theta)\rangle = V(\theta)|0\rangle$   {==*This is done on the quantum computer!*==}
-   From the measurements of $|\psi(\theta)\rangle$ in different bases, estimate $\langle\psi(\theta)|H_{1}|\psi(\theta)\rangle$
-   Update $\theta$ according to the minimization algorithm

**End While**

-   Parepare the sate $|\psi(\theta)\rangle = V(\theta)|0\rangle$   {==*This is done on the quantum computer!*==}
-   From the measurement of $|\psi(\theta)\rangle$ in different bases, estimate $\langle\psi(\theta)|H_{1}|\psi(\theta)\rangle$

---

Notice that:

1.  We require that $H_{1}$ be given as a linear combination of tensor products of Pauli matrices since we can use the technique - change of basis operator - that we introduced before to estimate $\langle \psi|H_{1}|\psi \rangle$. 
2.  The more terms we have in the linear combination, the bigger the number of bases in which we need to perform measurements. However, we can group serval measurements together. For instance, if we have $I \otimes X \otimes I \otimes X, I \otimes I \otimes X \otimes X$, and $I \otimes X \otimes I \otimes X$, we can use $I \otimes H \otimes H \otimes H$ as our change of basis matrix (H is Hadamard matrix) because it works for the three terms at the same time - that any orthonormal basis is an eigenvector basis of $I$, not just $\{|0\rangle, |1\rangle \}$.
3.  More frequent we measure $|\psi \rangle$ in each basis can lead to a more accurate estimation but increase time needed to estimate $\langle \psi |H_{1}|\psi \rangle$
4.  We usually estimiate $\langle\psi(\theta)|H_{1}|\psi(\theta)\rangle$ for the last state $|\psi(\theta)\rangle$ found by the optimization algorithm.
5. At the end of the VQ execution, you also know the $\theta_{0}$ parameters that were used to build the ground state, and you could use them to reconstruct $|\psi(\theta_{0})\rangle = V(\theta_{0})|0\rangle$. This state can be used to send to another quantum algorithm.

## VQE
VQE is used to search for a ground state of a given Hamiltonian $H$. However, with a small modification, we can also use it to find ***excited states***: eigenstates with higher energies.

Suppose that you have been given a Hamiltonian $H$ and you have used VQE to find a ground state $|\psi_{0}\rangle = V(\theta_{0})|0\rangle$ with energy $\lambda_{0}$. We may consider the modified Hamiltonian

$$
H' = H + C|\psi_{0}\rangle\langle\psi_{0}|,
$$

where $C$ is a ***positive real number***.

-   $|\psi_{0}\rangle\langle\psi_{0}|$: This is a product of a column vector ($|\psi_{0}\rangle$) and a row vector ($\langle\psi_{0}|$) of the same length. This is also a Hermitian matrix, since 

    $$
    (|\psi_{0}\rangle\langle\psi_{0}|)^{\dagger} = \langle\psi_{0}|^{\dagger} |\psi_{0}\rangle^{\dagger} = |\psi_{0}\rangle\langle\psi_{0}|.
    $$

-   $H'$ is a Hermitian since it's the sum of two Hermitian matrics.

If we have a generic quantum state $|\psi\rangle$, then

$$
\langle\psi|H'|\psi\rangle = \langle\psi|H|\psi\rangle + C\langle\psi|\psi_{0}\rangle\langle\psi_{0}|\psi\rangle = \langle\psi|H|\psi\rangle+|\langle\psi_{0}|\psi\rangle|^{2}.
$$

This means that the expectation value of $H'$ in a state $|\psi\rangle$ is the expectation value of $H$ plus a non-negative value that **quantifies the overlap of $|\psi\rangle$ and $|\psi_{0}\rangle$**. Hence we will have two extreme cases.

1.  $|\langle\psi_{0}|\psi\rangle| = C$ if $|\psi\rangle = |\psi_{0}\rangle$
2.  $|\langle\psi_{0}|\psi\rangle| = 0$ if $|\psi\rangle$ and $|\psi_{0}\rangle$ are orthogonal.

Thus, ***if we make $C$ big enough, $|\psi_{0}\rangle$, a ground state by hypothesis, will no longer be a ground state of H'***. Once you obtain $|\lambda_{1}\rangle$ (the first excited state), you can consider $H'' = H' + C|\psi_{1}\rangle\langle\psi_{1}|$ and use VQE to search for $|\lambda_{2}$, and so on and so forth. In this process, we need to pick the constants $C,C',\cdots$ wisely so that none of the eigenstates that we already know becomes a ground state again.

We have discussed how to estimate the expectation value of a Hamiltonian under the assumption that it was given as a sum of tensor products of Pauli matrices. However, $|\psi_{0}\rangle\langle\psi_{0}|$ term is not of that form. In fact, we know $|\psi_{0}\rangle$ only as the result of applying VQE. so we will not know $|\psi_{0}\rangle$ explicitly. Instead, we will have nothing more than some parameters $\theta_{0}$ such that $V(\theta_{0})|0\rangle = |\psi_{0}\rangle$. This is enough to compute the expectation values that we need.

At a given moment in the application of VQE, we have some parameters $\theta$ and we want to estimate the expectation value of $|\psi_{0}\rangle\langle\psi_{0}|$ with respect to $|\psi(\theta)\rangle = V(\theta)|0\rangle$. This quantuty is 

$$
\langle\psi(\theta)|\psi_{0}\rangle\langle\psi_{0}|\psi(\theta)\rangle = |\langle \psi_{0}|\psi(\theta)\rangle|^{2} = |\langle0|V(\theta_{0})^{\dagger}V(\theta)|0\rangle|^{2}.
$$

This is the probability of obtaining $|0\rangle$ as the outcome of measuring $V(\theta_{0})^{\dagger}V(\theta)|0\rangle$ in the computational basis! This is something that we can easily estimate because we can prepare $V(\theta_{0})^{\dagger}V(\theta)|0\rangle$ by first applying our ansatz $V$, using $\theta$ as the parameters, to $|0\rangle$, and then applying the inverse of our ansatz, with parameter $\theta_{0}$, to the resulting state. We will repeat this process several times, always measuring the resulting state $V(\theta_{0})^{\dagger}V(\theta)|0\rangle$ in the computational basis and computing the relative frequency of the outcome $|0\rangle$.

Then we have to deal with preparing the circuit for $V(\theta_{0})^{\dagger}$. All you need to remember that every unitary gate is reversible. Thus, you can take the circuit for $V(\theta)$ and read the gates from right to left, reversing each one of them. For example, if $\theta_{0} = (a,b)$ and $V(\theta_{0}) = XR_{Z}(a)R_{X}(b)S$, then $V(\theta_{0})^{\dagger} = S^{\dagger}R_{x}(-b)R_{z}(-a)X^{\dagger} = S^{\dagger}R_{x}(-b)R_{z}(-a)X$.

## Using VQE with Qiskit


## USing VQE with PennyLane