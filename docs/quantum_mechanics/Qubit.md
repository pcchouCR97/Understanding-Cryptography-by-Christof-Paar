## Dirac notation and inner products
with each ket we can associate a bra that is its adjoint or conjugate transpose or Hermitian transpose.

$$
\begin{array}{cc}
    \langle 0 \lvert = \lvert 0 \rangle^{\dagger} = \bigg(
    \begin{array}{c}
    1 \\ 0
    \end{array}
    \bigg)^{\dagger}
    =
    \bigg(
    \begin{array}{c}
    1 & 0
    \end{array}
    \bigg)
    ,&
    \langle 0 \lvert = \lvert 0 \rangle^{\dagger} = \bigg(
    \begin{array}{c}
    0 \\ 1
    \end{array}
    \bigg)^{\dagger}
    =
    \bigg(
    \begin{array}{c}
    1 & 0
    \end{array}
    \bigg)
\end{array}
$$

and, in gernal,
$$
a\langle 0 \lvert + b\langle 1 \lvert = a\lvert 0 \rangle^{\dagger} + b\lvert 1 \rangle^{\dagger} = a \bigg(\begin{array}{c} 1 & 0 \end{array} \bigg) + b \bigg(\begin{array}{c} 0 & 1 \end{array} \bigg) = \bigg(\begin{array}{c} a & b \end{array} \bigg)
$$
More, we can also compute the following properties:

--inner product properties--

This proves that $\lvert 0 \rangle$ and $\lvert 1 \rangle$ are not just elements of any basis but of an **orthonormal** one, since $\lvert 0 \rangle$ and $\lvert 0 \rangle$ are orthogonal and of length 1. Thus we can calcualte the inner product of two states $\lvert \psi_{1} \rangle = a\lvert 0 \rangle + b\lvert 1 \rangle$ and $\lvert \psi_{2} \rangle = c\lvert 0 \rangle + d\lvert 1 \rangle$ as:

$$
\begin{array}{cll}
\langle \psi_{1}\lvert\psi_{2} \rangle^{\dagger} & = & (a^{*}\langle0\lvert+b^{*}\langle1\lvert)(c^{*}\lvert0\rangle+d^{*}\lvert1\rangle)\\
 & = & a^{*}c\langle0\lvert0\rangle+a^{*}d\langle0\lvert1\rangle+b^{*}c\langle1\lvert0\rangle+b^{*}d\langle1\lvert1\rangle\\
 & = & a^{*}b+c^{*}d
\end{array}
$$

where $a^{*}$ and $b^{*}$ are the complex cpmjugates of $a$ and $b$

## One-Qubit Quantum gates
### **The Schrodinger equation**
The time-dependent Schrödinger equation is given by:

$$
\hat{H} \lvert\psi(t)\rangle = i \hbar \frac{\partial \lvert\psi(t)\rangle}{\partial t}
$$

where:

1. $\hbar$ is the reduced Planck's constant,
2. $\lvert\psi(t)\rangle = \psi(x,t)$ is the wave function,
3. $\hat{H}$ is the Hamiltonian operator.

To program a quantum computer, you don’t need to know how to solve Schrödinger’s equation. In fact, the only thing that you need to know is that its solutions are always a special type of linear transformations. For the purposes of the quantum circuit model, since we are working in finite-dimensional spaces and we have fixed a basis, the operations can be described by matrices that are applied to the vectors that represent the states of the qubits.



























---


Ref: A Practical Guide to Quantum Machine Learning and Quantum Optimization_ Hands-on Approach to Modern Quantum Algorithms