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

Notice that we have used the fact that $A|\lambda_{j}^{k} = \lambda_{j}|\lambda_{j}^{k} \rangle$ and that $|\psi \rangle = \sum_{j,k} \rangle \lambda_{j}^{k} | \psi \rangle | \lambda_{j}^{k} \rangle$.

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


## Intoducing Variational Quantum Eigensolver


## Using VQE with Qiskit


## USing VQE with PennyLane