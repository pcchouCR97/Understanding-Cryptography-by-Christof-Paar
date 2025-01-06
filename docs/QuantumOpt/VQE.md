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

$\sum_{y} a_{y} \langle x | y \rangle = a_{x}$ is because the fact that $\langle x | y \rangle = 1$  if $x = y$ and 0 otherwise. (The computational basis is an orthonormal basis).

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
In quantum mechanics, any physical magnitude that you can measure is represented by a Hermitian operator. These are linear operators $A$ that are equal to their adjoints (their conjugate transposes) $A^{\dagger} = A$.

The nice thing about Hermitian operators is that one can always {==find an orthonormal basis of eigenvectors with real eigenvalues==}. This means that there exist real numbers $\lambda_j$, $j = 1, \cdots, l,$ all of them different, and states $\lvert \lambda_{j}^{k}$, where $j = 1, \cdots, l,$ and $k = 1, \cdots, r_j$ such that the state $\{\lvert \lambda_{j}^{k}\}_{j,k} \rangle$ form an orthonormal basis and 

$$
A \lvert \lambda_{j}^{k} \rangle = \lambda_{j} \lvert \lambda_{j}^{k} \rangle,
$$

for every $j = 1, \cdots, l$ and fpr every $k = 1, \cdots, r_{j}$.

Here, 

1. We are considering the possiblility of having several eigenvectors $\lambda_{j} \lvert \lambda_{j}^{k} \rangle$ associated with the same eigenvalue $\lambda_{j}$, hence the use of the superindices $k = 1, \cdots, r_{j}$, where $r_j$ is the number of eigenvectors associated with the $\lambda_{j}^{k}$ eigenvalue. 
2. If all the eigenvalues are different, then we will have $r_{j} = 1$ for every $j$ and we can drop the $k$ superindices.

### Expectation values 


## Intoducing the Variational Quantum Eigensolver


## Using VQE with Qiskit


## USing VQE with PennyLane