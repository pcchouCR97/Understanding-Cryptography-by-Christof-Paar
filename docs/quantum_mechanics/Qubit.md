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

## Two qubits and entanglement
### Two-qubit state

For a two-qubit state system, we have four possibilities form a basis(**computational basis**) of a 4-dimensional space,

$$
\begin{array}{cccc}
\lvert0\rangle \otimes \lvert0\rangle,& \lvert0\rangle \otimes \lvert1\rangle,& \lvert1\rangle \otimes \lvert0\rangle,& \lvert1\rangle \otimes \lvert1\rangle
\end{array}
$$
 
The symbol $\otimes$ is a [**tensor product**](../Math_Fundamentals/linear_algebr_tensor.md). The tensor product of two column vectors is defined by

### Tensor Product

The tensor product of two vectors is defined as:

$$
\begin{pmatrix}
a_1 \\
a_2 \\
\vdots \\
a_n
\end{pmatrix}
\otimes
\begin{pmatrix}
b_1 \\
b_2 \\
\vdots \\
b_m
\end{pmatrix}
=
\begin{pmatrix}
a_1
\begin{pmatrix}
b_1 \\
b_2 \\
\vdots \\
b_m
\end{pmatrix} \\
a_2
\begin{pmatrix}
b_1 \\
b_2 \\
\vdots \\
b_m
\end{pmatrix} \\
\vdots \\
a_n
\begin{pmatrix}
b_1 \\
b_2 \\
\vdots \\
b_m
\end{pmatrix}
\end{pmatrix}
=
\begin{pmatrix}
a_1 b_1 \\
a_1 b_2 \\
\vdots \\
a_1 b_m \\
a_2 b_1 \\
a_2 b_2 \\
\vdots \\
a_2 b_m \\
\vdots \\
a_n b_1 \\
a_n b_2 \\
\vdots \\
a_n b_m
\end{pmatrix}
$$

Therefore, we expand four states

$$
\begin{array}{cccc}
\lvert0\rangle \otimes \lvert0\rangle = \begin{pmatrix} 1 \\ 0 \\ 0 \\ 0 \end{pmatrix}, & 
\lvert0\rangle \otimes \lvert1\rangle = \begin{pmatrix} 0 \\ 1 \\ 0 \\ 0 \end{pmatrix}, & 
\lvert1\rangle \otimes \lvert0\rangle = \begin{pmatrix} 0 \\ 0 \\ 1 \\ 0 \end{pmatrix}, & 
\lvert1\rangle \otimes \lvert1\rangle = \begin{pmatrix} 0 \\ 0 \\ 0 \\ 1 \end{pmatrix} 
\end{array}
$$

We usually omit the $\otimes$ and just write,

$$
\begin{array}{cccc}
\lvert00\rangle, &
\lvert01\rangle, &
\lvert10\rangle, &
\lvert11\rangle &
\end{array}
$$

The general expression for the state of such system is 

$$
\lvert\psi\rangle = a_{00}\lvert00\rangle+a_{01}\lvert01\rangle+a_{10}\lvert10\rangle+a_{11}\lvert11\rangle
$$

where $a_{00}, a_{01}, a_{10}, a_{11}$ are complex numbers or amplitudes such that $\sum_{x, y=0}^{1}|a_{xy}|^{2} = 1$. If we measure in the computational basis both qubits at this generic state that we are considering, we will obtain $\gamma$ with probability $|a_{\gamma}|^{2}$ where $\gamma \in [00, 01, 10, 11]$.

### What if we only measure one computational basis 0? 

The result will show that the system is not collapse completely, but it will remain in the state

$$
\frac{a_{00}\lvert00\rangle+a_{01}\lvert01\rangle}{\sqrt{|a_{00}|^{2} + |a_{01}|^{2}}}
$$

Also, recall the inner product of the one qubit case, we can apply inner product to 

$$
(\langle\psi_{2}\lvert\otimes\langle\psi_{2}\lvert)(\lvert\phi_{1}\rangle\otimes\lvert\phi_{2}\rangle) = \langle\phi_{1}\lvert\phi_{1}\rangle\langle\phi_{2}\lvert\phi_{2}\rangle
$$

### Two-qubit gate: tensor products

The tensor product of two matrices is defined as:

$$
\begin{pmatrix}
a_{11} & a_{12} \\
a_{21} & a_{22}
\end{pmatrix}
\otimes
\begin{pmatrix}
b_{11} & b_{12} \\
b_{21} & b_{22}
\end{pmatrix}
=
\begin{pmatrix}
a_{11}
\begin{pmatrix}
b_{11} & b_{12} \\
b_{21} & b_{22}
\end{pmatrix} &
a_{12}
\begin{pmatrix}
b_{11} & b_{12} \\
b_{21} & b_{22}
\end{pmatrix} \\
a_{21}
\begin{pmatrix}
b_{11} & b_{12} \\
b_{21} & b_{22}
\end{pmatrix} &
a_{22}
\begin{pmatrix}
b_{11} & b_{12} \\
b_{21} & b_{22}
\end{pmatrix}
\end{pmatrix}
$$

$$
=
\begin{pmatrix}
a_{11} b_{11} & a_{11} b_{12} & a_{12} b_{11} & a_{12} b_{12} \\
a_{11} b_{21} & a_{11} b_{22} & a_{12} b_{21} & a_{12} b_{22} \\
a_{21} b_{11} & a_{21} b_{12} & a_{22} b_{11} & a_{22} b_{12} \\
a_{21} b_{21} & a_{21} b_{22} & a_{22} b_{21} & a_{22} b_{22}
\end{pmatrix}.
$$

### CNOT Gate

CNOT gate: The value of the second qubit is flipped if and only if the value of the first qubit is 1. The application of a NOT gate on the second qubit (that we call the **target**) is **controlled** by the first qubit.

![CNOT gate](../quantum/images/CNOT.png)

Notice that the control qubit is indicated by a solid black circle and the target qubit is indicated by the $\oplus$ symbol (the symbol for an $X$ gate can also be used instead of $\oplus$).


$$
\text{CNOT} =
\begin{pmatrix}
1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & 0 & 1 \\
0 & 0 & 1 & 0
\end{pmatrix}.
$$

The truth table for the CNOT gate is:

$$
\begin{array}{|c|c||c|c|}
\hline
\text{Control Qubit} & \text{Target Qubit (Input)} & \text{Control Qubit (Output)} & \text{Target Qubit (Output)} \\
\hline
0 & 0 & 0 & 0 \\
0 & 1 & 0 & 1 \\
1 & 0 & 1 & 1 \\
1 & 1 & 1 & 0 \\
\hline
\end{array}
$$

### Swap 
![Swap gate](../quantum/images/swap.png)

In any case, the most prominent use of the CNOT gate is, without a doubt, the ability to create **entanglement**.

### Entanglement












---


Ref: A Practical Guide to Quantum Machine Learning and Quantum Optimization_ Hands-on Approach to Modern Quantum Algorithms